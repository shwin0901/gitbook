### 数组均匀分布

>数组分成 **n** 份，每一份和尽量相等

```js
const r = [22, 11, 33, 82, 42, 56, 13, 8];

// params: 数组，n: 份数
function onToNArr(params, n) {
    let result = Array.from({length: n}, () => ({num: 0, item: []}));
    let arr = params.sort((a, b) => b - a);
    arr.forEach(() => {
        let min = result.sort((a, b) => a.num - b.num)[0];
        min.num += i;
        min.item.push(i)
    })
	return result;
}

```

