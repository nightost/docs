# wepy
## 注意事项
1. 变量与方法尽量使用驼峰式命名，并且注意避免使用$开头。 以$开头的标识符为WePY框架的内建属性和方法，可在JavaScript脚本中以this.的方式直接使用，具体请[参考API文档](https://tencent.github.io/wepy/document.html#/api?id=api)
## 语法
### 事件
> 原 bindtap="click" 替换为 @tap="click"
> 原catchtap="click"替换为@tap.stop="click"。
> 原 capture-bind:tap="click" 替换为 @tap.capture="click"
> 原capture-catch:tap="click"替换为@tap.capture.stop="click"。
>事件传参使用优化后语法代替。 原bindtap="click" data-index={{index}}替换为@tap="click({{index}})"
### wpy
1. 其中，小程序入口文件app.wpy不需要template，所以编译时会被忽略。.wpy文件中的script、template、style这三个标签都支持lang和src属性，lang决定了其代码编译过程，src决定是否外联代码，存在src属性且有效时，会忽略内联代
2. 入口文件app.wpy中所声明的小程序实例继承自wepy.app类，包含一个config属性和其它全局属性、方法、事件。其中config属性对应原生的app.json文件，build编译时会根据config属性自动生成app.json文件，如果需要修改config中的内容，请使用微信提供的相关API。

### 各标签对应的lang值如下表所示
|标签 | lang默认值 | lang支持值|
|    ----       | ------- | ------- |
| style       | css | css、less、scss、stylus、postcss | 
| template |	wxml | wxml、xml、pug(原jade) | 
| script | babel  | 	babel、TypeScript | 

### 页面文件page.wpy中所声明的页面实例继承自wepy.page类，该类的主要属性介绍如下
|属性 | 说明|
|   ---- | ------- | 
| config | 页面配置对象，对应于原生的page.json文件，类似于app.wpy中的config |
| data | 页面渲染数据对象，存放可用于页面模板绑定的渲染数据 |
| components | 页面组件列表对象，声明页面所引入的组件列表 |
| methods | methods	wxml事件处理函数对象，存放响应wxml中所捕获到的事件的函数，如bindtap、bindchange |
| events | WePY组件事件处理函数对象，存放响应组件之间通过$broadcast、$emit、$invoke所传递的事件的函数 |
| 其它 | 小程序页面生命周期函数，如onLoad、onReady等，以及其它自定义的方法与属性 |

### 实例继承
```javascript
import wepy from 'wepy';

// 声明一个App小程序实例
export default class MyAPP extends wepy.app {
}

// 声明一个Page页面实例
export default class IndexPage extends wepy.page {
}

// 声明一个Component组件实例
export default class MyComponent extends wepy.component {
}
```

### page
```javascript
import wepy from 'wepy';

// export default class MyPage extends wepy.page {
export default class MyComponent extends wepy.component {
    customData = {}  // 自定义数据

    customFunction ()　{}  //自定义方法

    onLoad () {}  // 在Page和Component共用的生命周期函数

    onShow () {}  // 只在Page中存在的页面生命周期函数

    config = {};  // 只在Page实例中存在的配置数据，对应于原生的page.json文件

    data = {};  // 页面所需数据均需在这里声明，可用于模板数据绑定

    components = {};  // 声明页面中所引用的组件，或声明组件中所引用的子组件

    mixins = [];  // 声明页面所引用的Mixin实例

    computed = {};  // 声明计算属性（详见后文介绍）

    watch = {};  // 声明数据watcher（详见后文介绍）

    methods = {};  // 声明页面wxml中标签的事件处理函数。注意，此处只用于声明页面wxml中标签的bind、catch事件，自定义方法需以自定义方法的方式声明

    events = {};  // 声明组件之间的事件处理函数
}
```
>注意，对于WePY中的methods属性，因为与Vue中的使用习惯不一致，非常容易造成误解，这里需要特别强调一下：WePY中的methods属性只能声明页面wxml标签的bind、catch事件，不能声明自定义方法，这与Vue中的用法是不一致的
```js
// 正确示例

import wepy from 'wepy';

export default class MyComponent extends wepy.component {
    methods = {
        bindtap () {
            let rst = this.commonFunc();
            // doSomething
        },

        bindinput () {
            let rst = this.commonFunc();
            // doSomething
        },
    }

    //正确：普通自定义方法在methods对象外声明，与methods平级
    customFunction () {
        return 'sth.';
    }

}
```
### 组件单实例
需要注意的是，WePY中的组件都是静态组件，是以**组件ID**作为唯一标识的，每一个ID都对应一个组件实例，当页面引入两个相同ID的组件时，这两个组件共用同一个实例与数据，当其中一个组件数据变化时，另外一个也会一起变化。
如果需要避免这个问题，则需要分配多个组件ID和实例。代码如下：

```js
<template>
    <view class="child1">
        <child></child>
    </view>

    <view class="child2">
        <anotherchild></anotherchild>
    </view>
</template>


<script>
    import wepy from 'wepy';
    import Child from '../components/child';

    export default class Index extends wepy.component {
        components = {
            //为两个相同组件的不同实例分配不同的组件ID，从而避免数据同步变化的问题
            child: Child,
            anotherchild: Child
        };
    }
</script>
```
>注意：WePY中，在父组件template模板部分插入驼峰式命名的子组件标签时，不能将驼峰式命名转换成短横杆式命名(比如将childCom转换成child-com)，这与Vue中的习惯是不一致。
### 计算属性
**需要注意的是，只要是组件中有任何数据发生了改变，那么所有计算属性就都会被重新计算**

### WePY数据绑定方式
WePY使用脏数据检查对setData进行封装，在函数运行周期结束时执行脏数据检查，一来可以不用关心页面多次setData是否会有性能上的问题，二来可以更加简洁去修改数据实现绑定，不用重复去写setData方法。代码如下：

```js
this.title = 'this is title';
```
需注意的是，在异步函数中更新数据的时候，必须手动调用$apply方法，才会触发脏数据检查流程的运行。如：

```js
setTimeout(() => {
    this.title = 'this is title';
    this.$apply();
}, 3000);
```