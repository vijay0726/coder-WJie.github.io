---
title: vue响应式原理—依赖技术的分析和学习
---

#### 图解:
![vue响应式原理图解](https://img-blog.csdnimg.cn/20210307112633570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjU2NzQyOA==,size_16,color_FFFFFF,t_70#pic_center)

```html
<body>
    <!-- 
        1.app.message修改数据。Vue内部是如何监听message数据的改变
        * Object.defineProperty -> 监听对象属性的改变

        2.当数据发生改变，Vue是如何知道要通知哪些人，界面发生刷新。
        * 发布订阅者模式
     -->
    <div id="app">
        <div>{{message}}</div>
        <div>{{message}}</div>
        <div>{{message}}</div>
    </div>

    <script>
        const obj = {
            message: '哈哈哈',
            name: 'wangjie'
        }
		*
        Object.keys(obj).forEach(key => {
            let value = obj[key]

            Object.defineProperty(obj, key, {
            	//每次给obj[key]赋值都调用set()
                set(newValue) {
                    console.log('监听' + key + '改变');
                    //根据解析html代码，获取到那些人有用属性
                    value = newValue
                    dep.notify()
                },
                //每次获取obj[key]值都调用get()方法
                get() {
                    return value
                }
            })
        })
        //发布者订阅者模式
        //发布者
        class Dep {
            constructor() {
            //将所有订阅者存在subscribes数组中
                this.subscribe = []
            }
            
            addSub(watcher) {
                this.subscribe.push(watcher)
            }
			//“通知”所有订阅者，即更新界面发生刷新的地方
            notify() {
                this.subscribe.forEach(item => {
                    item.update()
                })
            }
        }

        //订阅者
        class Watcher {
            constructor(name) {
                this.name = name
            }

            update() {
                console.log(this.name + '发生update')
            }
        }

        const dep = new Dep()

        const w1 = new Watcher('张三')
        dep.addSub(w1)

        const w2 = new Watcher('李四')
        dep.addSub(w2)

    </script>
    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: '哈哈哈',
                name: 'wangjie'
            }
        })
    </script>
</body>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210306181211298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjU2NzQyOA==,size_16,color_F7B4B6,t_70#pic_center)

