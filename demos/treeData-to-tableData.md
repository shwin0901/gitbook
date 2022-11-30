### treeData (树结构数据) 转成 tableData (列表数据)

```javascript
// 使用递归遍历，convert的str参数记录父级的key值
function convert(treeData, str = "") {
    const tableData = {};
    for(let [key, value] of Object.entries(treeData)) {
       if(value instanceof Object) {
            let objKey = str ? `${str}.${key}` : key
            const tableObj = convert(value, objKey);
            Object.assign(tableData, tableObj)
        } else {
            let objKey = str ? `${str}.${key}` : key
            tableData[objKey] = value
        }
    }
    return tableData
}

const tree = {
  a: {
    b: {
      d: {
        e: '1',
      },
      f: '2'
    },
    c: {
      g: '3',
      h: '4'
    }
  },
  j: '5'
}

convert(tree)
// {
//	a.b.d.e: "1",
//	a.b.f: "2",
//	a.c.g: "3",
//	a.c.h: "4",
//	j: "5",
//}

```

