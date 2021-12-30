title: JavaScript 中常用的方法等
date: 2021-4-20 14.37  
modified: 2021-4-20 14.37
tag:

- javascript

photos:

- https://images.unsplash.com/photo-1543169564-be8896b30cdb?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80

---

JavaScript 中常用的方法汇总

<!--more-->

##### object.keys

遍历对象的属性名返回一个新数组

##### object.difineProperty

第一个参数对象 ：目标（给哪个对象添加属性）

第二个参数字符串 ：名字（属性名叫什么）

第三个参数类型是对象：配置项 {enumerable:true；控制属性是否可枚举，默认值 false。writable:true;控制属性是否可修改，默认 false。configurable:true;控制属性可删除，默认 false。}

数据代理案例

```js
let a = {
  b:"1"
}
let c = "2"
Object.defineProprrty(a,"c",{
  get(){
    return c
  }
  set(value){
  return c = value
}
})
```

##### ?.和??运算符

??和||类似但是不同的是逻辑或会在左侧为假值时返回右侧的操作符而？？只会在左侧为 null 和 undefined 时返回右侧

`?.`主要用于在多层的 object/array 进行取值和函数调用，一般如果左边的值为 undefined 和 null 时会报错，但是加上?.就不会报错了

```js
let b;
b?.map((i) => {
  console.log(i);
});
```

##### for in 和 for of

for in

1.for in 更适合遍历对象，因为 for in 利用索引遍历，不能进行几何运算

2.会遍历数组的所有可枚举值，包括原型

for of

1.for of 遍历的是数组元素值，而且只是数组内的元素，不包括原型

2.for of 不能遍历对象，只能遍历有迭代器对象（iterator）的集合，如数组，字符串,map,set 等

```js
// for in
var obj = { a: 1, b: 2, c: 3 };

for (let key in obj) {
  console.log(key);
}
// a b c

//for of
const array1 = ["a", "b", "c"];

for (const val of array1) {
  console.log(val);
}
// a b c
```

hasOwnProperty()

可以判断某属性是不是该对象的实例属性

```js
var arr = [1, 2, 3];
Array.prototype.a = 123;
for (let index in arr) {
  if (arr.hasOwnProperty(index)) {
    let res = arr[index];
    console.log(res);
  }
}
// 1 2 3
```

##### Symbol

这个函数会动态的生成一个匿名、全局唯一的值。

symbol 的基本用处

1.避免对象的键被覆盖

2.避免魔术字符串

##### 模糊搜索

```
//vue计算属性写法
computed:{
	return this.list.filter(i=>{
		return i.name.indexof(this.inputValue) !== -1
	})
}
//计算属性初始化就会执行一次，indexof（）这时候还没输入，所以indexof（''）返回全部
```

##### indexof()

```
//indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。
stringObject.indexOf(searchvalue,fromindex)
fromindex 非必填 规定在字符串中开始检索的位置
```

##### filter()

```
Array.filter() 三个参数 item index arr
1.创建一个新的数组
2.必须有返回值 return
3.和map不同的是符合条件返回true 否则false

方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
```

##### sort()

Array.sort() 两个参数 a b

1.改变原数据

2.a-b 升序 b-a 降序

3.写回调函数时要有返回值

##### Push()

往数组末尾加一个或多个元素 返回改变后数组的长度

##### unshift()

方法可向数组的开头添加一个或更多元素，并返回新的长度 返回改变后数组的长度

##### splice()

方法向/从数组添加/删除项目，并返回删除的项目。

三个参数

第一个从什么地方添加或删除写 index，如果写负数就是从后面开始

第二个 删除多少个 写 0 就是不删 或者不写也是不删

第三个 需要添加到数组的新项目

##### Shift()

删除数组中的第一个元素 改变原数组

##### reverse()

方法用于颠倒数组中元素的顺序。
