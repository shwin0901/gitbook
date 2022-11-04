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