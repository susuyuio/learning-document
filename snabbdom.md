# Snabbdom <span style="font-size:15px">_____ learing notes</span>

## BACKGROUND
- virtual dom library
- vue react等框架使用

## FEATURES
- #### Simple 200
    轻量，源码仅约200行
- #### 模块化，可扩展
    每个VNode和全局模块均有一组丰富的钩子，可连接到diff和patch过程的任何部分
- #### 是虚拟DOM基准测试中最快的虚拟DOM库之一
    其他的还有：……

## VNode
- ### sel : String
- ### data : Object
- ### children : Array<vnode>
- ### text : string
- ### key : string | number
``` js
h('div#app', 
  {
    class: {active: true},
    props: {className: 'container'},
    attrs: {href: '/foo', disabled: false},
    dataset: {action: 'reset'},
    style: {opacity: '0', transition: 'opacity 1s', delayed: {opacity: '1'}, remove: {opacity: '0'}, destroy: {opacity: '0'}},
    on: {click: [clickHandler, 5], change: handle}
  },
  [
    h('input', {props: {type: 'radio', name: 'test', value: '0'}}),
    h('input', {props: {type: 'radio', name: 'test', value: '1'}})
  ],
  'text content.',
  key: 'key'
)
```

## Core
- ### snabbdom.init([])
    接受模块列表，返回使用制定模块集的补丁函数patch
- ### patch()
    接受两个参数 (当前视图DOM/VNode, 更新视图VNode) </br>
    如果传递了具有父元素的DOM，将转换newNode为DOM </br>
    Snabbdom再VNode中存储信息，避免创建新的旧VNode树 </br>
- ### h()
    创建VNode
- ### toNode()
    将DOM节点转换为VNode,特别适用于修补存在的、服务器端生成的内容
- ### hook
    可用来扩展，或在生命周期中选择执行任意代码

| Name        | Triggered when                                     | Arguments to callback   |
| ----------- | --------------                                     | ----------------------- |
| `pre`       | the patch process begins                           | none                    |
| `init`      | a vnode has been added                             | `vnode`                 |
| `create`    | a DOM element has been created based on a vnode    | `emptyVnode, vnode`     |
| `insert`    | an element has been inserted into the DOM          | `vnode`                 |
| `prepatch`  | an element is about to be patched                  | `oldVnode, vnode`       |
| `update`    | an element is being updated                        | `oldVnode, vnode`       |
| `postpatch` | an element has been patched                        | `oldVnode, vnode`       |
| `destroy`   | an element is directly or indirectly being removed | `vnode`                 |
| `remove`    | an element is directly being removed from the DOM  | `vnode, removeCallback` |
| `post`      | the patch process is done                          | none                    |


## 第三方（插件？）功能

## 可优化／不足，影响