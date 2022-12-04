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

