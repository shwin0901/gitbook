# 修饰器

修饰器（Decorator）是ES7的一个提案，它是用于对类和类的方法进行处理的函数

### 类的修饰

**添加静态属性**

```javascript
@fc1
class MyClass {}

function fc1(traget) {
    traget.name = "observable"
}

console.log(MyClass.name) // "observable“
```

```javascript
@fc2(100)
class MyClass {}

function fc2(value) {
    return function(traget) {
     	traget.name = value   
    }
}

console.log(MyClass.name) // 100
```

* fc1是一个修饰器以@符号开头，对 MyClass 进行了修改，添加了一个 name 属性并赋值为 observable；

* fc1第一个参数是所修饰 MyClass 这个类，如果需要传入更多参数，则需要在修饰器外再写一层函数，例：fc2；



**添加实例属性**

```javascript
@fc3
class MyClass{}

function fc3(traget) {
    traget.prototype.testable = true
}

const myClass = new MyClass()
console.log(myClass.testable) // true
```

* 添加实例属性，则需要对类的prototype（原型）对象添加属性



### 类的属性修改

```javascript
class MyClass {
    @noEnumerable name = "observable"
    
    @readonly too() {
        return "Hello World"
    }
}
    
function noEnumerable(traget, name, description) {
    description.enumerable = false
	console.log(description)
    // configurable: true
	// enumerable: true
	// initializer: ƒ ()
	// writable: true
}
    
function readonly(traget, name, description) {
    description.writeable = false
}

const myClass = new MyClass()
myClass.too = () => {
    console.log("hello, mobx")
} //这里会报错不能修改

```

* 属性修饰器参数
  * traget ：目标类（MyClass）的 prototype
  * nam：被修饰的类的成员名称
  * description：被修饰的类成员的描述对象
    * configurable：是否支持再次进行配置
    * enumerable：是否允许被遍历
    * initializer：初始化函数
    * writable：是否允许被修改



注：修饰器只能用于类和类的方法，不能用于函数，因为存在函数提升。类是不会提升



### 关于vscode使用修饰器语法报错

* 在搜索设置输入 experimentalDecorators 
* 找到 JS/TS › Implicit Project Config: Experimental Decorators 选项勾选



### 关于 React 项目中不支持修饰器

* 安装装饰器驱动插件

  ```
  npm install @babel/plugin-proposal-decorators
  ```

* 在 package.json 文件添加配置

  ```json
  "babel": {
      "plugins": [
        [
          "@babel/plugin-proposal-decorators",
          {
            "legacy": true
          }
        ]
      ],
      "presets": [
        "react-app"
      ]
  }
  ```

* 或者在 babel.config.js 文件中添加配置 

  ```javascript
  module.exports = {
    // https://github.com/facebook/create-react-app/blob/main/packages/babel-preset-react-app/create.js
    // babel-preset-react-app
    presets: ["react-app"],
    // presets: ["@babel/preset-env"],
    plugins: [
      [
        "@babel/plugin-proposal-decorators",
        {
          "legacy": true
        }
      ]
    ]
  };
  ```

  

