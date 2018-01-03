# 双向绑定
先上一张流程图

![vue执行流程及双向绑定](vue执行流程及双向绑定.png)
new Vue()
this_init()
initState()
initData()
observe()
new Observer()
this.walk()
defineReactive()
new Dep() 
Object.defineProperty()
get:
dep.depend()
set:
dep.notify()
