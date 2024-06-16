## 1、初识ES6

ES6的目标是使JavaScript 语言可以适应更复杂的应用，实现代码库之间的共享，并迭代维护新版本，成为企业级开发语言。

ECMAScript是JavaScript语言的国际标准，JavaScript是实现ECMAScript标准的脚本语言

## 2、let和const关键字

let关键字的新的特性：

- let关键字声明的变量只在所处的块级作用域有效。
- let关键字声明的变量不存在变量提升。
- let关键字声明的变量具有暂时性死区特性。

在使用var关键字声明变量时，变量可以先使用再声明，这种现象就是变量提升。用let 使得ES6变量使用更规范更严格

```vue
//暂时性死区演示
<script>
  var num = 10;
  if (true) {
    console.log(num);	// 报错，无法在初始化之前访问num
    let num = 20;
  }
</script>

```

const关键字声明的常量的特点：

- const关键字声明的常量具有块级作用域。

- const关键字声明常量时必须赋值。

- const关键字声明常量并赋值后常量的值不能修改。、

- 对于复杂数据类型（如数组、对象）虽然不能重新赋值，但是可以更改内部的值。

  ```vue
  <script>
    const ary = [100, 200];
    ary[0] = 'a';
    ary[1] = 'b';
    console.log(ary);  // 可以更改数组内部的值，结果为['a', 'b']
    ary = ['a', 'b’];    // 报错，无法对常量赋值
  </script>
  
  ```

## 3、解构赋值

#### 数组解构

```vue
<script>
  let [a, b, c, d] = [1, 2, 3];
  console.log(a);	// 输出结果：1
  console.log(b);	// 输出结果：2
  console.log(c);	// 输出结果：3
  console.log(d);	// 输出结果：undefined
</script>

```

#### 对象解构

```vue
<script>
  let person = { name: 'zhangsan', age: 20 };
  let { name, age } = person;	// 解构赋值
  console.log(name);	// 输出结果：zhangsan
  console.log(age);		// 输出结果：20
</script>

```

## 4、箭头函数

用于简化函数的定义语法

```vue
（）=> {}
```

因为箭头函数没有名字，我们通常做法是把箭头函数赋给一个变量，变量名就是函数名然后通过变量名去调用函数。

```vue
<script>
  const fn = () => {
    console.log(123);	// 输出结果：123
  };
  fn();		// 函数调用
</script>

```

箭头函数特点：

1. 省略大括号

2. 省略参数外部的小括号

3. 箭头函数中的this关键字

   ES6中，箭头函数不绑定this关键字，它没有自己的this关键字，如果在箭头函数中使用this关键字，那么this关键字指向的是箭头函数定义位置的上下文this。也就是说，箭头函数被定义在哪，箭头函数中的this就指向谁。

   ```vue
   <script>
     const obj = { name: 'zhangsan' };
     function fn() {
       console.log(this);    		// 输出结果：{name: "zhangsan"}
       return () => {
         console.log(this);  		// 输出结果：{name: "zhangsan"}
       };
     }
     // call()方法可以改变函数内部的this指向，将函数fn()内部的this指向obj对象
     const resFn = fn.call(obj);
     resFn();
   </script>
   
   ```

## 剩余参数

剩余参数是指在函数中，当函数实参个数大于形参个数时，剩余的实参可以存放到一个数组中。

```vue
<script>
  function sum(first, ...args) {
    console.log(first);   // 输出结果:10
    console.log(args);  // 输出结果:[20, 30]
  }
  sum(10, 20, 30);
</script>

```

```vue
<script>
  const sum = (...args) => {
    let total = 0;
    args.forEach((item) => {
      total += item;
    });
    // 等价于：args.forEach(item => total += item);
    return total;
  };
  console.log(sum(10, 20));       // 输出结果：30
  console.log(sum(10, 20, 30)); // 输出结果：60
</script>

```

```vue
<script>
  // 以数组的解构赋值为例
  let students = ['王五', '张三', '李四'];
  let [s1, ...s2] = students;
  console.log(s1);  // 输出结果：王五
  console.log(s2);  // 输出结果：["张三", "李四"]
</script>

```

## 6、扩展运算符

扩展运算符和剩余参数的作用是相反的，扩展运算符可以将数组或对象转换为用逗号分隔的参数序列。扩展运算符也用3个点“…”表示。

```vue
<script>
  let ary = [1, 2, 3];
  // ...ary相当于1, 2, 3
  console.log(...ary);	// 输出结果：1 2 3
  // 等价于
  console.log(1, 2, 3);	// 输出结果：1 2 3
</script>

```

