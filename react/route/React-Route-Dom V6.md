# React-Route-Dom V6

#### 1. Routes 替换 Swtich

```react
import { Routes } from  'react-route-dom';

function App() {
    return <>
        <Routes>
        	<Route path="/" element={<Home/>} />	
        </Routes>
    </>
}
```



#### 2. Route变化

V6不再支持render和 Compoent，改为element

```react
<Routes>
	<Route path="/" element={<Home/>} />	
</Routes>
```

**注：element的值为组件标签，Route标签必须包含在Routes标签里**



#### 3. exact 属性

v6 内部算法改变，不再需要加exact实现精确匹配路由，默认就是匹配完整路径。如果需要旧的行为，路径后加/*

```react
<Route path="/home/*" element={<Home />} /> 
```



#### 4.Route先后顺序不受限，React Route 自动找出最优匹配路径



#### 5. v6 保留Link，NavLink， 但是NavLink的activeClassName属性被移除

```rea	
<NavLink className={({ isActive }) => isActive ? "red" : "blue"} />
```



#### 6.嵌套路由改为相对匹配

嵌套路由必须放在<Routes> </Routes>中，且使用相对路径，不再像 v5 那样必须提供完整路径，因此路径变短

```react
<Routes>
    <Route path="/home" element={<Home/>}>
    	<Route index element={<Home2/>} />
        <Route path="a" element={<A/>} />
        <Route path="b" element={<b/>} />
    </Route>
</Routes>
```



#### 7. Outlet 组件占位符（嵌套路由的位置）

```react
function App() {
    return <div>
        <h1>嵌套路由放下面</h1>
        
        // 相当去props.childrens
        <Outlet/>
    </div>
}
```



#### 8. 使用index 指定默认路由，或者path为空

当嵌套路由有多个子路由的时候，可以增加 index 属性来指定默认路由。

```react
// 使用index
<Routes>
    <Route path="/" element={<Layout />}>
    	<Route index element={<Home/>} />
        <Route path="user" element={<User/>} />
    </Route>
</Routes>

// path为空
const routes = [{
    path: '/',
    element: <Layout/>,
    children: [
        {
            path: "",
            element: <Home/>
        },
        {
            path: "user",
            element: <User/>
        }
    ]
}]
```



#### 9. 用useNavigate实现编程式导航，useHistory被移除

```react
import { useNavigate } from 'react-route-dom';

const navigate = useNavigate()

navigte("/home");  //push 路由跳转
navigte("/user", {replace: true}) //路由重定向
```



#### 10. 通过hooks获取路由参数方法

* **useParams** 返回当前动态路由参数

  ```react
  import { useParams } from 'react-route-dom'
  
  function App() {
      const params = useParams()
      console.log(params.id)
  }
  ```



* **useSearchParams**

​	UseSearchParams hooks 用于读取和修改 URL 中当前位置的查询字符串（/xxx/xxx?userId=1000）。与  useState 用法一样，useSearchParams 返回一个由两个值组成的数组: 当前位置的搜索参数和一个可用于更新它们的函数。

**更改 searchParams 时，必须传入所有的查询参数，否则会覆盖已有参数。**

```react
import { useSearchParams } from "react-router-dom";

function App() {
  let [searchParams, setSearchParams] = useSearchParams();
}
```



* **useRoutes** 

​	声明式路由，不能写index，可用path："" 显示默认组件，useRoutes 的返回值是一个有效React元素，可以用来呈现路由树，如果没有匹配返回null

```react
import { BrowserRouter, useRoutes } from "react-router-dom";

function RouteElemet() {
  let element = useRoutes([
    {
      path: "/",
      element: <Dashboard />,
      children: [
        {
          path: "messages",
          element: <DashboardMessages />,
        },
        { path: "tasks", element: <DashboardTasks /> },
      ],
    },
    { path: "team", element: <AboutPage /> },
  ]);

  return element;
}

function App() {
    return (
      <BrowserRouter>
        <RouteElemet />
      </BrowserRouter>
}
```

**注：**

**`userRoutes`本身现在只需要BrowserRouter进行包裹不像以前需要`<Routes />`组件进行包裹，否则会报错**



#### 11. [MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)

* 将内部路由存在数组中，并能指定初始路由

```jsx
import { MemoryRouter, Router, Route } from 'react-router-dom';

const App = () => (
	<MemoryRouter initialEntries={["/page"]}>
    	<Router>
        	<Route path="/home" element={<Home/>} />
            <Route path="/page" element={<page/>} />
        </Router>
    </MemoryRouter>
)

const App = () => (
	<MemoryRouter 
    	initialEntries={[{
            pathname: "/page",
            state: {},
            search: '',
            hash: '',
            key: 'default',
        }]}
    >
    	<Router>
        	<Route path="/home" element={<Home/>} />
            <Route path="/page" element={<page/>} />
        </Router>
    </MemoryRouter>
)
```

