### tableData (树结构数据) 转成 treeData (列表数据)

```js
  const entry = {
    "a.b.d.e": "1",
	  "a.b.f": "2",
	  "a.c.g": "3",
	  "a.c.h": "4",
	  j: "5",
  }
  
  function fnc(entry) {
    const keys = Object.keys(entry);
    const obj = Object.create(null);
    for(let item of keys) {
      const arr = item.split('.');
      arr.reduce((pre, nex, index) => {
        if(index === arr.length - 1) {
          pre[nex] = entry[item];
          return;
        }
        pre[nex] = pre[nex] || {};
        return pre[nex]
      }, obj)
    }
  }

  console.log(fnc(entry))
```