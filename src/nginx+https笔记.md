---
title: 'nginx+https笔记'
date: 2019-10-11
---

### 前言

你需要有已经安装的nginx 并且有相应的基础 如果没有请移步 我的文章安装

### 使用感觉

由于基于typescript的npm包并不是很多，所以大部分的插件等都需要自己手动完成，vue/cli3.0自带的ts比较弱，语法检查上面还有很多待改进的地方！

我是直接基于脚手架附带的ts 环境（scss+router+ts+vuex）

### 入门教程 （简单用法 需vue基础）

>搭建环境 

本文主要是讲用法 如需搭建环境请前往地址[vue/cli官网](https://cli.vuejs.org/guide/creating-a-project.html#vue-create)

>定义变量 --标签访问不变 {{name}} {{sex}}
```javascript
    // 老版本
    new Vue({
        data: {
            name:1,
            sex:2
        }
    })


    // 新版本
    export default class ConTact extends Vue {
        // public可以省略
        public sex = 20
        name = 1

        mounted(){
            //访问变量需要加this
            console.log(this.sex) // 20
        }
        ....
    }


    // ts版本 -- 更多方法参考ts官方文档 本文只提供实例
    export default class ConTact extends Vue {
        // public可以省略
        public sex:number = 20
        name:string = '12'
        ....
    }
```
#### 注：访问需要加this #####
#### 总结：变量现在不需要写在data里面了 可以定义在上面 public同样可以省略， ####


>生命周期的更改
```javascript
    // 老版本
    new Vue({
        data: {
        },
        mounted(){

        },
        created(){

        }
        ....
    })

    // 新版本
    export default class ConTact extends Vue {
        // public可以省略
        public mounted(num) {
            
        }

        created(num) {
            return 1 + num
        }

        ....
    }
```

#### 总结：生命周期可以直接写在，public/private/protected等可以省略 ####

> vue/cli3.0删除计算属性更改为get

   由于项目中需要使用使用counted用来计算结果，但是在vue/cli3.0的方法中 直接定义counted会是成为一个this变量并不会当做生命周期来使用！
```javascript

    <!-- 老版本 -->

    new Vue({
        el: '#app',
        data: {
        },
        computed: {
            // 计算属性的 getter
            reversedMessage: function () {
            // `this` 指向 vm 实例
            return this.message.split('').reverse().join('')
            }
        }
    })

    <!-- vue/cli3.0写法 -->
    export default class ConTact extends Vue {
        constructor() {
            super();
        }
        <!-- get xxx进行监听 -->
        get userid(num) {
            return 1 + num
        }
    }
```
#### 总结：使用进行 get xxx(){}进行监听数据 后面跟函数 其他操作正常进行 ####


> [vue-property-decorator](https://juejin.im/post/5c173a84f265da610e7ffe44) 使用了这个的同学需要移步到这个查看里面的方法， 方法包含有

@Component (from vue-class-component) <br/>
@Prop<br/>
@Model<br/>
@Watch<br/>
@Emit<br/>
@Inject<br/>
@Provide<br/>
Mixins (the helper function named mixins defined at vue-class-component)<br/>

本文就不一一介绍了

推荐文章：[vue-property-decorator](https://juejin.im/post/5c173a84f265da610e7ffe44)
  



### 采坑技巧

如果你在代码中遇到了这样的问题

![charles 1](../../src/images/1570699130(1).jpg)

![charles 1](../../src/images/1570699186(1).jpg)

报错为 Property '$router' does not exist on type 'ConTact'（属性“$router”在类型“ConTact”上不存在）
很明显报错为属性不存在 尝试为他添加一个属性 下图

![charles 1](../../src/images/1570699231(1).jpg)
就解决了

相同问题的好友：$route/$nextTick/..... 等 只要基于vue的原型 大部分都找不到 所以需要手动去添加，当然不改也没事 不影响效果！

#### 问题主要是typescript中的this指向本身 只要本身没有他就没有 他不会去查询其他地方的数据 ####
### 总结

typescript对vue/cli3.0现在兼容的还真的有点少！同NPM包应用较少

总之失误挺多的，经验不足也导致反应不够迅速，处置手段也比较具有侵入性。

技术进阶之路漫漫，我还要继续努力。