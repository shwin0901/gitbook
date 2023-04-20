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



##### 阻止浏览器鼠标右键弹出列表

```javascript
document.addEventListener("contextmenu", function(e) {
    e.preventDefault(); //阻止鼠标右键默认行为
    
    // 自定义元素替换默认鼠标右键列表
    element.style.left = e.pageX + "px";
    element.style.top = e.pageY + "px";
})
```



