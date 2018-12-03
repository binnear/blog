本文主要是为了通过一些面试题(react16和其之前的版本对比)来梳理自己react的知识点，帮助自己更加了解react。

## DONE
- 描述对react理解？
- props和state的区别？
- 无状态组件和状态组件？

## DOING
- 关于React组件的生命周期？


## TO DO
 
- 生命周期的哪个阶段异步请求数据？
- key值的作用？你会怎样设置key值？
- react组件的划分业务组件技术组件？
- react性能优化方案？
- React 中 Element 与 Component 的区别是？
- 在什么情况下你会优先选择使用 Class Component 而不是 Functional Component？
- Controlled Component 与 Uncontrolled Component 之间的区别是什么？
- 为什么我们需要使用 React 提供的 Children API 而不是 JavaScript 的 map？
- 传入 setState 函数的第二个参数的作用是什么？
- JSX的有什么优点？
- 什么是react组件，通过什么方法定义?
- 除了在构造函数中绑定 this，还有其它方式吗？
- (在构造函数中)调用 super(props) 的目的是什么？
- 什么是pureComponent？
- 什么是HOC（Higher-Order Component）？适用于什么场景？
- createElement 与 cloneElement 的区别是什么？
- 调用setState后发生了什么？setState为什么是异步的？setState的两种使用方式？
- React如果处理事件绑定？
- 为什么虚拟dom会提高性能，diff算法?
- React 中 refs 的作用是什么？
- 简述flux 思想？
- 多个组件嵌套时，各组件声明周期调用顺序？
- 数组如何渲染到页面？
- context的理解？
- react16 有哪些变动？
- 什么是Fiber？是为了解决什么问题？
- 两个并不是父子关系的组件，如何实现相互的消息传递？请想出尽量多的办法，并说说各自的优缺点？
- 如果你能够改进React的一样功能，那会是哪一个功能？
- redux中间件，redux有什么缺点？
- redux-router的为什么可以在所有路由中的可以保存状态？
- vue和react的区别？
- 有没有看过什么令你眼前一亮的库，里面用到什么技术？
- vue和react的区别？


### 1. 描述对react理解？
    react官网给出的定义是一个用于构建用户界面的JavaScript库。它创建的虚拟DOM，通过diff算法，减少了我们直接用JS操作DOM时的一些不必要的消耗。通过react组件，我们可以更方便快速地构建应用。

### 2. props和state的区别？
    props是父组件传递给子组件的外部属性，在子组件中props是只读的，不能被修改。

    state是子组件内部的状态，可以通过setState方法进行修改。

### 3. 无状态组件和状态组件？
    无状态组件就是一个纯函数组件，形如：function Customer(props) {}，它没有自身的state，只有父组件传递过来的props属性。通常对于没有状态的组件建议写成无状态组件，因为它所有输入输出数据都是由props决定，而且不会产生副作用,是省略了渲染的时候组件类实例化的过程

    状态组件是一个继承React.Component的类，或是通过React.createClass()创建的类，它可以添加管理自身的state。

    如果能用无状态组件就尽量用无状态组件

### 4. 关于React组件的生命周期？
- React16版本之前的生命周期：
  - 子组件初始化：
    - constructor
      构造函数，在创建组件的时候调用一次，这里一般进行组件状态的初始赋值，方法绑定this。

    - componentWillMount
      在组件挂载之前调用一次，因为该函数是在组件render之前执行的，所以在该函数里调用setState不会发生重新渲染，通常推荐使用constructor代替，react不推荐在该方法中进行异步请求，因为我们不能保证请求结果一定是在render方法执行之前返回。它所使用的场景主要是服务端渲染发送ajax请求。

    - render
      挂载组件

    - componentDidMount
      在组件挂载之后调用一次，这个时候DOM已经渲染到页面了，其子组件也已经全部渲染到了页面，我们可以进行DOM的操作，获取ref，建议在这里进行远程数据的请求，调用setState更新状态后会触发组件重新渲染。

    - componentWillUnmount
      在组件卸载或销毁之前调用，一般用来进行一些必要的清理，比如无效的timers、interval，或者取消网络请求，或者清理任何在componentDidMount()中创建的DOM元素(elements);

  - 父组件render：
    - componentWillReceiveProps
        当父组件re-render时该钩子函数就会执行，即使所传入的属性没有改变。这个钩子最大的用途是：组件的部分状态是依赖于属性时做状态同步使用，在其中使用this.setState是不会触发额外的渲染的，this.setState的状态更新和props触发的render合并一次进行。

    - shouldComponentUpdate
      shouldComponentUpdate主要是用来优化React应用性能的，组件的状态或者属性改变时都会触发该函数，但只有在返回true时，组件才会被重新渲染，默认返回true。

    - componentWillUpdate
      当我们没有覆写componentShouldUpdate时，componentWillUpdate会在其之后立即执行。当shouldComponent被覆写过时，componentWillUpdate主要用来取代componentWillReceiveProps，用来同步Props至组件的部分状态。

    - render

    - componentDidUpdate
      它和componentDidMount的功能类似，componentDidMount发生于组件的首次render之后，而componentDidUpdate则是发生于组件状态及属性变化所导致的re-render之后。主要是用来请求后台数据。和componentWillReceiveProps类似，做相应处理时，需要做属性是否变更的判断，如下面代码所示。有趣的一点： componentWillReceiveProps接收的参数是nextProps, componentDidUpdate接收的是preProps。

    - componentWillUnmount

  - 子组件setState：
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate
    - componentWillUnmount

  - 子组件forceUpdate：
    使用forceUpdate会直接跳过shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate
    - componentWillUnmount

- React16版本的生命周期：

componentWillMount,componentWillReceiveProps,componentWillUpdate以上三个生命周期在react17将被废弃，新增了他们对应的UNSAFE_componentWillMount,UNSAFE_componentWillReceiveProps,UNSAFE_componentWillUpdate三个方法，之所以废弃这三个方法，是因为react官方推出的fiber架构，在react17将支持异步渲染，那时生命周期可被打断执行，（生命周期被打断，下次恢复的时候就需要再次执行一次之前被打断的生命周期，这样就导致某个生命周期函数被执行多次），其打断的阶段正式构建虚拟dom阶段，也就是被去除的这三个生命周期。所以我们要尽量避免使用它们，用官方新提供的生命周期取代替。

  - init：
    - constructor
      构造函数，和之前版本没有什么改变

    - static getDerivedStateFromProps
      一个静态方法，所以不能在这个函数里面使用this，这个函数有两个参数props和state，分别指接收到的新参数和当前的state对象，这个函数会返回一个对象用来更新当前的state对象，如果不需要更新可以返回null，官方新增这个方法是为了替代将被废弃的三个生命周期方法

    - componentWillMount/UNSAVE_componentWillMount
    - render

  - update:
    - componentWillReceiveProps/UNSAFE_componentWillReceiveProps

    - static getDerivedStateFromProps (该方法在16.3之前setState不会触发，16.4及之后setState也会触发)

    - shouldComponentUpdate
      官方提倡我们使用PureComponent来减少重新渲染的次数而不是手工编写shouldComponentUpdate代码，在未来的版本，shouldComponentUpdate返回false，仍然可能导致组件重新的渲染
    - componentWillUpdate/UNSAFE_componentWillUpdate
      
    - render

    - getSnapshotBeforeUpdate
      这个方法在render之后，componentDidUpdate之前调用，有两个参数prevProps和prevState，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给componentDidUpdate，如果你不想要返回值，返回null，这个方法一定要和componentDidUpdate一起使用，否则控制台也会有警告

    - componentDidUpdate
  
  - unmount:
    - componentWillUnmount


###  生命周期的哪个阶段异步请求数据？
###  key值的作用？你会怎样设置key值？
###  react组件的划分业务组件技术组件？
###  react性能优化方案？
###  React 中 Element 与 Component 的区别是？
###  在什么情况下你会优先选择使用 Class Component 而不是 Functional Component？
###  Controlled Component 与 Uncontrolled Component 之间的区别是什么？
###  为什么我们需要使用 React 提供的 Children API 而不是 JavaScript 的 map？
###  传入 setState 函数的第二个参数的作用是什么？
###  JSX的有什么优点？
###  什么是react组件，通过什么方法定义?
###  除了在构造函数中绑定 this，还有其它方式吗？
###  (在构造函数中)调用 super(props) 的目的是什么？
###  什么是pureComponent？
###  什么是HOC（Higher-Order Component）？适用于什么场景？
###  createElement 与 cloneElement 的区别是什么？
###  调用setState后发生了什么？setState为什么是异步的？setState的两种使用方式？
###  React如果处理事件绑定？
###  为什么虚拟dom会提高性能，diff算法?
###  React 中 refs 的作用是什么？
###  简述flux 思想？
###  多个组件嵌套时，各组件声明周期调用顺序？
###  数组如何渲染到页面？
###  context的理解？
###  react16 有哪些变动？
###  什么是Fiber？是为了解决什么问题？
###  两个并不是父子关系的组件，如何实现相互的消息传递？请想出尽量多的办法，并说说各自的优缺点？
###  如果你能够改进React的一样功能，那会是哪一个功能？
###  redux中间件，redux有什么缺点？
###  redux-router的为什么可以在所有路由中的可以保存状态？
###  有没有看过什么令你眼前一亮的库，里面用到什么技术？
###  vue和react的区别？