# vue-six-input

> input分段输入框，仅适用于6位数

## 参数

| 参数 | 类型 | 备注 | 必须 | 默认值 |
| ------ | ------ | ------ | ------ | ------ |
| completed | event | 结果回调，表示输入完成 | - | - |

## 示例

![](https://gitee.com/liuhaier/images/raw/master/img/six-input-password.gif)
```html
 <NumberVerify @completed="completed"/>

 ```

### 回调结果

 ```js
completed(v) {
      console.log("值为：",v)
}
 ```
