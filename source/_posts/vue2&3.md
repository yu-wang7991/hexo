title: VUE 2 & 3
date: 2021-4-20 14.37  
modified: 2021-4-20 14.37
tag:

- javascript

photos:

- https://images.unsplash.com/photo-1501555088652-021faa106b9b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8OXx8b3V0ZG9vcnxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=800&q=60

---

vue 复习

<!--more-->

##### MVVM

VUE 参考了 mvvm 架构模型，m（模型 model）data 中的数据，v（视图 view）模版，vm（视图模型 viewmodel）vue 实例对象

但是 vue 中添加了一个属性。ref
通过 ref 可以拿到 dom 对象，通过 ref 直接去操作视图。这一点上，违背了 mvvm

##### 数据代理

![](../img/Snipaste_2021-11-16_23-33-59.png)

这部分只做了数据代理，没有做数据劫持，也就是数据改变视图改变（响应式），其实\_data 里面的属性 vue 有做数据劫持。

Vue2.x 是使用 Object.defindProperty()，来进行对对象的监听的，但是改变数组里的某个值不会触发 set，(如果要监听的到话，需要重新编写数组的方法)， 必须遍历每个对象的每个属性，如果对象嵌套很深的话，需要使用递归调用。

##### 事件修饰符

1.prevent:阻止默认事件（常用）

2.stop：阻止时间冒泡（常用）

3.once：事件只触发一次（常用）

4.capture:使用事件的捕获模式

5.self：只有 event.target 是当前操作元素才会触发事件

6.passive：事件的默认行为立即执行，无需等待事件回调执行完毕。（@scroll 滚动条事件用不到，@wheel 滚轮事件用到一般 c 端会用到）

Tips：可以使用链式写法@click.prevent.stop

##### 键盘事件

@keyup.enter=""

1.vue 常用的按键别名

回车 enter

删除 delete （退格和删除都能触发）

退出 esc

空格 space

2.vue 为提供的按键，可以使用案件原始的 key 值去绑定，但注意转为短横线命名。

3.系统修饰键用法比较特殊 最好配合 keydown 使用

4.vue.config.keyCode.自定义键名 = 键码

Tips：可以使用链式写法@keyup.enter.y=""意思是 enter 和 y 都要按才能触发

##### 计算属性

```js
完整写法
computed:{
	fullName:{
		get(){
			return this.nameA+'-'this.nameB
		}
		set(value){
		const arr = value.split('-')
		this.nameA = arr[0]
		this.nameB = arr[1]
		}
	}
}
简写（只用到get，用不到set的时候使用）
computed:{
	fullName(){
		return this.nameA+'-'this.nameB
	}
}
```

##### 监视属性

```js
data:{
	id:2,
	numbers:{
		a:1,
		b:1
	}
}
watch:{
	id:{
		handler(){
			console.log('a改变了')
		}
	}，
	//如果要监视多级结构中某个属性的变化可以这样写
	'numbers.a':{
  	handler(new,old){
  			console.log('a改变了')
  	}
	}，
	//监视多级结构所有属性的变化
	numbers:{
		immediate:true,//初始化时让handler执行一次
		deep:true,//深度监视
		handler(new,old){
		console.log('numbers改变了')
		}
	}
}
//简写（用不到deep，和immediate时使用）
watch:{
	id(new,old){
		console.log('id改变了')
	}
}

Tips：
vm.$watch('id',{
		immediate:true,//初始化时让handler执行一次
		deep:true,//深度监视
		handler(n,o){
		}
})
vm.$watch('id',function(n,o){
console.log('id改变了'，n,o)
})
```

##### 计算属性和监听属性的区别

computed 能完成的，watch 都能完成，watch 可以执行异步任务，computed 不行，computed 只有原始值发生变化，才会触发，具有缓存，watch 是数据改变就会触发。

Tips：所有被 vue 管理的函数嘴还写成普通函数，所有不被 vue 管理的函数最好写成箭头函数，这样 this 才是指向 vm 或组件实例对象。

##### vue.set()

##### 数据劫持

```js
//正常初始化data后，模版解析渲染页面，这个时候是响应式的，但是后面又想给data对象中追加数据，这个时候vue不做响应式，就像vue的bug。
如果要给对象加属性，而且要响应式，要用
1.vue.set(target,propertyName/index,value)或vm.$set(target,propertyName/index,value)
2.但是vue.set和vm.$set不能给vm或vm根数据对象添加对象（Tips:不能给data直接添加属性 要再加一层结构）
如果要修改数组中的某个元素一定要用如下方法不然不能响应式，可以使用这些vue包裹过的API push() shift() unshift() splice() sort() reverse()或者vm.$set() vue.set()
```

##### Es6

``` js
//形参默认值
test(a="1"){
	console.log(a)
}
test（）//1
test（2）//2
```

##### 管道符

```js
//和methods 同一层级的配置项
常用在插值表达式中{{a | c}}
methods:{

},
filter:{
	c(value){
	//value 会收到a
	return a + 1
	}
}
```

##### V-html

向指定节点渲染包换 html 结构的内容

v-html 可以识别 html 结构

v-html 有安全性问题 不要在用户提交的内容上使用 容易导致 xxs 攻击

##### v-clock

直接写在标签里 <div v-clock/>

使用 css 属性属性选择器[v-clock]{}可以解决因为网速满页面出现{{name}}情况

当脚本跑起来时 vue 会删掉 v-clock 属性

##### V-once

只在初始化时解析一次 后面就视为静态内容 <div v-once/>

##### 自定义指令

```js
1.v-自定义指令名

2.写在配置项directives：{

自定义指令名（element，binding）{

		element.innerText = binding.value * 10

 	}

}
//如果需要处理一些逻辑时自定义指令写成对象形式
fbind:{
  //一上来调用，指令绑定时
  bind(e,b){

  },
  //指令所在的元素被插入页面时
  inserted(e,d){
    //比如input获取焦点需要写在这里
  },
  //指令所在的模版被重新解析时
   update(e,b){

   }
}

3.初始化时调用一次，指令所在的模版被重新解析式调用。
```
