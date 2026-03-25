##### v-if和v-show的比较


<table>
	<th colspan="2">
		<th align=center>v-if</th>
		<th align=center>v-show</th>
	</th>
	<tr>
        <th colspan="2" align=center>共同点</th>
		<td colspan="2">都能实现元素的显示隐藏</td>
	</tr>
	<tr>
        <th rowspan="5" align=center>不同点</th>
        <td align=center>方式</td>
        <td>通过控制dom节点的存在与否来控制元素的显隐</td>
        <td>通过设置DOM元素的display样式，block为显示，none为隐藏</td>
	</tr>
	<tr>
		<td align=center>编译过程</td>
        <td>切换有一个局部编译/卸载的过程，切换过程中<br>
            合适地销毁和重建内部的事件监听和子组件</td>
        <td>只是简单地切换元素的 CSS property display</td>
	</tr>
	<tr>
		<td align=center>编译条件</td>
        <td>
            v-if是惰性的，如果初始条件为假，则什么也不做；<br>
            只有在条件第一次变为真时才开始局部编译
        </td>
        <td>
            v-show是在任何条件下（首次条件是否为真）都被编译，<br>
            然后被缓存，而且DOM元素保留；
        </td>
	</tr>
	<tr>
		<td align=center>性能消耗</td>
        <td>有更高的切换消耗</td>
        <td>有更高的初始渲染消耗</td>
	</tr>
	<tr>
		<td align=center>其他</td>
        <td>
            有配套的 v-else-if 和 v-else<br>
        	可以搭配 template 使用
        </td>
        <td align=center>
            ------
        </td>
	</tr>
	<tr>
        <th colspan="2" align=center>应用场景</th>
        <td>
        	运行时条件很少改变,如：<br>
            登陆、注册等Dialog
        </td>
        <td>
            需要非常频繁地切换,如：<br>
            页面上需要频繁显示与关闭的控件
        </td>
	</tr>
	<tr>
        <th colspan="2" align=center>总结</th>
        <td colspan="2">
        	v-if判断是否加载，可以减轻服务器的压力，在需要时加载,<br>
            但有更高的切换开销，比如单页面应用;<br><br>
            v-show调整DOM元素的CSS的dispaly属性，<br>
            可以使客户端操作更加流畅，但有更高的初始渲染开销。<br>
            如果需要非常频繁地切换，则使用 v-show 较好，tab中使用较好；
        </td>
	</tr>
</table>
##### Vue基本模板语法

```html
<!DOCTYPE html>
<html lang="zh">
  <head>
    <meta charset="UTF-8">
    <title>Vue小案例</title>
    <style type="text/css">
      #app {
        margin-bottom: 50px;
      }
      .info{
        width: 200px;
        background-color: #efefef;
      }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
  <body>
    <h2>Vue基本模板语法</h2>
    <div id="app">
      <h3>1、文本引用</h3>
      <div>{{ title }}</div>
      <input type="text" v-model="title">


      <h3>2、HTML</h3>
      原始数据:<p>{{ rawHtml }}</p>
      HTML片段:<p v-html="rawHtml">标签中的内容会被替换掉</p>


      <h3>3、Attribute</h3>
      <button v-bind:disabled="false">使用v-bind:可以操作属性</button>
      <button :disabled="true">使用v-bind:可以操作属性</button>


      <h3>4、指令</h3>
      <a v-bind:href="url">v-bind就是一个指令</a>
      <br><br>
      <button v-on:click="alertDemo('v-on:')">v-on指令</button>
      <button @click="alertDemo('v-on:的缩写@')">v-on指令</button>


      <h3>5、条件渲染</h3>
      <div v-if="isShow1">v-if的内容被显示啦！</div>
      <div v-else>这里与刚才的内容不可能同时出现！</div><br>

      <button @click="isShow1=!isShow1">切换v-if的显示内容</button><br><br>
      <div v-show="isShow2">v-show的内容被显示啦！</div>
      <button @click="isShow2=!isShow2">切换v-show的显示内容</button>


      <h3>6、列表渲染、事件处理</h3>
      <div>
        <h5>数组</h5>
        <p v-for="(item,index) in arrayList">{{item}}----索引:{{index}}</p>
      </div>

      <div>
        <h5>对象数组</h5>
        <ul v-for="(item, index) in dataItems">
          <li class="info" @click="alertInfo(item)">{{item.id + '------' + item.name}}</li>
        </ul>
      </div>

      <div>
        <h5>对象</h5>
        <p v-for="(val,key,index) in user">值：{{val}}---键：{{key}}-----索引:{{index}}</p>
      </div>



      <h3>7、数据绑定之选择框</h3>
      <select v-model="selected">
        <option disabled value="">请选择</option>
        <option value="A">A</option>
        <option value="B">B</option>
        <option value="C">C</option>
      </select>
      <span>Selected: {{selected}}</span>



      <h3>8、组件</h3>
      <!--需要注意的是，组件的最外层需要由vue实例包裹，否则不会生效-->
      <test-com></test-com>
      <component-a></component-a>


      <h3>9、props传递信息</h3>
      <div>
        <props-com v-for="(item, index) in propsData" :value="item"></props-com>
      </div>

      <h3>10、计算属性</h3>
      <!--
        计算属性与方法的区别：
            1、两者的执行结果是完全相同的
            2、计算属性基于他们的依赖进行缓存的，只有在相关依赖发生改变时，
            他们才会重新求值，也就是说，只要他的依赖没有发生变化，
            那么每次访问的时候计算属性都会立即返回之前的计算结果，不再执行函数
            3、每次触发重新渲染时，调用方法将总会再次执行函数
      -->
      <input type="text" v-model="num1">
      <input type="text" v-model="num2">
      <p>求和结果：{{sum}}</p>
    </div>
  <!--这里没有vue实例包裹-->
  <test-com></test-com>
  </body>
  <script type="text/javascript">
    //这里定义了一个组件（需要在实例创建前定义）
    Vue.component("test-com", {
      template: '<li>这里是全局组件内容(全局组件)</li>'
    })
    let ComponentA = {
      template: '<li>这里是局部组件内容(局部组件)</li>'
    }
    Vue.component("props-com", {
      props: ["value"],
      template: '<li>{{value}}</li>'
    })
    //创建一个Vue实例
    let app = new Vue({
      //挂载页面元素
      el: "#app",
      data: {
        title: 'Hello world!',
        rawHtml: '<span style="color:#ff0000">This should br red.</span></p>',
        url: "https://www.baidu.com/",
        isShow1: true,
        isShow2: false,
        arrayList:[1,2,3,4,5],
        dataItems: [{
          id: "1",
          name: "jack"
        },{
          id: "2",
          name: "tom"
        }],user:{
          name:"tom",
          age:"18",
          sex:"男"
        },
        selected: '',
        propsData: ['Java', 'Python'],
        num1: 1,
        num2: 2,
      },
      methods:{
        alertDemo(str){
          alert("点击事件：" + str)
        },
        alertInfo(item){
          alert(item.id + "." + item.name + "被点击了")
        }
      },
      components: {
        ComponentA
      },
      computed: {
        sum: function(){
          return parseInt(this.num1) + parseInt(this.num2)
        }
      }
    });

  </script>
</html>
```

####    Vue生命周期

##### 1、Vue生命周期简介

Vue实例从创建到销毁的过程，就是生命周期。详细来说也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、卸载等一系列过程。

下面是其生命周期图：

![20171026193015421](.\pic\2020032216442443.png)

Vue提供的钩子是图中的红色部分。

##### 2、钩子介绍

###### 1、beforeCreate

在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。

在这个阶段Vue中的data和methods都是获取不到的。

###### 2、created

实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
**主要应用：**调用数据，调用方法，调用异步函数

created钩子可以获取Vue的data，调用Vue方法，获取原本HTML上的直接加载出来的DOM，但是无法获取到通过挂载模板生成的DOM（例如：v-for循环遍历Vue.list生成li）。

###### 3、beforMount

在挂载开始之前被调用：相关的 render 函数（模板）首次被调用。
例如通过v-for生成的html还没有被挂载到页面上

###### 4、mounted

el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。
**有初始值的DOM渲染，例如我们的初始数据list,渲染出来的li，只有这里才能获取**

###### 5、beforeUpdate

数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
当我们更改Vue的任何数据，都会触发该函数

###### 6、updated

由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。
该钩子在服务器端渲染期间不被调用。

数据更新就会触发（vue所有的数据只有有更新就会触发）,如果想数据一遍就做统一的处理，可以用这个，如果想对不同数据的更新做不同的处理可以用nextTick，或者是watch进行监听。

###### 7、beforeDestroy

实例销毁之前调用。在这一步，实例仍然完全可用。

###### 8、destroyed

Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。只有在销毁vue实例时才会调用这两个钩子函数

##### Vue生命周期

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<div id="app">
    <aaa></aaa>
</div>

<template id="aaa">
    <div>
        <p class="myp">A组件</p>
        <button @click="destroy">destroy</button>
        <input type="text" v-model="msg">
        <p>msg:{{msg}}</p>
    </div>
</template>

</body>
<script src="./vue.js"></script>

<script>
    //生命周期：初始化阶段 运行中阶段 销毁阶段
    Vue.component("aaa",{
        template:"#aaa",
        data:function(){
            return {msg:'hello'}
        },
        timer:null,
        methods:{
            destroy:function(){
                this.$destroy()
            }
        },
        // beforeCreate:function(){
        //     console.log('beforeCreate:刚刚new Vue()之后，这个时候，数据还没有挂载呢，只是一个空壳')
        //     console.log(this.msg)//undefined
        //     console.log(document.getElementsByClassName("myp")[0])//undefined
        // },
        // created:function(){
        //     console.log('created:这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数')
        //     this.msg+='!!!'
        //     console.log('在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取')
        //     console.log('接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染')
        // },
        // beforeMount:function(){
        //     console.log('beforeMount：虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated')
        //     this.msg+='@@@@'
        //     console.log('在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取')
        //     console.log(document.getElementsByClassName("myp")[0])//undefined
        //     console.log('接下来开始render，渲染出真实dom')
        // },
        // // render:function(createElement){
        // //     console.log('render')
        // //     return createElement('div','hahaha')
        // // },
        // mounted:function(){
        //     console.log('mounted：此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了')
        //     console.log(document.getElementsByClassName("myp")[0])
        //     console.log('可以在这里操作真实dom等事情...')
        //
        //        // this.$options.timer = setInterval(function () {
        //        //     console.log('setInterval')
        //        //      this.msg+='!'
        //        // }.bind(this),500)
        // },
        // beforeUpdate:function(){
        //     //这里不能更改数据，否则会陷入死循环
        //     console.log('beforeUpdate:重新渲染之前触发')
        // },
        // updated:function(){
        //     //这里不能更改数据，否则会陷入死循环
        //     console.log('updated:数据已经更改完成，dom也重新render完成')
        // },
        // beforeDestroy:function(){
        //     console.log('beforeDestory:销毁前执行（$destroy方法被调用的时候就会执行）,一般在这里善后:清除计时器、清除非指令绑定的事件等等...')
        //     clearInterval(this.$options.timer)
        // },
        // destroyed:function(){
        //     console.log('destroyed:组件的数据绑定、监听...都去掉了,只剩下dom空壳，这里也可以善后')
        // }
    })



    new Vue({
    }).$mount('#app')


</script>
</html>
```

参考地址：

https://blog.csdn.net/DQ1005/article/details/89345248