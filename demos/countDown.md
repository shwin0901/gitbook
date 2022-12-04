### 实现活动页面倒计时

```javascript
// 比如，
// 还有 60s 的时候，打印：00:01:00
// 还有 59s 的时候，打印：00:00:59

const ts = Date.now() + 1000 * 60;

const countdown = (expireTS) => {
  // code here
  let timer = null; // 记录定时器
  const nowTime = new Date().valueOf();
  let time = expireTS - nowTime; // 差值

  function init(time) {
    let h = Math.floor(time / (1 * 60 * 60 * 1000) % 24) // 把差值转换成小时，按小时单位取余
    let m = Math.floor(time / (60 * 1000) % 60) //按分钟单位取余
    let s = Math.floor(time / 1000 % 60) //按秒单位取余
    h = h < 10 ? `0${h}` : h;
    m = m < 10 ? `0${m}` : m;
    s = s < 10 ? `0${s}` : s;
      
    console.log(`${h}:${m}:${s}`) //打印结果
  }

  timer = setInterval((() => {
    time = time - 1000;
    if(time >= 0) {
      init(time)
    } else {
      clearInterval(timer)
    }
  }), 1000)

  init(time);
};

countdown(ts);
```

