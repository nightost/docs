#ie vue bugs
## Bug
在ie中，如果table内
```html
  <table>
    <tr v-for="item in list"><tr>
  </table>
```
如果list为空，ie可能出现崩溃
## Solution

需要加入tbody
```html
<table>
    <tbody>
      <tr v-for="item in list"><tr>
    </tbody>
  </table>
```