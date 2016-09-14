# Vue入门
vue.js是构建数据驱动的web界面框架，并不是一个全能的框架，聚焦于视图层，主要特点：
1. 响应数据绑定，修改模型数据，dom相应的也会更新。
2. 组件系统，独立可复用的小组件可以构建复杂的大型应用；
3. 简洁，数据驱动，组件化，轻量快速，模块友好。

## 1. 安装
    npm install vue -g
    bower install vue -g
    vue.js不支持ie8及其以下版本

## 2. 数据绑定

### 2.1 单向数据绑定，绑定表达式
    格式： {{对象属性名称}}

### 2.2 双向数据绑定
    格式： v-model="对象属性名称"

### 2.3 {{*}}首次绑定数据后，不随数据变化而改变（仅绑定一次）
    {{*message}}

### 2.4 {{{}}} 将html类型数据正常绑定到页面。
    按照正常表达式绑定数据时，如果表达式含有html标签，界面将会显示出标签。
    但是，往往这不是我们所需要的，需要将html标签解析成dom中的元素，此时我们采用{{{}}}
    可以转义解析成我们想要的。

## 3 vue实例

### 3.1 vue实例中data属性绑定的数据和原数据类型指向的是同一内存空间。
    var vm=new Vue({
    el:'#app',
    data:{
    message:msg}
    });
    vm.message==msg //true

### 3.2 vue实例创建绑定数据对象后，将数据对象重新绑定成其他的对象，不能刷新视图。
但是可以改变对象的属性值。因为数据绑定后，指向的一个内存空间，改变绑定对象，内存空间发生了变化。

### 3.3  实例创建后，不能设置以前对象没有的属性，无法映射到视图。通过Vue.set 和vm.$set可以添加对象属性。
      var vm = new Vue({
            el: "#app",
            data: {
                message: "<div>hello world<div>"
            }
        });

        vm.$set("age",100);

        //msg 对象变量
         Vue.set(msg,"name","li xiansheng");

## 4. vue实例属性和方法
### vue通过$暴露实例上的属性和方法
### 4.1 $el vue实例绑定的当前元素。

    不能更改绑定的对象。更改绑定对象元素并不能绑定新的对象和刷新视图。
    $el 实际上采用javascript原生的ducument.querySelector("#app")，来获取绑定对象元素。

### 4.2 $data 获取当前绑定的数据

    更改data对象属性，属性视图，尽量不要更改。

### 4.3 $watch 监控模型变化

    vm.$watch("message",function(newVal,oldVal){
    console.log(newVal,oldVal);
    });

## 5. 生命周期

| 方法名   |	     用法  |
| --------- |------------|
| created|先实例化,在实例化后(检测el)|
| vm.$mount('#app')|	手动挂载实例|
|beforeCompile|	开始编译之前|
|compiled|	编译完成后|
|ready|	插入文档后|
|vm.$destroy()	|手动销毁实例|
|beforeDestroy	|将要销毁|
|destroyed	|销毁实例|

### 5.1 生命周期的使用
``````
var vm = new Vue({
    data:{
        hello:'zfpx'
    },
    created: function () {alert('实例创建完成');},
    beforeCompile: function () {alert('开始编译前')},
    compiled: function () {alert('编译完成')},
    ready: function () {alert('准备好了')},
    beforeDestroy: function () {alert('准备销毁')},
    destroyed: function () {alert("销毁")}
});
vm.$mount('#app'); // 手动挂载绑定元素
vm.$destroy();// 实例销毁
``````

### 5.2 属性计算 computed
vue 实例中的this 指的是当前实例对象。
````
var vm = new Vue({
    data:{
        name:'zfpx'
    },
    computed:{
    hello:funciton(){
    return this.name+1111; //this 指的是实例vm
    }
    }//对象，可以定义多个属性计算方法
});
````

### 5.3 属性的获取和设置

````
var vm = new Vue({
    data:{
        name:'zfpx'
    },
    computed:{
    hello:
    {
      get:funciton(){
        returen this.name+1111;
      },
      set:function(val){
        this.name=val;
      }
    }
    }//对象，可以定义多个属性计算方法
});
````

## 6. vue 指令
### 6.1 v-if、 v-show 、v-else
````
v-if  逻辑指令。不满足条件时，标签被从dom中移除。可以同v-else一起使用，但是必须保证v-else指令
紧随其后。否则编译器无法解析识别。
v-if 支持template。 同时还可以与template 标签一起使用。解析时，template标签不被解析，只解析template包含的dom部分。

```

````
v-show 隐藏显示指令。 不满足条件时，内容被隐藏，标签样式display被设置成不可见。标签在dom中还存在。
v-show 也可以同v-else 同时使用。
v-show 不支持template。
v-else指令紧随其后。否则编译器无法解析识别。
````

````
将 v-show 用在组件上时，因为指令的优先级 v-else 会出现问题
用另一个 v-show 替换 v-else,可以达到 v-if 的效果.
````

````
v-if 也是惰性的：如果在初始渲染时条件为假，
则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。

一般来说，v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗。
因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。
````