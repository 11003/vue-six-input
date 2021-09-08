# vue-six-input

> input分段输入框，适用于6位数、4位数等

## 参数

| 参数 | 类型 | 备注 | 必须 | 默认值 |
| ------ | ------ | ------ | ------ | ------ |
| inputType | String | 输入类型 | 否 | 'number'（'number'/'password'） |
| inputNumber | Number | 验证码长度 | 否 | 6 |
| completed | event | 输入框结果回调 | - | - |

## 示例

### number类型

![](https://gitee.com/liuhaier/images/raw/master/img/six-input-number.gif)
```html
 <NumberVerify @completed="completed"/>
 ```

### password类型

![](https://gitee.com/liuhaier/images/raw/master/img/six-input-password.gif)
 ```html
 <NumberVerify input-type="password" @completed="completed"/>
 ```

### 回调结果

 ```js
completed(v) {
      console.log("值为：",v)
}
 ```
