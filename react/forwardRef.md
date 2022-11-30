### React父组件调用子组件中的方法

#### [React.forwardRef](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref)

描述：

* [React.forwardRef]() 会创建一个接受 `ref` 属性的React组件;
* `React.forwardRef` 接受渲染函数作为参数。React 将使用 `props` 和 `ref` 作为参数来调用此函数;



#### [useImperativeHandle](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)

描述：

* `useImperativeHandle hooks ` 可以在使用 `ref` 时自定义暴露给父组件的实例值；
* `useImperativeHandle` 应当与 [`forwardRef`](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref) 一起使



```javascript
// 父组件
import { useRef } from 'react'
import Child from './Child'
function App() {
    const childRef = useRef(null);
    
    const getChildMethod =  () => {
        childRef.current.sendMessage(); // "子组件的方法"
    }
    
    retrun <>
		<Child ref={childRef} />
    	<button onClick={getChildMethod}>获取子组件的方法</button>
	</>
}


// 子组件
import { forwardRef, useImperativeHandle } from 'react'

const Child = forwardRef((props, ref) => { //使用forwardRef创建一个有ref参数的react组件
    
    useImperativeHandle(ref, () => ({ // 暴露给父组件的ref实例
        sendMessage,
    }))
    
    const sendMessage = () => {
        console.log("子组件的方法")
    }
    
    return <>
    	<p>child component</p>
    </>
})

export default Child
```

