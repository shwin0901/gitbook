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
    const component = (
        <div>
        	<span data-testid="componentTestId">jest</span>
        </div>
    )
    test('render component', () => {
        const { container, getByTestId } = render(component);
        expect(container).toMatchSnapshot();
        
        const span = getByTestId('componentTestId');
        expect(span).toBeInTheDocument();
    })
    ```

    

  * cleanup：将组件在容器中卸载并销毁容器

  * act: [介绍](https://github.com/threepointone/react-act-examples/blob/master/sync.md)

  * fireEvent: 处理render页面的event事件

  * waitFor: 如果需要等待一段时间（接口API调用），等待回调函数处理完成

    ```js
    await waitFor(() => expect(mockAPI).toHaveBeenCalledTimes(1))
    ```

  * userEvent: 处理页面事件 [doc](https://testing-library.com/docs/ecosystem-user-event/);

  * scrren: 包含预先绑定到 document.body 的所有查询[doc](https://testing-library.com/docs/queries/about/#screen);

* #### Mock

  ```js	
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



