##### 获取浏览器地址栏 url search

```js
const getSearchValue = (search, key) => {
  const searchPar = new URLSearchParams(search || window.location.search)
  const paramsObj = {}
  for (const [key, value] of searchPar.entries()) {
    paramsObj[key] = value
  }
  return paramsObj[key]
}
```



##### 正则校验前后数据

```js
const str = '401815708';
/^4.*8$/gi.test(str); // true
/^1.*8$/gi.test(str); // false

// 正则：^表示开头（^1, ^2），
// $表示结尾（2$, 1$），
// .* 与
```