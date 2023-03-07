# 基本类型

| 类型          | 例子               | 描述                                        |
| ------------- | ------------------ | ------------------------------------------- |
| string        | let a:string = 'a' | 任意字符串类型                              |
| number        |                    | 任意数字类型                                |
| boolean       |                    | 布尔值 true\|false                          |
| 字面量        | 本身               | 限制变量的值就是该字面量的值                |
| any           |                    | 任意类型                                    |
| unkonwn       |                    | 表示类型安全的any                           |
| void          |                    | 没有返回值（或undefined）                   |
| never         | 没有值             | 没有值，不能是任何值                        |
| object        | {a: string}        | 对象类型                                    |
| array         | [1,2,3]            | 数字类型                                    |
| tuple（元组） | [1,2]              | typescript 新增类型，表示固定长度的数组类型 |
| enum（枚举）  | enum{A, B}         | typescript 新增类型，枚举类型               |

### any和unknown

* **any** 表示任意类型，一个变量类型设置为any时相当于关闭了typescript类型检测

* **unknown** 表示未知类型，实际上时一个类型安全的any

* 区别：any类型的变量能赋值给其他变量，unknown类型的变量不能赋值给其他变量

* unknown类型赋值问题可通过 **as(类型断言)** 解决，告诉解析器变量的类型

  * ```typescript
    let a: unkonwn
    a = 'AAA'
    
    let b:string
    b = a as string //或者
    b = <string>a
    ```



### object

类型定义时可以通过 **[propName: string]: any** ，来指定多个属性（propName可以为任意单词）

```typescript
let a: {name: string, [propName: string]: any}

a = {name: 'name', b: 'b', c: 123}
```



### array

数组的类型声明：

```typescript
let a: string[];
a = ['a', 'n']

let a = Array<number>
```



### **tuple元组**

表示固定长度的数组

```typescript
let a = [string, number]
a = ['a', 12]
```



### enum（枚举）

在多个值进行选择时适合用枚举

```typescript
enum Num {
    one,
    two
}
let a:Num
Num.one === 0
Num.two === 1

enum Num { //可设置枚举属性的值
    one = 1,
    two = 2,
}
```



### type类型别名

```typescript
type MyType = 1 | 2 | 3
type obj = {a: string} & {b: number}
obj = {a: 'a', b: 2}
```

* & 表示同时





# Class类

#### 修饰符

```javascript
Class Father {
    name: string; //默认属性
    static age: number = 12; //静态属性
    readonly money: number = 10000000000000 //只读属性
}

const son = new Fatcher('name', 21)
son.name // 'name'
Father.age // 12
```

| 修饰符                | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| pubilc（默认值）      | 修饰的属性可在任意位置访问、修改                             |
| private（私有属性）   | 修饰的属性只能在类的内部访问、修改<br />（可在类中添加方法返回私有属性，在外部调用该方法，使得私有属性在外部访问） |
| protected（保护属性） | 修饰的属性只能在当前类和子类中访问、修改                     |
| static（静态属性）    | 修饰的属性通过 new 创建的对象不能访问，只能通过 类 访问该属性 |
| readonly（只读属性）  | 修饰的属性只能访问无法修改                                   |



#### 构造函数和this

```javascript
Class Father {
    name: string; //声明
    age: number;
    
    consturctor(name: string, age: number) { //构造函数
        this.name = name;
        this.age = age
    }
    
    doSomeThing() {
        console.log("dosomething")
    }
}

const son = New Father('子', 18)
// new 关键字调用
//	1. 创建一个新的对象
//	2. 调用Class类的构造函数，将构造函数中的this指向创建对象
// 	3. 将实例对象__protot__指向构造函数的prototype原型
// 	4. 返会一个新的对象

son.name // 子
son.doSomeThing() //‘dosomething’
```

**new 关键字调用**

1. 创建一个新的对象
2. 将实例对象__protot__指向构造函数的prototype原型
3. 调用Class类的构造函数，将构造函数中的this指向创建对象
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。





#### extends 继承

```javascript
Class Father { //父类
    name: string;
    age: number;
    
    consturctor(name: string, age: number) { //构造函数
        this.name = name;
        this.age = age
    }
    
    doSomeThing() {
        console.log("dosomething")
    }
}

class Son extends Father { //子类
        doSomeThing() {
        console.log("son dosomething")
    }
}

```

1. 使用继承后，子类会拥有父类的所有属性和方法
2. 可在子类中添加一个新的属性和方法
3. 可父类对已有的方法进行重写覆盖(**方法重写**)



### super

1. 在子类中调用super方法表示当前类的父类
2. 在子类中的构造函数须通过super方法调用父类的构造函数，即可在子类的构造函数中使用父类的属性
3. 调用super.doSomeThing() 即表示调用父类的doSomeThing方法



### abstract（抽象类）

1. 抽象类不能用来创建对象
2. 抽象类是专门用来继承的类

**抽象方法**

```typescript
abstract class Father { //抽象类
    name: string;
    
    constructor(name: string) {
        this.name = name
    }
    
    abstract doSome(): void //抽象方法
}
```

* 抽象方法使用 abstract 定义，没有方法体
* 抽象方法智能在抽象类定义，子类必须对抽象方法进行重写



### interface 接口

```typescript
interface MyInterface {
    name: string;
    
    doSomething(): void;
}

class MyClass implements MyInterface {
    name: string;
    
    constructor(name: string) {
        this.name = name
    }
    
    doSomething() {}
}
```

* 接口只定义对象的结构，接口中所有属性都不能有实际的值
* 接口中所有的方法都是抽象方法，即定义类时需满足接口的结构需求





### 泛型

使用场景：在定义函数或类时，遇到类型不明确时可以使用泛型

```typescript
function fc<T, K>(a: T):K {
    return `${a}`
}

fc<number, string>(10) //指定泛型


interface Inter {
    a: string;
    b: number;
    c: {
        d: string
    };
    e?: [number|string]
}

function fc2<T extends Inter>(a: T):number { //泛型T继承Inter
    return a,b
}
fc2<Inter>({a: 'a', b: 2, c: {d: 'd'}, e: [1, 's']})


class MyClass<T> {
    name: T
}

const mycls = new MyClass<string>('hello')
```

* T extends Inter 表示泛型 T 必须是 Inter 实现类（子类）

