# mobx

### observable

Observable 值可以是JS基本数据类型、引用类型、普通对象、类实例、数组和映射

```javascript
class MyClass {
    @observable count = 0;
}

const myClass = new MyClass()

console.log(myClass.count)  // 0

```



### 关于mobx6属性不更新问题

通过`@observable`修饰的属性变更后，视图并不刷新，`autorun`无法自动触发的问题

* 在不做改变的前提下，class中添加`makeObservable`，同时使用makeObservable包裹要监听的对象（我们项目中是对整个class对象设置成observable，因此在构造函数内使用了`makeObsevable(this)`）

```javascript
import { observable, makeObservable } from "mobx"

class MyClass {
    @observable count = 0;
    constructor() {
        makeObservable(this)
    }
}
```



### computed

计算值(computed values)是可以根据现有的状态或其它计算值衍生出的值

```javascript
class MyClass {
    @observable count = 10;
    @observable pirce = 5;
    @computed get totalPirce() {
        return this.count * this.pirce
    }
}

const myClass = new MyClass()

console.log(myClass.totalPirce)  // 50
```

* computed get 设置的 totalPirce 方法变更之后只会执行一次
* computed get 设置的 totalPirce 方法放回的值当成对象属性访问



### action

用来修改状态的属性

* #### **@action**

  * 通过实例对象修改状态会触发多次更新（autorun），通过@action fuc() {}只会触发一次更新

  * 通过 **configure** 开启严格模式，开启只能通过action定义的方法进行状态更新否则会报错

    ```javascript
    import { observable, action, autorun, makeObservable, configure } from 'mobx'
    
    configure({
        enforceActions: "observed" //开启严格模式
    })
    class MyClass {
        @observable count = 10;
        @observable pirce = 5;
        
        constructor() {
            makeObservable(this)
        }
        
        @action change() {
            this.count = 11
            this.pirce = 6
        }
    }
    
    const myClass = new MyClass()
    
    autorun(() => {
        console.log("autorun", store.count)
    })
    ```

    

* #### **@ation.bound**

  * `action.bound` 可以用来自动地将动作绑定到目标对象(**绑定this**)。 注意，与 `action` 不同的是，`(@)action.bound` 不需要一个name参数，名称将始终基于动作绑定的属性。

    ```javascript
    import { observable, action, autorun, makeObservable, configure } from 'mobx'
    
    configure({
        enforceActions: "observed" //开启严格模式
    })
    class MyClass {
        @observable count = 10;
        @observable pirce = 5;
        
        constructor() {
            makeObservable(this)
        }
        
        @action.bound change() {
            this.count++
        }
    }
    
    const myClass = new MyClass()
    setTimeOut(myClass.change, 1000)
    
    console.log(store.count) //11
    ```

    

* #### runInAction(name?, thunk)

  `runInAction` 是个简单的工具函数，它接收代码块并在(异步的)动作中执行。这对于即时创建和执行动作非常有用，例如在异步过程中。`runInAction(f)` 是 `action(f)()` 的语法糖。

  ```javascript
  import { runInAction } from 'mobx'
  
  class MyClass {
      @observable count = 10;
      @observable pirce = 5;
      
      constructor() {
          makeObservable(this)
      }
  }
  
  const myClass = new MyClass()
  
  runInAction(() => {
      myClass.count++
  })
  
  console.log(store.count) //11
  ```



### 异步 action

异步调用, 不能直接在定义的异步函数里修改状态，需要额外定义函数

* **定义 action 函数 (需要比较复杂的计算量，但需要额外定义函数)**

  ```javascript
  class MyClass {
      @observable count = 10;
      @observable pirce = 5;
      
      constructor() {
          makeObservable(this)
      }
      
      @action.bound asyncChange() {
          setTimeOute(() => {
              this.change()
          }, 1000)
      }
      
      @action.bound change() {
          this.count = 111
      }
  }
  ```

  

* **直接调用 action 函数 （需要额外定义函数，在异步函数里直接定义）**

  ```javascript
  class MyClass {
      @observable count = 10;
      @observable pirce = 5;
      
      constructor() {
          makeObservable(this)
      }
      
      @action.bound asyncChange() {
          setTimeOute(() => {
              action('changeCount', () => {
                  this.count = 123
              })
          }, 1000)
      }
  }
  ```

  

* **调用 runInAction （适用于简单计算，不需要额外定义函数）**

  ```javascript
  class MyClass {
      @observable count = 10;
      @observable pirce = 5;
      
      constructor() {
          makeObservable(this)
      }
      
      @action.bound asyncChange() {
          setTimeOute(() => {
             runInAction(() => {
                 this.count = 1234
             })
          }, 1000)
      }
  }
  ```

  

### Reactions（监听数据修改的方式）

Reactions 用来对状态改变自动发生的副作用进行建模

* **autorun**：
  * 在被其观察的任意一个值发生改变时重新执行一个函数
  * 默认会执行一次，当内部所观测的发生改变时会重新触发
* **reaction**
  * 有两个参数都是函数，第一函数返回值做为第二个函数的参数使用
  * 第二个函数为data, reaction
    * data 为第一个函数的返回值
    * reaction为当前函数本身，调用reaction.dispose()，手动停止当前 reaction 的监听
* **when**
  * 有两个参数都是函数，第一个函数返回为true的时候，第二个函数才会执行
  * 只会生效一次，下次所依赖的数据还是被满足时不会执行

```javascript
import {makeObservable, autorun, when, reaction } from 'mobx'

class MyClass {
    @observable count = 10;
    @observable pirce = 5;
    
    constructor() {
        makeObservable(this)
    }
    
    @action.bound change() {
        this.count = 20
    }
    
    @action.bound changeWhen(value = 5) {
        this.pirce = value
    }
}

const myClass = new MyClass()

autorun(() => {
    console.log("autorun:", myClass.count)
})

myClass.change() // 调用之后，autorun还会执行


when(() => {
    return myClass.pirce > 10
}, () => {
    console.log("when:", myClass.pirce)
})
myClass.changWhen(11) //调用之后，when第一个函数返回值满足为true，则第二个函数会执行
myClass.changWhen(12) //后续的调用即使满足也不会执行when

reaction(() => {
    return myClass.pirce
}, (data, reaction) => {
    console.log("reaction:", data) //为第一个函数的返回值
    if(myClass.pirce > 10) {
        reaction.dispose() //当前pirce大于10，就不再循环调用
    }
})
```

