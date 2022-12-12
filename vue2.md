# Vue2

## 数据绑定
v-bind:单向绑定，数据只能从data流向页面。

v-model：双向绑定，数据不仅能从data流向页面，还能从页面流向data。

备注：

v-model只能应用在表单类元素（输入类元素），如input、select

v-model:value 可以简写为v-model，应为v-model默认收集的就是value值。


## el与data的两种方法

el的第一种写法：

```
 el: '#root'

```

el的第二种写法：

```
        const v = new Vue({
            // el: '#root', //第一种写法
            data:{ // 用于存储数据，供el提供的容器使用。
                name: 'ironartisan',
                school: {
                    name: 'halo',
                    url: 'https://ironartisan.top'
                }
                
            }
        })

        console.log(v)
        v.$mount('#root') //第二种写法
```

data的第一种写法：对象式

```
data:{
 name:'ironartisan'
}
```

data的第二种写法：函数式

```
data:function() {
 return {
    name: 'ironartisan'
  }
}
```

由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。


## MVVM模型

- M:模型（Model）,data中的数据
- V:视图（View），模板代码
- VM：视图模型（ViewModel）,Vue实例

观察发现：

- data中所有的属性，最后都出现在vm身上
- vm身上所有的属性及Vue原型上所有属性，再Vue模板中都可以直接使用。
  
## 事件

### 基本使用

- 使用`v-on:xxx`或`@xxx`绑定事件，其中`xxx`是事件名；
- 事件的回调需要配置在methods对象中，最终会在`vm`上；
- methods中配置的函数，不要用箭头函数，否则`this`就不是`vm`了；
- methods中配置的函数，都是被Vue管理的函数，`this`的指向是vm或组件实例对象；
- `@click="demo"`和 `@click="demo($event)"`效果一致，但后者可以传参。

### 事件修饰符

- `prevent`:阻止默认事件（常用）；
- `stop`:阻止事件冒泡（常用）；
- `once`：事件只触发一次（常用）；
- `capture`：使用事件的捕获模块；
- `self`：只有event，target是当前操作元素才触发事件；
- `passive`：事件的默认行为立即执行，无需等待事件回调执行完毕；
  
## 计算属性

- 定义：要用的属性不存在，要通过已有属性计算得来
- 原理：底层借助了`Object.defineproperty`方法提供的`getter`和`setter`
- get函数什么时候执行？
  - 初次读取时会执行一次
  - 当依赖的数据发生改变时会被再次调用
- 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
- 备注：
  - 如果属性最终会出现在vm上，直接读取使用即可。
  - 如果计算属性要被修改，那必须写set函数去相应修改，且set中要引起计算时依赖的数据发生改变

## 监视属性

- 当监视属性变化时，回调函数自动调用，进行相关操作
- 监视的属性必须存在，才能进行监视
- 监视的两种写法：
  - `.new Vue`时传入watch配置
  - 通过`vm.$watch`监视

## 深度监视

- vue中的watch默认不监测对象内部值的改变（一层）
- 配置`deep:true`可以监测对象内部值的改变（多层）

备注：
- vue自身可以监测对象内部值的改变，但vue提供的watch默认不可以
- 使用watch时根据数据的具体结构，决定是否采用深度监视
  

## computed与watch之问的区别:
- computed能完成的功能，watch都可以完成。
- watch能完成的功能，computed不一定完成，例如：watch可以进行异步操作。
- 两个重要的小原则: 
  - 所被vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象。
  - 所有不被vue管理的函数（定时器的回调函数、ajax的回调函数、promise的回调函数），最好写成箭头函数，这样this的指向才是vm或组件实例对象。

## 绑定样式

绑定class样式：`:class`

- 字符串写法。适用于样式的类名不确定，需要动态指定。
- 数组写法。适用于要绑定的样式个数不确定、名字也不确定
- 对象写法。适用于要绑定的样式个数确定、名字也确定，但要动态决定用不用。

绑定style样式：`:style`

- 对象写法。`:style={fontSize:xxx}`,其中xxx是动态值。
- 数组写法。`:style=[a,b]`，其中a、b是样式对象。

## 监视数据原理

- Vue会监视data中所有层次的数据
- 如何检测对象中的数据
  - 通过setter实现监视，且要在new Vue时就传入要检测的数据
    - 对象中后追加的属性，Vue中默认不做响应式处理
    - 如需给后添加的属性做响应式，请使用如下API
      - `Vue.set(target, propertyName/index, value)`
      - 或`Vm.$set(target, propertyName/index, value)`
- 如何监测数组中的数据
  - 通过包裹数组更新元素的方法实现，本质就是做了两件事：
    - 调用原生对应的方法对数组进行更新
    - 重新解析模板，进而更新页面
- 在Vue修改数组中的某个元素一定要用如下方法：
  - 使用这些API：`push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse`
  - Vue.set()或vm.$set()

特别注意：`Vue.set()`和`vm.$set()`不能给vm或vm的根对象添加属性。


## 收集表单数据

- 若`<input type="text">`，则用`v-model`收集的是value值，用户输入的就是value值
- 若`<input type="radio">`，则用`v-model`收集的是value值，且要给标签配置value值
- 若`<input type="checkbox">`
  - 没有配置input的value属性，那么收集的是checked（布尔值）
  - 配置input的value属性
    - `v-model`的初始值是非数组，收集的就是checked（布尔值）
    -` v-model`的初始值是数组，收集的就是value组成的数组

备注：`v-model`的三个修饰符：
- `lazy`：失去焦点再收集数据
- `number`：输入字符串转为有效的数字
- `trim`：输入首位空格过滤
  

## 过滤器

- 定义：对要显示的数据进行特定格式化后再显示
- 语法：
  - 注册过滤器：`Vue.filter(name, callback)`或`new Vue(filter:{})`
  - 使用过滤器：`{{xxx |过滤器名}}`或 `v-bind:属性="xxx" | 过滤器名`

备注：
- 过滤器可以接收额外参数、多个过滤器也可以串联
- 并没有改变原本的数据，是产生新的对应的数据


## 生命周期

![生命周期](https://cdn.jsdelivr.net/gh/ironartisan/picRepo/生命周期.png)

常用的生命周期钩子:

- mounted: 发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
- beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。

关于销毁Vue实例
- 销毁后借助Vue开发者工具看不到任何信息。
- 销毁后自定义事件会失效，但原生DOM事件依然有效。
-  一般不会再beforeDestroy操作数据, 因为即便操作数据, 也不会再触发更新流程了。

## 组件

Vue中使用组件的三大步骤:

- 定义组件(创建组件)
- 注册组件
- 使用组件(写组件标签)

如何定义一个组件?

- 使用`Vue.extend(options)`创建, 其中options 䄨new Vue(options)时传入的那个options几乎一样, 但也有点区别; 区别如下:
  - el不要写, 为什么? 最终所有的组件都要经过一个 vm 的管理, 由 vm 中的 el 决定服务哪个容器。
  -  data必须写成函数, 为什么? 避色组件被复用时, 数据存在引用关系。

备注: 使用template可以配置组件结构。

如何注册组件?

- 局部注册：靠`new Vue`的时侯传入`components`选项
- 全局注册：靠`Vue. component` ('组件名', 组件)
  
编写组件标签：
- `<school></school>`

几个注意点：

- 关于组件名:
  - 一个单词组成：
    - 第一种写法（首字母小写）：`school`
    - 第二种写法（首字母大写）：`School`
  - 多个单词组成：
    - 第一种写法（kebab-case命令）：`my-school`
    - 第二种写法(CamelCase命令）：`MySchool`(需要Vue脚手架支持)
  - 备注：
    - 组件名尽可能回避`HMTL`中已有的元素名称，例如:`h2` `H2`都不行
    - 可以使用name配置项指定组件再开发者工具中呈现的名字
- 关于组件标签
  - 第一种写法:`<school></school>`
  - 第二种写法：`<school/>`
  - 备注：不适用脚手架时，`<school/>`会导致后续组件不能渲染
- 一个简写方式
  `const school = Vue.extend(options)`可简写为：`const school = options`

关于 VueComponent：
- school组件本质上是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的
- 我们只需要写`<school></school>`或`<school/>`，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`
- 特别注意：每次调用Vue.extend，返回的是一个全新的VueComponent
- 关于this指向：
  - 组件配置中：
    - data函数、methods中的函数、watch中的函数、computed中的函数，他们的this均是【VueComponent实例对象】
    - .new Vue()配置中：
      - data函数、methods中的函数、watch中的函数、computed中的函数，他们的this均是【Vue实例对象】
- VueComponent的实例对象，以后简称vc（也称为组件实例对象）
- Vue的实例对象，以后简称vm

## ref属性

- 被用来给元素或子组件注册引用信息（id的替代者）
- 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
- 使用方式：
  - 打标识：`<h1 ref="xxx"> <h1>`或`<School ref="xxx"></School>`
  - 获取：this.$refs.xxx

## 配置项props

功能：让组件接收外部传过来的数据

- 传递数据：`<Demo name="xxx"/>`
- 接收数据：
  - 第一种方式（只接收）：`props:['name']`
  - 第二种方式（限制类型）：`props:{name:Number}`
  - 第三种方式（限制类型、限制必要性、指定默认值）
  ```html
      props:{
        schoolName:{
            type:String,// 类型
            required:true //必要的
            default：'北邮'
        },
      }
  ```

备注：props是只读的，Vue底层会检测你对props的修改，如果进行了修改，就会发出警告。若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。