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

![](imgs/Snipaste_2021-11-16_23-33-59.png)

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
4.指令函数里的this指向window，而不是由vue管理的函数都指向vm，因为指令函数把元素传给你了，方便你操作dom
```

##### 生命周期

```js
beforCreated 访问不到数据和方法，因为还没数据监听和数据代理
created 初始化完成，数据和方法都可以访问 但是真实dom还没有挂载，一般写初始化数据，比如通过求返回的数据渲染页面，但是访问不到htmlelement
beforMount 挂载前，页面已经出来但是还未经vue编译，此时对dom操作都没有效果，因为下一步会将之前准备好的虚拟dom转换成真实dom，而不是你改完的dom
mounted 数据，方法和dom都已经准好，一般在这里写需要等初始化页面完成后，再对html的dom节点操作的需求。如果在这里面写会更新dom需要用到的data数据时，比如根据借口返回值渲染页面，可能发生闪屏。
beforUpdate 数据已经更新，但是页面还没渲染
updated 页面渲染完成
beforDestory 实例销毁前，一般写收尾工作，比如消除定时器等。注意不要写操作数据，因为不会触发更新。
destory
```

##### vue 组件化

```js
有单文件组件和多文件组件,下面是单文件组件的配置写法，一般实际项目上不会用单文件组件，到vue开发工具里组件注册时的名字会变成首字母大写，如果不想用注册时的名字，可以加配置项name
<script>
      Vue.config.productionTip = false;
      const msg = Vue.extend({
        data() {
          return {
            msg: "不是",
          };
        },
        template: `
        <div>
          <p>{{msg}}</p>
          </div>
        `,
      });
      const error1 = {
        data() {
          return {
            error: "错误",
          };
        },
        template: `
        <p>{{error}}</p>
        `,
      };
      const error = {
        data() {
          return {
            error: "是",
          };
        },
        template: `
        <div>
          <p>{{error}}</p>
          <error1 />
          </div>
        `,
        //组件嵌套
        components: {
          error1,
        },
      };

      const vm = new Vue({
        el: "#app",
        components: {
          error,
          msg,
        },
        data() {
          return {};
        },
        methods: {},
        created() {},
        mounted() {},
      });
    </script>
```

VueComponent

```js
//每一个组件其实就是VueComponent的实例对象，VueComponen是一个构造函数，由vue.extend生成的，也就是new出来的，每个组件里的this指VueComponent，我们写的组件，比如组件名叫school，使用时<school >，vue在解析的时候会帮我们创建new Vuecomponent(options)；
```

##### 内置关系

```js
VueComponent.property._proto_ === vue.property
组件实例的原型对象默认zhii x指向vue原型对象，这样方便组件访问vue的属性和方法。组件实例的原型对象默认指向object原型对象，vue改变了它的指向。
//Tips：所有函数实例的_proto_都指向缔造它的原型原型对象，property是显式的原型属性，_proto_是隐式的原型属性
```

##### 为什么 main.js 里要写成 render 函数

```
import Vue from 'vue'实际上引入的是运行时vue，并不是完整版的vue，少了template解析器，因为webpack打包完后就不用template了,这样做节约了100多kb,最重要的打包后不需要用到template解析器代码。
```

##### props

```
功能：让组件接受外部传的数据
传递数据
<Demo name="xxx"/>
接受数据：
第一种方式（只接收）
props:['name']
第二种方式（限制类型）
props:{
name:String
}
第三种方式（限制类型，限制是否必传，默认值）
props:[
{name:String,required:true,default:'老王'}
]
Tips：props是只读的，在组件内直接改prop会页面包错，需要在data中复制一份，myName:this.name
```

##### Mixin 混入

```js
export defule {
	data(){

	},
	methods:{

	},
	//生命周期函数会都调用
	mounted(){

	}
}
import mixins from './xxx'
//局部混入
mixins:['xxx']
//全局混入
Vue.mixin(xxx)
文件里有的，混入文件里也有，优先用文件里有的。
```

##### ES6 Module 及 CommonJS 的配对使用

```js
export  function common(params) {
  console.log(params)
};

export let commcls=new class{
  constructor(name,sex,age=27){
    this.name=name;
    this.sex=sex;
    this.age=age;
  }
  GetName(){
    return "我的名字是"+ this.name;
  }
  get Money(){
    return "我是属性钱，大家都爱我"
  }

};

export  const pi=3.1415926;
//这种写法是分别导出
//按需导出的写法是去掉export，在最下面写上export {common,commcls,const}
/**最常用的全部导出写法 export default{common:function common(params) {
  console.log(params)
}}写成键值对**/
//以上有函数，js类，常量的导出

导入
import {common,commcls} from './xxx'
import * as api from './xxx'

导出
module.exports = {
    name: 'commonJS_exports.js',
    add: function(a, b){
        return a + b;
    }
}

导入
let comObj = require('../api/module/commonJS_exports');
let {name} = require('../api/module/commonJS_exports');
//在module对象中有一个属性loaded用于记录该模块是否被加载过，它的默认值为false，当模块第一次被加载和执行过后会设置为true，后面再次加载时检查到module.loaded为true, 则不会再次执行模块代码。require函数可以接收表达式，借助这个特性我们可以动态地指定模块加载路径
const moduleNames = ['foo.js', 'bar.js'];
moduleNames.forEach(name=>{
   require('./' + name);
})

CommonJS和ES6 Module的区别
//CommonJS模块依赖关系的建立发生在代码运行阶段,ES6 Module模块依赖关系的建立发生在代码编译阶段。前者可以动态指定require的模块路径，后者不行，但是可以冗余代码检测和排除，我们可以用静态分析工具分析工具检测出哪些模块没有被调用过，然后去掉这些模块，后者编译器程序效率更高。
```

###### 插件的使用

```js
创建一个plugins.js
export defulue{
	install(vue,x){
		//比如全局过滤器
		Vue.filter(...)
		//全局混入
		Vue.mixin(...)
	}
}
在main.js
import plugins from './plugins'
Vue.use(plugins,1)//可以传参

```

##### 组件传值

```js
1.父向子传值
//父组件
<Student :name="小明"/>
//自组件
props：[name]

2.子向父传值
(1)第一种
//父组件
	<Student @getName="demo"/>
  //getName是自定义事件名称
  methods:{
    getName(name){
      console.log(name)
      //小明
    }
  }
//子组件
<button @clicl="sendName"><button/>
  data(){
  return{
    name:"小明"
  }
}
	methods:{
    sendName(){
      //getName这里必须写的和在父组件里子组件标签上自定义事件名字一样
      this.$emit('getName',this.name)
      //解除绑定自定义事件
      //this.$off('getName')
    }
  }
(2)第二种
//父组件
<Student :getName="getName"/>
  //传一个函数给子组件
  methods:{
    getName(name){

    }
  }
//子组件
<button @clicl="sendName"><button/>
data(){
  return{
    name:"小明"
  }
}
props：[getName]，
methods:{
  sendName(){
    this.getName(this.name)
  }
}
(3)第三种
//父组件
<Student ref="Student"/>
  methods:{
    getName(name){

    }
  }
  mounted(){
 this.$refs.Student.$on('getName',this.getName)
    //这种写法可以延迟绑定自定义事件
}
//子组件
<button @clicl="sendName"><button/>
  data(){
  return{
    name:"小明"
  }
}
	methods:{
    sendName(){
      this.$emit('getName',this.name)
    }
  }

```

##### $bus

```js
全局事件总线
在main.js 里new Vue({
...
beforCreated(){
	Vue.prototype.$bus = this
}
})
A组件接收数据
methods:{
	demo(data){

	}
},
mounted(){
	this.$bus.$on('xxx',this.demo)
}
B组件提供数据
this.$bus.$emit('xxx',this.data)
```

##### $nextTick

- 作用：在下一次 dom 更新后调用
- 什么时候用：当数据改变，要基于更新后的新 dom 进行操作时，要在 nextTick 的回调函数里执行。

##### Vue 封装的过渡与动画

元素进入的样式

- v-enter 进入的起点
- v-enter-active 进入过程中
- v-enter-to 进入的终点

元素离开的样式

- v-leave 离开的起点
- v-leave-active 离开的过程中
- V-leave-to 离开终点

<transition name="xxx"></transition>

- xxx 对应 v-leave 的 v
- appear 一开始进入时就有动画

若有多个元素需要过渡，transition-group 每个元素加 key 值

##### proxy 代理

写在 vue.config.js

第一种

```
devServer:{
	proxy:'http://localhost:5000'//目标服务器
}
```

缺点：这样写请求全部走了代理，不能控制哪些请求走代理。

第二种

```
devServer:{
	proxy:{
		'/api':{
			target:http://localhost:5000',
			pathRewrite:{'^/api':''}//去掉url的api
			ws:true,//开启webSockst
			changeOrigin:true,//你多大鞋我就多大的脚
		}
	}'
}
```

##### es6...扩展操作符

数组合并

```js
//es5
let book1 = ["平凡的世界第一部", "平凡的世界第二部", "平凡的世界第三部"];
let book2 = ["人生"];
let book3 = book1.concat(book2);
//console.log(book3) // (4) ["平凡的世界第一部", "平凡的世界第二部", "平凡的世界第三部", "人生"]

//es6
let book4 = [...book1, ...book2];
book4[3] = "月夜静悄悄";
console.log(book4); //(4) ["平凡的世界第一部", "平凡的世界第二部", "平凡的世界第三部", "月夜静悄悄"]
```

对象合并

```js
let obj1 = {
  book1: "平凡的世界第一部",
  book2: "平凡的世界第二部",
  book3: "平凡的世界第三部",
};
let obj2 = {
  ...obj1,
  book4: "人生",
};
console.log(obj2); //{book1: "平凡的世界第一部", book2: "平凡的世界第二部", book3: "平凡的世界第三部", book4: "人生"}

//如后者与前者相同，后者覆盖前者
let obj1 = {
  book1: "平凡的世界第一部",
  book2: "平凡的世界第二部",
  book3: "平凡的世界第三部",
  book4: "月夜静悄悄",
};
let obj2 = {
  ...obj1,
  book4: "月夜静悄悄11111",
  book5: "人生",
};
console.log(obj2); //{book1: "平凡的世界第一部", book2: "平凡的世界第二部", book3: "平凡的世界第三部", book4: "月夜静悄悄11111", book5: "人生"}

let obj = { name: "daisy" };
let obj1 = { job: "web" };
let obj2 = { sex: 1 };
let obj5 = { ...obj, ...obj1, ...obj2 };
console.log(obj5); //{name: "daisy", job: "web", sex: 1}

//与Object.assign用法相同 Object.assign(target, ...sources)
let obj4 = Object.assign(obj, obj1, obj2);
console.log(obj4); //{name: "daisy", job: "web", sex: 1}
console.log(obj); //{name: "daisy", job: "web", sex: 1}  /**注意目标对象也会随之改变 */
```

##### 插槽

1. 作用：让父组件可以向子组件指定位置插入 html 结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

2. 分类：默认插槽、具名插槽、作用域插槽

3. 使用方式：

   默认插槽：

```vue
父组件中：
<Category>
           <div>html结构1</div>
        </Category>
子组件中：
<template>
  <div>
    <!-- 定义插槽 -->
    <slot>插槽默认内容...</slot>
  </div>
</template>
```

​ 具名插槽

```vue
父组件中：
<Category>
            <template slot="center">
              <div>html结构1</div>
            </template>

            <template v-slot:footer>
               <div>html结构2</div>
            </template>
        </Category>
子组件中：
<template>
  <div>
    <!-- 定义插槽 -->
    <slot name="center">插槽默认内容...</slot>
    <slot name="footer">插槽默认内容...</slot>
  </div>
</template>
```

​ 作用域插槽

```vue
理解：
<span
  style="color:red"
>数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。</span>
（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）
父组件中：
<Category>
			<template scope="scopeData">
				<!-- 生成的是ul列表 -->
				<ul>
					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
				</ul>
			</template>
		</Category>

<Category>
			<template slot-scope="scopeData">
				<!-- 生成的是h4标题 -->
				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
			</template>
		</Category>
子组件中：
<template>
  <div>
    <slot :games="games"></slot>
  </div>
</template>

<script>
export default {
  name: "Category",
  props: ["title"],
  //数据在子组件自身
  data() {
    return {
      games: ["红色警戒", "穿越火线", "劲舞团", "超级玛丽"],
    };
  },
};
</script>
```

## Vuex

### 1.概念

​ 在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

### 2.何时使用？

​ 多个组件需要共享数据时

### 3.搭建 vuex 环境

1. 创建文件：`src/store/index.js`

```js
//引入Vue核心库
import Vue from "vue";
//引入Vuex
import Vuex from "vuex";
//应用Vuex插件
Vue.use(Vuex);

//准备actions对象——响应组件中用户的动作
const actions = {};
//准备mutations对象——修改state中的数据
const mutations = {};
//准备state对象——保存具体的数据
const state = {};

//创建并暴露store
export default new Vuex.Store({
  actions,
  mutations,
  state,
});
```

在`main.js`中创建 vm 时传入`store`配置项

```js
......
//引入store
import store from './store'
......

//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store
})
```

### 4.基本使用

1. 初始化数据、配置`actions`、配置`mutations`，操作文件`store.js`

```js
//引入Vue核心库
import Vue from "vue";
//引入Vuex
import Vuex from "vuex";
//引用Vuex
Vue.use(Vuex);

const actions = {
  //响应组件中加的动作
  jia(context, value) {
    // console.log('actions中的jia被调用了',miniStore,value)
    context.commit("JIA", value);
  },
};

const mutations = {
  //执行加
  JIA(state, value) {
    // console.log('mutations中的JIA被调用了',state,value)
    state.sum += value;
  },
};

//初始化数据
const state = {
  sum: 0,
};

//创建并暴露store
export default new Vuex.Store({
  actions,
  mutations,
  state,
});
```

1. 组件中读取 vuex 中的数据：`$store.state.sum`

2. 组件中修改 vuex 中的数据：`$store.dispatch('action中的方法名',数据)` 或 `$store.commit('mutations中的方法名',数据)`

   > 备注：若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写`dispatch`，直接编写`commit`

### 5.getters 的使用

1. 概念：当 state 中的数据需要经过加工后再使用时，可以使用 getters 加工。

2. 在`store.js`中追加`getters`配置

```js
......

const getters = {
	bigSum(state){
		return state.sum * 10
	}
}

//创建并暴露store
export default new Vuex.Store({
	......
	getters
})
```

1. 组件中读取数据：`$store.getters.bigSum`

### 6.四个 map 方法的使用

1. <strong>mapState 方法：</strong>用于帮助我们映射`state`中的数据为计算属性

```js
computed: {
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),

    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
```
