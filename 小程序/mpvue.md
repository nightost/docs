# mpvue
## style
写法（sass ）
## 原生组件与自定义组件样式
定义了theme文件夹，用来重置元素组件样式
其余组件统一使用vue编写，方便以后向web扩展
## 小图标
iconfont
iconfont的更新较其他项目复杂一些
## 静态图片
考虑大小的问题，后期考虑图片服务器，减小小程序包大小
## api
小程序内分为api(互联网请求)，localApi(本地微信接口，摄像头，分享，本地存储等)
当然api内的request也不是web所用的ajax类库，而是微信提供的接口，也会定义在localApi
## 顶层数据
vuex(暂时还没想到哪些数据存储在vuex)
## 路由
由原生的navigateTo提供
## 生命周期
### app 部分：
1. onLaunch，初始化
2. onShow，当小程序启动，或从后台进入前台显示
3. onHide，当小程序从前台进入后台
### page 部分：
1. onLoad，监听页面加载
2. onShow，监听页面显示
3. onReady，监听页面初次渲染完成
4. onHide，监听页面隐藏
5. onUnload，监听页面卸载
6. onPullDownRefresh，监听用户下拉动作
7. onReachBottom，页面上拉触底事件的处理函数
8. onShareAppMessage，用户点击右上角分享
9. onPageScroll，页面滚动
10. onTabItemTap, 当前是 tab 页时，点击 tab 时触发 （mpvue 0.0.16 支持）
## 函数增强
lodash 按需加载
## 时间增强
[最小化moment](https://github.com/moment/moment/issues/2373)
## 金钱计算
moneyUtils 已添加mixin
## promise
封装了新的request，可以支持promise
## 分包
暂时没考虑到哪些，后面会使用webpack做分离
## 备注
>微信小程序的页面的 query 参数是通过 onLoad 获取的，mpvue 对此进行了优化，直接通过this.$root.$mp.query 获取相应的参数数据，其调用需要在 onLoad 生命周期触发之后使用，比如 onShow 等
>v-for="(item, index) in list" :key="item.name" 必须绑定key
## 参考
1. [https://segmentfault.com/a/1190000014984581](https://segmentfault.com/a/1190000014984581)
2. [http://www.cnblogs.com/calamus/p/8693980.html](http://www.cnblogs.com/calamus/p/8693980.html)