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

- 使用v-on:xxx或@xxx绑定事件，其中xxx是事件名；
- 事件的回调需要配置在methods对象中，最终会在vm上；
- methods中配置的函数，不要用箭头函数，否则this就不是vm了；
- methods中配置的函数，都是被vue管理的函数，this的指向是vm或组件实例对象；
- @click="demo"和 @click="demo($event)"效果一致，但后者可以传参。

### 事件修饰符

- prevent:阻止默认事件（常用）；
- stop:阻止事件冒泡（常用）；
- once：事件只触发一次（常用）；
- capture：使用事件的捕获模块；
- self：只有event，target是当前操作元素才触发事件；
- passive：事件的默认行为立即执行，无需等待事件回调执行完毕；
  
## 计算属性

- 定义：要用的属性不存在，要通过已有属性计算得来
- 原理：底层借助了Object.defineproperty方法提供的getter和setter
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
  - .new Vue时传入watch配置
  - 通过vm.$watch监视

## 深度监视

- vue中的watch默认不监测对象内部值的改变（一层）
- 配置deep:true可以监测对象内部值的改变（多层）

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


