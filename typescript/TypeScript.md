# TypeScript



### Class类

#### 属性

```javascript
Class Father {
    name: string; //实例属性
    static age: number = 12; //静态属性
    readonly money: number = 10000000000000 //只读属性
}

const son = new Fatcher('name', 21)
son.name // 'name'
Father.age // 12
```

1. **实例属性**：直接定义的属性，需要通过对象的实例去访问可 **读、写**；
2. **static**：为**静态属性**（类属性），new 关键字创建的对象不能访问该属性，只能通过类 Father 访问该属性
3. **readonly**：**只读属性无法修改**



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

class Son entends Father { //子类
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
