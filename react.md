# React
### `1. Basic knowledge`
- 1个概念 —— 组件
- 4个必须API
- 单向数据流
- 完美的错误提示
- Flux架构 —— 单向数据流（状态管理框架：Redux MobX）

### `2. Point`
- 受控组件、 非受控组件
- 单一职责原则（创建组件）
- DRY原则（数据状态管理）

### `3. JSX`
- 不是模版语言，是一种语法糖
- 在JS代码中写HTML标记

### `4. 生命周期`
- 三个阶段
    - Render
    - Pre-commit
    - Commit
- 三个类型
    - 创建时
        - constructor、componentDidMount
    - 更新时
        - getDerivedStateFromProps、shouldComponentUpdate、render、getSnaphotBeforeUpdate、componentDidUpdate
    - 卸载时
        - componentWillUnmount

- constructor
    >a. 用于初始化内部状态，很少使用<br>
    >b. 唯一可以直接修改state的地方(this.state.xxx)
- getDerivedStateFromProps
    >a. 当state需要从props初始化时使用
    >b. 尽量不要使用，computed可更好的保持状态一致性，减少复杂度
    >c. 每次render都会调用
    >d. 典型场景：表单控件获取默认值
- componentDidMount
    >a. UI渲染完成时调用
    >b. 只执行一次
    >c. 典型场景：获取外部资源
- componentWillUnmount
    >a. 组件移除时被调用
    >b. 典型场景：释放资源
- getSnapshotBeforeUpdate
    >a. 在页面render之前调用，state已更新
    >b. 典型场景：获取render之前的DOM状态（why?）
- componentDidUpdate
    >a. 每次UI更新时被调用
    >b. 典型场景：页面需要根据props变化重新获取数据
- shouldComponentUpdate
    >a. 决定Virtual Dom是否重绘
    >b. 一般由PureComponent自动实现
    >c. 典型场景：性能优化

### `5. Virtual DOM`
- 广度优先分层比较
- 算法复杂度为O(n)
- diff计算、key属性

### `6. 组件复用（设计模式）`
- 高阶组件（复用计时器）
- 函数作为子组件（选择不同颜色展示）

### `7. context API`

### `8. 使用脚手架创建React应用`
- Create React App (simple)
- Codesandbox (online)
- Rekit (intact)

### `9. 打包部署（Webpack）`
- ES6/JSX编译、整合资源（图片/Less/Sass）、优化代码体积
- 注意事项
    - nodejs环境为production
    - 禁用开发时换用代码（logger）
    - 设置应用路径

### `10. Redux状态管理（通用独立状态管理框架）`
- 让组件通信更容易
- 特性
    - Single Source of truth
    - 可预测性
    - 纯函数更新store（生成新store）
- Store
    - 基本概念
        - getState()
        - dispatch(action)
        - subscribe(listener)
    - bindActionCreators // 封装dispatch-action
    - combineReducers // 组合Reduce
- connect（高阶组件设计模式，连接组件与store）
    - mapStateToProps
    - mapDispatchToProps
- 异步处理（action的设计模式，多个同步action组合）
    - MiddleWare中间件，在dispatcher层
        - 截获action（请求）
        - 发出action（结果）
- 如何组织action和reducer
    - 按照业务行为分成多个小文件共同管理相关action和redeucer
    - 在分别导入action和reducer的整体文件
- 不可变数据
    - 优点
        - 性能优化（仅通过引用比较是否一直）
        - 易于调试和跟踪
        - 易于推测
    - 使用
        - {...}; Object.assign({}, val)
        - immutability-helper
        - immer

### `11. React Router`
- 三种实现方式
    - URL路径
    - hash路由
    - 内存路由<Memory-Router>
- API
    - <Link> 普通链接 不触发浏览器刷新
    - <NavLink> 在Link基础上添加选中class
    - <Prompt> 满足条件时提示用户是否离开当前页
    - <Redirect> 重定向，例如登录判断
    - <Route> 路径匹配时显示对应组件
        - exact 精确匹配
        - 不排他，都满足条件均显示
        - prefetch 预加载
        - replace 替换页面
    - <Switch> 只显示第一个匹配的路由
- 参数
    - 传参 path = "/topic/:id"
    - 获取 this.props.match.params

### `12. UI组件库`
- Ant.Design 蚂蚁金服
- Material UI 谷歌
- Semantic UI

### `13. 使用Next.js创建同构应用`
- 服务器端渲染
- getInitialProps 
- LazyLoad => dynamic(import('../component')

### `14. 使用Jest、Enzyme等工具进行单元测试`
- 涉及工具
    - Jest: Facebook开源JS单元测试框架
    - JS DOM: 浏览器环境的NodeJS模拟
    - Enzyme: React组件渲染和测试
    - nock: 模拟HTTP请求
    - sinon: 函数模拟和调用跟踪
    - istanbul: 单元测试覆盖率

### `15. 常用开发调试工具`
- ESLint 语法风格检查
- Prettier 格式化代码工具
- React DevTool
- Redux DevTool

### `16. Rekit`
- 工具集和IDE
