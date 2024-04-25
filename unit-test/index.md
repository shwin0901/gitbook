### 单元测试

#### 1. 技术栈

* jest
* testing-library/react

### 2. 命令行

```json
// package.json
{
    "nocache": "jest --no-cache", //清除缓存
    "watch": "jest --watchAll", //实时监听
    "coverage": "jest --coverage", //生成覆盖测试文件
    "verbose": "npx jest --verbose", //显示测试描述
}

//终端运行单个测试文件
jest -- '运行文件的相对位置' --projects '运行jest的配置文件位置'
例：
jest -- './src/test/add.test.jsx' --projects './src/test/test.jest.config.js'
```

#### 3. 用到的API

* #### jest
  
  * describe
  
  * test / it （测试语句）
    
    ```js
    describe('test examplate', () => {
        test('the can', () => {
            expect(0.1 + 0.2).toBeCloseTo(0.3) // 浮点数判断相等
        })
        it('the can', () => {
            expect(0.1 + 0.2).toBeCloseTo(0.3) // 浮点数判断相等
        }, 3000) //3000表示超时时间
    })
    ```

* #### testing-library
  
  * render: 渲染组件
    
    ```jsx
    import { fireEvent, screen, cleanup } from '@testing-library/react'
    
    const component = (
        <div>
            <span data-testid="componentTestId">jest</span>
            <button onClick={() => console.log('click')}>click</button>
        </div>
    )
    test('render component', () => {
        const { container, getByTestId } = render(component);
        expect(container).toMatchSnapshot();
    
        const span = getByTestId('componentTestId');
        expect(span).toBeInTheDocument();
    
        const btn = screen.getByText('click');
        expect(btn).toBeInTheDocument();
        fireEvent.click(btn); // 触发button点击事件
    })
    ```

* cleanup：将组件在容器中卸载并销毁容器

* act: [介绍](https://github.com/threepointone/react-act-examples/blob/master/sync.md)

* beforeEach: 在每次test前触发的回调函数

* afterEach: 在每次test后触发的回调函数（**一般清除mock的缓存 jest.clearAllMocks(); | cleanup()**）

* fireEvent: 处理render页面的event事件

* waitFor: 如果需要等待一段时间（接口API调用），等待回调函数处理完成
  
  ```js
  await waitFor(() => expect(mockAPI).toHaveBeenCalledTimes(1))
  ```

* userEvent: 处理页面事件 [doc](https://testing-library.com/docs/ecosystem-user-event/);

* scrren: 包含预先绑定到 document.body 的所有查询[doc](https://testing-library.com/docs/queries/about/#screen);

* #### Mock
  
  ```jsx
  import { render, screen } from '@testing-library/react';
  import { getList, getUser } from 'src/api'
  
  jest.mock('src/api');
  jest.mocked(getList).mockResolvedValue({
      status: 'success',
      payload: [],
  });
  (getUser as jest.Mock).mockReturnValue({
      payload: {},
  })
  
  // mock项目依赖里方法
  jest.mock('react-router-dom', () => ({
      useNavigate: () => jest.fn(),
      useLocation: () => ({state: {a: 1}})
  })))
  
  // mock指定文件里的方法的返回值
  jest.mock('./src/utils', () => ({
     hanldReturn: () => ({a: 1, b: 2}); 
  }))
  
  // mock APi接口的返回值
  jest.mock('./src/api.js', () => ({
      getListApi: () => Promise.resolve({
          code: 200,
          status: 'success',
          data: [{a: 1}, {b: 2}],
      })
  }))
   // 或者
   import { getListApi } from './src/api.js';
      jest.mock('./src/api');
      describe('mock api', () => {
          getListApi.mockResolvedValue([{a: 1}, {b: 2}]),
          test('mock api', async () => {
              const value = await getListApi();
              expecrt(value).toBe([{a: 1}, {b: 2}]);
          })
      })
  
  
  // mock 组件
  import Details from 'src/pages/components/Details';
  const Element = (props) => (<div>
      <Details {...props}/>
  </div>)
  
  const mockDetails = jest.fn();
  jes.mock('src/pages/components/Details', () => (props) => {
      mockDetails(props);
      return <div>Details</div>
  })
  
  test('render chilren component', () => {
      render(<Element a={1} b={'2'} />);
      expect(mockDetails).toBeCalledWith({a:1, b:'2'});
      expect(screen.getByText('Details')).toBeInTheDocument();    
  })
  ```








