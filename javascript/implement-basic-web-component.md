# Implement basic web component

## 倒计时

### 基础idea

截止日期  - 当前时间  = 剩余 时间 （单位都是 毫秒）

余数的概念

* 我们 除以3 的话 我们 的余数就会从 3-1 到 0 之间开始循环

![](../.gitbook/assets/image%20%28101%29.png)

{% embed url="https://jsfiddle.net/Boyanliuu/fno41mkt/21/" %}

```text
  //先转换成秒， 除以1000然后 用余数算出来当前是多少秒
  second = Math.floor(timeRemaining/ 1000 % 60) ;
  //先转换成秒， 除以1000然后转换成 分钟 除以60 用余数算出来当前是多少分钟
  minute = Math.floor(timeRemaining/ 1000 / 60 %  60) ;
    //先转换成秒， 除以1000然后转换成 分钟 除以60 然后再除以60 转换成 小时 然后 用余数算出来是多少小时
  hour = Math.floor(timeRemaining/ 1000 / 60 / 60 % 24);
  day = Math.floor(timeRemaining/ 1000 / 60 / 60 /24);
  

```



