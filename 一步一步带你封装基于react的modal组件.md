中秋放假，一个人有点无聊，于是写点博文暖暖心，同时祝大家中秋快乐~ 🙃
  
接下来将一步步带领大家实现一个基本的modal弹窗组件，封装一个简单的动画组件，其中涉及到的一些知识点也会在代码中予以注释讲解。

## 一. modal组件的实现；

### 1. 环境搭建

我们使用create-react-app指令，快速搭建开发环境：

```bash
create-react-app modal
```

安装完成后，按照提示启动项目，接着在`src`目录下新建`modal`目录，同时创建`modal.jsx modal.css`两个文件

`modal.jsx`内容如下：

```javascript
import React, { Component } from 'react';
import './modal.css';
class Modal extends Component {
  render() {
    return <div className="modal">
      这是一个modal组件
    </div>
  }
}
export default Modal;
```

回到根目录，打开App.js，将其中内容替换成如下：

```javascript
import Modal from './modal/modal';
import React, { Component } from 'react';
import './App.css';
class App extends Component {
  render() {
    return <div className="app">
      <Modal></Modal>
    </div>
  }
}
export default App;
```

完成以上步骤后，我们浏览器中就会如下图显示了：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ff5b9da810390?w=937&h=509&f=png&s=24543)

### 2. modal样式完善

写之前，我们先回想一下，我们平时使用的modal组件都有哪些元素，一个标题区，内容区，还有控制区，一个mask；

`modal.jsx`内容修改如下：

```javascript
import React, { Component } from 'react';
import './modal.css';
class Modal extends Component {
  render() {
    return <div className="modal-wrapper">
      <div className="modal">
        <div className="modal-title">这是modal标题</div>
        <div className="modal-content">这是modal内容</div>
        <div className="modal-operator">
          <button className="modal-operator-close">取消</button>
          <button className="modal-operator-confirm">确认</button>
        </div>
      </div>
      <div className="mask"></div>
    </div>
  }
}
export default Modal;
```

`modal.css`内容修改如下：

```css
.modal {
  position: fixed;
  width: 300px;
  height: 200px;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  border-radius: 5px;
  background: #fff;
  overflow: hidden;
  z-index: 9999;
}

.modal-title {
  width: 100%;
  height: 50px;
  line-height: 50px;
  padding: 0 10px;
}

.modal-content {
  width: 100%;
  height: 100px;
  padding: 0 10px;
}

.modal-operator {
  width: 100%;
  height: 50px;
}

.modal-operator-close, .modal-operator-confirm {
  width: 50%;
  border: none;
  outline: none;
  height: 50px;
  line-height: 50px;
  opacity: 1;
  color: #fff;
  background: rgb(247, 32, 32);
  cursor: pointer;
}

.modal-operator-close:active, .modal-operator-confirm:active {
  opacity: .6;
  transition: opacity .3s;
}

.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: #000;
  opacity: .6;
  z-index: 9998;
}
```

修改完成后，我们浏览器中就会如下图显示：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ff5a18886240c?w=937&h=509&f=png&s=26747)

### 3. modal功能开发

到这里我们的准备工作已经完成，接下就具体实现modal功能，再次回想，我们使用modal组件的时候，会有哪些基本的功能呢？

1. 可以通过`visible`控制`modal`的显隐；
2. `title`,`content`可以自定义显示内容；
3. 点击取消关闭`modal`，同时会调用名为`onClose`的回调，点击确认会调用名为`confirm`的回调，并关闭`modal`，点击蒙层`mask`关闭`modal`；
5. animate字段可以开启/关闭动画；

#### 3.1. 添加`visible`字段控制显隐

`modal.jsx`修改如下：

```javascript
import React, { Component } from 'react';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
  }

  render() {
    // 通过父组件传递的visile控制显隐
    const { visible } = this.props;
    return visible && <div className="modal-wrapper">
      <div className="modal">
        <div className="modal-title">这是modal标题</div>
        <div className="modal-content">这是modal内容</div>
        <div className="modal-operator">
          <button className="modal-operator-close">取消</button>
          <button className="modal-operator-confirm">确认</button>
        </div>
      </div>
      <div className="mask"></div>
    </div>
  }
}
export default Modal;
```

`App.js`修改如下：

```javascript
import Modal from './modal/modal';
import React, { Component } from 'react';

import './App.css';
class App extends Component {
  constructor(props) {
    super(props)
    // 这里绑定this因为类中的方法不会自动绑定指向当前示例，我们需要手动绑定，不然方法中的this将是undefined，这是其中一种绑定的方法，
    // 第二种方法是使用箭头函数的方法，如：showModal = () => {}
    // 第三种方法是调用的时候绑定，如：this.showModal.bind(this)
    this.showModal = this.showModal.bind(this)  
    this.state = {
      visible: false
    }
  }

  showModal() {
    this.setState({ visible: true })
  }

  render() {
    const { visible } = this.state
    return <div className="app">
      <button onClick={this.showModal}>click here</button>
      <Modal visible={visible}></Modal>
    </div>
  }
}
export default App;
```

以上我们通过父组件`App.js`中的visible状态，传递给`modal`组件，再通过`button`的点击事件来控制visible的值以达到控制`modal`组件显隐的效果
  
未点击按钮效果如下图：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ff6cf1e0aa33b?w=937&h=509&f=png&s=23996)

点击按钮后效果如下图：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ff6dc37bb4f9c?w=937&h=509&f=png&s=28102)

#### 3.2. `title`与`content`内容自定义

`modal.jsx`修改如下：

```javascript
import React, { Component } from 'react';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
  }

  render() {
    const { visible, title, children } = this.props;
    return visible && <div className="modal-wrapper">
      <div className="modal">
        {/* 这里使用父组件的title*/}
        <div className="modal-title">{title}</div>
        {/* 这里的content使用父组件的children*/}
        <div className="modal-content">{children}</div>
        <div className="modal-operator">
          <button className="modal-operator-close">取消</button>
          <button className="modal-operator-confirm">确认</button>
        </div>
      </div>
      <div className="mask"></div>
    </div>
  }
}
export default Modal;
```

`App.js`修改如下：

```javascript
import Modal from './modal/modal';
import React, { Component } from 'react';

import './App.css';
class App extends Component {
  constructor(props) {
    super(props)
    this.showModal = this.showModal.bind(this)
    this.state = {
      visible: false
    }
  }

  showModal() {
    this.setState({ visible: true })
  }

  render() {
    const { visible } = this.state
    return <div className="app">
      <button onClick={this.showModal}>click here</button>
      <Modal
        visible={visible}
        title="这是自定义title"
      >
        这是自定义content
      </Modal>
    </div>
  }
}
export default App;
```

接着我们点击页面中的按钮，结果显示如下：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ff7a2212142c1?w=937&h=509&f=png&s=28330)

#### 3.3. 取消与确认按钮以及蒙层点击功能添加

> 写前思考：我们需要点击取消按钮关闭`modal`，那么我们就需要在`modal`中维护一个状态，然后用这个状态来控制`modal`的显隐，好像可行，但是我们再一想，我们前面是通过父组件的`visible`控制`modal`的显隐，这样不就矛盾了吗？这样不行，那我们作一下改变，如果父组件的状态改变，那么我们只更新这个状态，`modal`中点击取消我们也只更新这个状态，最后用这个状态值来控制`modal`的显隐；至于`onClose`钩子函数我们可以再更新状态之前进行调用，确认按钮的点击同取消。

`modal.jsx`修改如下：

```javascript
import React, { Component } from 'react';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.maskClick = this.maskClick.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.state = {
      visible: false
    }
  }

  // 首次渲染使用父组件的状态更新modal中的visible状态，只调用一次
  componentDidMount() {
    this.setState({ visible: this.props.visible })
  }

  // 每次接收props就根据父组件的状态更新modal中的visible状态，首次渲染不会调用
  componentWillReceiveProps(props) {
    this.setState({ visible: props.visible })
  }

  // 点击取消更新modal中的visible状态
  closeModal() {
    console.log('大家好，我叫取消，听说你们想点我？傲娇脸👸')
    const { onClose } = this.props
    onClose && onClose()
    this.setState({ visible: false })
  }

  confirm() {
    console.log('大家好，我叫确认，楼上的取消是我儿子，脑子有点那个~')
    const { confirm } = this.props
    confirm && confirm()
    this.setState({ visible: false })
  }

  maskClick() {
    console.log('大家好，我是蒙层，我被点击了')
    this.setState({ visible: false})
  }

  render() {
    // 使用modal中维护的visible状态来控制显隐
    const { visible } = this.state;
    const { title, children } = this.props;
    return visible && <div className="modal-wrapper">
      <div className="modal">
        <div className="modal-title">{title}</div>
        <div className="modal-content">{children}</div>
        <div className="modal-operator">
          <button
            onClick={this.closeModal}
            className="modal-operator-close"
          >取消</button>
          <button
            onClick={this.confirm}
            className="modal-operator-confirm"
          >确认</button>
        </div>
      </div>
      <div
        className="mask"
        onClick={this.maskClick}
      ></div>
    </div>
  }
}
export default Modal;
```

`App.js`修改如下：

```javascript
import Modal from './modal/modal';
import React, { Component } from 'react';

import './App.css';
class App extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.showModal = this.showModal.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.state = {
      visible: false
    }
  }

  showModal() {
    this.setState({ visible: true })
  }

  closeModal() {
    console.log('我是onClose回调')
  }

  confirm() {
    console.log('我是confirm回调')
  }

  render() {
    const { visible } = this.state
    return <div className="app">
      <button onClick={this.showModal}>click here</button>
      <Modal
        visible={visible}
        title="这是自定义title"
        confirm={this.confirm}
        onClose={this.closeModal}
      >
        这是自定义content
      </Modal>
    </div>
  }
}
export default App;
```

保存后，我们再浏览器中分别点击取消和确认，控制台中将会出现如下图所示：

![](https://user-gold-cdn.xitu.io/2018/9/22/165ffa878ab3ea26?w=937&h=509&f=png&s=41400)

### 4. modal优化

以上就完成了一个基本的`modal`组件，但是我们还有一个疑问，就是现在引入的`modal`是在类名为`App`的元素之中，而一些被广泛使用的UI框架中的`modal`组件确实在`body`层，无论你在哪里引入，这样就可以防止`modal`组件受到父组件的样式的干扰。
  
而想要实现这种效果，我们必须得先了解React自带的特性：`Portals`(传送门)。这个特性是在16版本之后添加的，而在16版本之前，都是通过使用`ReactDOM`的`unstable_renderSubtreeIntoContainer`方法处理，这个方法可以将元素渲染到指定元素中，与`ReactDOM.render`方法的区别就是，可以保留当前组件的上下文`context`，`react-redux`就是基于`context`进行跨组件之间的通信，所以若是使用`ReactDOM.render`进行渲染就会导致丢失上下文，从而导致所有基于`context`实现跨组件通信的框架失效。

#### 4.1. `ReactDOM.unstable_renderSubtreeIntoContainer`的使用

```javascript
ReactDOM.unstable_renderSubtreeIntoContainer(
  parentComponent, // 用来指定上下文
  element,         // 要渲染的元素
  containerNode,   // 渲染到指定的dom中
  callback         // 回调
);
```

接下来在我们的项目中使用它，`src`目录下新建`oldPortal`目录，并在其中新建`oldPortal.jsx`，`oldPortal.jsx`中的内容如下：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class OldPortal extends React.Component {
  constructor(props) {
    super(props)
  }

  // 初始化时根据visible属性来判断是否渲染
  componentDidMount() {
    const { visible } = this.props
    if (visible) {
      this.renderPortal(this.props);
    }
  }

  // 每次接受到props进行渲染与卸载操作
  componentWillReceiveProps(props) {
    if (props.visible) {
      this.renderPortal(props)
    } else {
      this.closePortal()
    }
  }

  // 渲染
  renderPortal(props) {
    if (!this.node) {
      // 防止多次创建node
      this.node = document.createElement('div');
    }
    // 将当前node添加到body中
    document.body.appendChild(this.node);

    ReactDOM.unstable_renderSubtreeIntoContainer(
      this,           // 上下文指定当前的实例
      props.children, // 渲染的元素为当前的children
      this.node,      // 将元素渲染到我们新建的node中,这里我们不使用第四个参数回调.
    );
  }

  // 卸载
  closePortal() {
    if (this.node) {
      // 卸载元素中的组件
      ReactDOM.unmountComponentAtNode(this.node)
      // 移除元素
      document.body.removeChild(this.node)
    }
  }

  render() {
    return null;
  }
}

export default OldPortal
```

保存后,我们在`modal.jsx`中使用它:

```javascript
import React, { Component } from 'react';
import OldPortal from '../oldPortal/oldPortal';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.maskClick = this.maskClick.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.state = {
      visible: false
    }
  }

  componentDidMount() {
    this.setState({ visible: this.props.visible })
  }

  componentWillReceiveProps(props) {
    this.setState({ visible: props.visible })
  }

  closeModal() {
    console.log('大家好，我叫取消，听说你们想点我？傲娇脸👸')
    const { onClose } = this.props
    onClose && onClose()
    this.setState({ visible: false })
  }

  confirm() {
    console.log('大家好，我叫确认，楼上的取消是我儿子，脑子有点那个~')
    const { confirm } = this.props
    confirm && confirm()
    this.setState({ visible: false })
  }

  maskClick() {
    console.log('大家好，我是蒙层，我被点击了')
    this.setState({ visible: false })
  }

  render() {
    const { visible } = this.state;
    const { title, children } = this.props;
    return <OldPortal visible={visible}>
      <div className="modal-wrapper">
        <div className="modal">
          <div className="modal-title">{title}</div>
          <div className="modal-content">{children}</div>
          <div className="modal-operator">
            <button
              onClick={this.closeModal}
              className="modal-operator-close"
            >取消</button>
            <button
              onClick={this.confirm}
              className="modal-operator-confirm"
            >确认</button>
          </div>
        </div>
        <div
          className="mask"
          onClick={this.maskClick}
        ></div>
      </div>
    </OldPortal>
  }
}
export default Modal;
```

可以看到,我们仅仅是在`modal`中`return`的内容外层包裹一层`OldPortal`组件,然后将控制显隐的状态`visible`传递给了`OldPortal`组件,由`OldPortal`来实际控制`modal`的显隐;然后我们点击页面中的按钮,同时打开控制台,发现`modal`如我们所想,床送到了`body`层:

![](https://user-gold-cdn.xitu.io/2018/9/22/1660059697d76237?w=1090&h=616&f=png&s=84206)

#### 4.2. 16版本`Portal`使用

在16版本中,`react-dom`原生提供了一个方法`ReactDOM.createPortal()`,用来实现传送门的功能:

```javascript
ReactDOM.createPortal(
  child,    // 要渲染的元素
  container // 指定渲染的父元素
)
```

参数比之`unstable_renderSubtreeIntoContainer`减少了两个,接着我们在项目中使用它.
  
在`src`目录下新建`newPortal`目录,在其中新建`newPortal.jsx`,`newPortal.jsx`内容如下:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class NewPortal extends React.Component {
  constructor(props) {
    super(props)
    // 初始化创建渲染的父元素并添加到body下
    this.node = document.createElement('div');
    document.body.appendChild(this.node);
  }

  render() {
    const { visible, children } = this.props;
    // 直接通过显隐表示
    return visible && ReactDOM.createPortal(
      children,
      this.node,
    );
  }
}
export default NewPortal
```

可以很清晰的看到内容对比`unstable_renderSubtreeIntoContainer`的实现简化了很多,然后我们在`modal.jsx`中使用:

```javascript
import React, { Component } from 'react';
import NewPortal from '../newPortal/newPortal';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.maskClick = this.maskClick.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.state = {
      visible: false
    }
  }

  componentDidMount() {
    this.setState({ visible: this.props.visible })
  }

  componentWillReceiveProps(props) {
    this.setState({ visible: props.visible })
  }

  closeModal() {
    console.log('大家好，我叫取消，听说你们想点我？傲娇脸👸')
    const { onClose } = this.props
    onClose && onClose()
    this.setState({ visible: false })
  }

  confirm() {
    console.log('大家好，我叫确认，楼上的取消是我儿子，脑子有点那个~')
    const { confirm } = this.props
    confirm && confirm()
    this.setState({ visible: false })
  }

  maskClick() {
    console.log('大家好，我是蒙层，我被点击了')
    this.setState({ visible: false })
  }

  render() {
    const { visible } = this.state;
    const { title, children } = this.props;
    return <NewPortal visible={visible}>
      <div className="modal-wrapper">
        <div className="modal">
          <div className="modal-title">{title}</div>
          <div className="modal-content">{children}</div>
          <div className="modal-operator">
            <button
              onClick={this.closeModal}
              className="modal-operator-close"
            >取消</button>
            <button
              onClick={this.confirm}
              className="modal-operator-confirm"
            >确认</button>
          </div>
        </div>
        <div
          className="mask"
          onClick={this.maskClick}
        ></div>
      </div>
    </NewPortal>
  }
}
export default Modal;
```

使用上与`OldPortal`一样,接下来看看浏览器中看看效果是否如我们所想:

![](https://user-gold-cdn.xitu.io/2018/9/22/166006d66704d4fe?w=1090&h=616&f=png&s=76480)

可以说`Portals`是弹窗类组件的灵魂,这里对`Portals`的使用仅仅是作为一个引导,讲解了其核心功能,并没有深入去实现一些复杂的公共方法,有兴趣的读者可以搜索相关的文章,都有更详细的讲解.

## 二. 出入场动画实现

### 1. 动画添加

从一个简单的效果开始(使用的代码是以上使用`NewPortal`组件的`Modal`组件),`modal`弹出时逐渐放大,放大到1.1倍,最后又缩小到1倍,隐藏时,先放大到1.1倍,再缩小,直到消失.
  
惯例先思考: 我们通过控制什么达到放大缩小的效果?我们如何将放大和缩小这个过程从瞬间变为一个渐变的过程?我们在什么时候开始放大缩小?又在什么时候结束放大缩小?
  
放大和缩小我们通过`css3`的属性`transform scale`进行控制,渐变的效果使用`transition`过度似乎是不错的选择,而放大缩小的时机,分为元素开始出现,出现中,出现结束,开始消失,消失中,消失结束六种状态,然后我们分别定义这六种状态的`scale`参数,再使用`transition`进行过度,应该就能实现我们需要的效果了:
  
再`modal.css`添加如下代码:

```css
.modal-enter {
  transform: scale(0);
}

.modal-enter-active {
  transform: scale(1.1);
  transition: all .2s linear;
}

.modal-enter-end {
  transform: scale(1);
  transition: all .1s linear;
}

.modal-leave {
  transform: scale(1);
}

.modal-leave-active {
  transform: scale(1.1);
  transition: all .1s linear;
}

.modal-leave-end {
  transform: scale(0);
  transition: all .2s linear;
}
```

六种类名分别定义了出现与消失的六种状态,同时设置了各自的过度时间,接下来我们就在不同的过程给元素添加对应的类名,就能控制元素的显示状态了.
  
在我们写逻辑之前,我们还需要注意一点,之前我们组件的显隐是在`NewPortal`组件中实际控制的,但是我们在`Modal`组件中添加动画,就需要严格掌控显隐的时机,比如刚渲染就要开始动画,动画结束之后才能隐藏,这样就不适合在`NewPortal`组件中控制显隐了.有的读者就疑惑了,为什么不直接在`NewPortal`组件中添加动画呢?当然这个问题的答案是肯定的,但是`NewPortal`的功能是传送,并不复杂动画,我们要保持它的纯净,不宜与其他组件耦合.
  
修改`newPortal.jsx`的内容如下: 

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class NewPortal extends React.Component {
  constructor(props) {
    super(props)
    this.node = document.createElement('div');
    document.body.appendChild(this.node);
  }

  render() {
    const { children } = this.props;
    return ReactDOM.createPortal(
      children,
      this.node,
    );
  }
}
export default NewPortal
```

修改`modal.jsx`的内容如下:

```javascript
import React, { Component } from 'react';
import NewPortal from '../newPortal/newPortal';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.maskClick = this.maskClick.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.leaveAnimate = this.leaveAnimate.bind(this)
    this.enterAnimate = this.enterAnimate.bind(this)
    this.state = {
      visible: false,
      classes: null,
    }
  }

  componentDidMount() {
    this.setState({ visible: this.props.visible })
  }

  componentWillReceiveProps(props) {
    if (props.visible) {
      // 接收到父组件的props时,如果是true则进行动画渲染
      this.enterAnimate()
    }
  }

  // 进入动画
  enterAnimate() {
    // 这里定义每种状态的类名,就是我们之前modal.css文件中添加的类
    const enterClasses = 'modal-enter'
    const enterActiveClasses = 'modal-enter-active'
    const enterEndActiveClasses = 'modal-enter-end'
    // 这里定义了每种状态的过度时间,对应着modal.css中对应类名下的transition属性的时间,这里的单位为毫秒
    const enterTimeout = 0
    const enterActiveTimeout = 200
    const enterEndTimeout = 100
    // 将显隐状态改为true,同时将classes改为enter状态的类名
    this.setState({ visible: true, classes: enterClasses })
    // 这里使用定时器,是因为定时器中的函数会被加入到事件队列,带到主线程任务进行完成才会被调用,相当于在元素渲染出来并且加上初始的类名后enterTimeout时间后开始执行.
    // 因为开始状态并不需要过度,所以我们直接将之设置为0.
    const enterActiveTimer = setTimeout(_ => {
      this.setState({ classes: enterActiveClasses })
      clearTimeout(enterActiveTimer)
    }, enterTimeout)
    const enterEndTimer = setTimeout(_ => {
      this.setState({ classes: enterEndActiveClasses })
      clearTimeout(enterEndTimer)
    }, enterTimeout + enterActiveTimeout)

    // 最后将类名置空,还原元素本来的状态
    const initTimer = setTimeout(_ => {
      this.setState({ classes: '' })
      clearTimeout(initTimer)
    }, enterTimeout + enterActiveTimeout + enterEndTimeout)
  }

  // 离开动画
  leaveAnimate() {
    const leaveClasses = 'modal-leave'
    const leaveActiveClasses = 'modal-leave-active'
    const leaveEndActiveClasses = 'modal-leave-end'
    const leaveTimeout = 0
    const leaveActiveTimeout = 100
    const leaveEndTimeout = 200
    // 初始元素已经存在,所以不需要改变显隐状态
    this.setState({ classes: leaveClasses })
    const leaveActiveTimer = setTimeout(_ => {
      this.setState({ classes: leaveActiveClasses })
      clearTimeout(leaveActiveTimer)
    }, leaveTimeout)
    const leaveEndTimer = setTimeout(_ => {
      this.setState({ classes: leaveEndActiveClasses })
      clearTimeout(leaveEndTimer)
    }, leaveTimeout + leaveActiveTimeout)
    // 最后将显隐状态改为false，同时将类名还原为初始状态
    const initTimer = setTimeout(_ => {
      this.setState({ visible: false, classes: '' })
      clearTimeout(initTimer)
    }, leaveTimeout + leaveActiveTimeout + leaveEndTimeout)
  }

  closeModal() {
    console.log('大家好，我叫取消，听说你们想点我？傲娇脸👸')
    const { onClose } = this.props
    onClose && onClose()
    // 点击取消后调用离开动画
    this.leaveAnimate()
  }

  confirm() {
    console.log('大家好，我叫确认，楼上的取消是我儿子，脑子有点那个~')
    const { confirm } = this.props
    confirm && confirm()
    this.leaveAnimate()
  }

  maskClick() {
    console.log('大家好，我是蒙层，我被点击了')
    this.setState({ visible: false })
  }

  render() {
    const { visible, classes } = this.state;
    const { title, children } = this.props;
    return <NewPortal>
      <div className="modal-wrapper">
        {
          visible &&
          <div className={`modal ${classes}`}>
            <div className="modal-title">{title}</div>
            <div className="modal-content">{children}</div>
            <div className="modal-operator">
              <button
                onClick={this.closeModal}
                className="modal-operator-close"
              >取消</button>
              <button
                onClick={this.confirm}
                className="modal-operator-confirm"
              >确认</button>
            </div>
          </div>
        }
        {/* 这里暂时注释蒙层，防止干扰 */}
        {/* <div
          className="mask"
          onClick={this.maskClick}
        ></div> */}
      </div>
    </NewPortal>
  }
}
export default Modal;
```

效果如下：

![](https://user-gold-cdn.xitu.io/2018/9/22/16600d2727e7633c?w=600&h=339&f=gif&s=274108)

### 2. 动画组件封装

实现了动画效果，但是代码全部在`modal.jsx`中，一点也不优雅，而且也不能复用，因此我们需要考虑将之抽象成一个`Transition`组件。
  
思路：我们从需要的功能点出发，来考虑如何进行封装。首先传入的显隐状态值控制元素的显隐；给与一个类名，其能匹配到对应的六种状态类名；可以配置每种状态的过渡时间；可以控制是否使用动画；
  
在`src`目录新建`transition`目录，创建文件`transition.jsx`,内容如下：

```javascript
import React from 'react';
// 这里引入classnames处理类名的拼接
import classnames from 'classnames';

class Transition extends React.Component {
  constructor(props) {
    super(props)
    this.getClasses = this.getClasses.bind(this)
    this.enterAnimate = this.enterAnimate.bind(this)
    this.leaveAnimate = this.leaveAnimate.bind(this)
    this.appearAnimate = this.appearAnimate.bind(this)
    this.cloneChildren = this.cloneChildren.bind(this)
    this.state = {
      visible: false,
      classes: null,
    }
  }

  // 过渡时间不传入默认为0
  static defaultProps = {
    animate: true,
    visible: false,
    transitionName: '',
    appearTimeout: 0,
    appearActiveTimeout: 0,
    appearEndTimeout: 0,
    enterTimeout: 0,
    enterActiveTimeout: 0,
    enterEndTimeout: 0,
    leaveTimeout: 0,
    leaveEndTimeout: 0,
    leaveActiveTimeout: 0,
  }

  // 这里我们添加了首次渲染动画。只出现一次
  componentWillMount() {
    const { transitionName, animate, visible } = this.props;
    if (!animate) {
      this.setState({ visible })
      return
    }
    this.appearAnimate(this.props, transitionName)
  }

  componentWillReceiveProps(props) {
    const { transitionName, animate, visible } = props
    if (!animate) {
      this.setState({ visible })
      return
    }
    if (!props.visible) {
      this.leaveAnimate(props, transitionName)
    } else {
      this.enterAnimate(props, transitionName)
    }
  }

  // 首次渲染的入场动画
  appearAnimate(props, transitionName) {
    const { visible, appearTimeout, appearActiveTimeout, appearEndTimeout } = props
    const { initClasses, activeClasses, endClasses } = this.getClasses('appear', transitionName)
    this.setState({ visible, classes: initClasses })
    setTimeout(_ => {
      this.setState({ classes: activeClasses })
    }, appearTimeout)
    setTimeout(_ => {
      this.setState({ classes: endClasses })
    }, appearActiveTimeout + appearTimeout)
    setTimeout(_ => {
      this.setState({ classes: '' })
    }, appearEndTimeout + appearActiveTimeout + appearTimeout)
  }

  // 入场动画
  enterAnimate(props, transitionName) {
    const { visible, enterTimeout, enterActiveTimeout, enterEndTimeout } = props
    const { initClasses, activeClasses, endClasses } = this.getClasses('enter', transitionName)
    this.setState({ visible, classes: initClasses })
    const enterTimer = setTimeout(_ => {
      this.setState({ classes: activeClasses })
      clearTimeout(enterTimer)
    }, enterTimeout)
    const enterActiveTimer = setTimeout(_ => {
      this.setState({ classes: endClasses })
      clearTimeout(enterActiveTimer)
    }, enterActiveTimeout + enterTimeout)
    const enterEndTimer = setTimeout(_ => {
      this.setState({ classes: '' })
      clearTimeout(enterEndTimer)
    }, enterEndTimeout + enterActiveTimeout + enterTimeout)
  }

  // 出场动画
  leaveAnimate(props, transitionName) {
    const { visible, leaveTimeout, leaveActiveTimeout, leaveEndTimeout } = props
    const { initClasses, activeClasses, endClasses } = this.getClasses('leave', transitionName)
    this.setState({ classes: initClasses })
    const leaveTimer = setTimeout(_ => {
      this.setState({ classes: activeClasses })
      clearTimeout(leaveTimer)
    }, leaveTimeout)
    const leaveActiveTimer = setTimeout(_ => {
      this.setState({ classes: endClasses })
      clearTimeout(leaveActiveTimer)
    }, leaveActiveTimeout + leaveTimeout)
    const leaveEndTimer = setTimeout(_ => {
      this.setState({ visible, classes: '' })
      clearTimeout(leaveEndTimer)
    }, leaveEndTimeout + leaveActiveTimeout + leaveTimeout)
  }

  // 类名统一配置
  getClasses(type, transitionName) {
    const initClasses = classnames({
      [`${transitionName}-appear`]: type === 'appear',
      [`${transitionName}-enter`]: type === 'enter',
      [`${transitionName}-leave`]: type === 'leave',
    })
    const activeClasses = classnames({
      [`${transitionName}-appear-active`]: type === 'appear',
      [`${transitionName}-enter-active`]: type === 'enter',
      [`${transitionName}-leave-active`]: type === 'leave',
    })
    const endClasses = classnames({
      [`${transitionName}-appear-end`]: type === 'appear',
      [`${transitionName}-enter-end`]: type === 'enter',
      [`${transitionName}-leave-end`]: type === 'leave',
    })
    return { initClasses, activeClasses, endClasses }
  }


  cloneChildren() {
    const { classes } = this.state
    const children = this.props.children
    const className = children.props.className

    // 通过React.cloneElement给子元素添加额外的props，
    return React.cloneElement(
      children,
      { className: `${className} ${classes}` }
    )
  }


  render() {
    const { visible } = this.state
    return visible && this.cloneChildren()
  }
}

export default Transition
```

`modal.jsx`内容修改如下：

```javascript
import React, { Component } from 'react';
import NewPortal from '../newPortal/newPortal';
import Transition from '../transition/transition';
import './modal.css';
class Modal extends Component {
  constructor(props) {
    super(props)
    this.confirm = this.confirm.bind(this)
    this.maskClick = this.maskClick.bind(this)
    this.closeModal = this.closeModal.bind(this)
    this.state = {
      visible: false,
    }
  }

  componentDidMount() {
    this.setState({ visible: this.props.visible })
  }

  componentWillReceiveProps(props) {
    this.setState({ visible: props.visible })
  }

  closeModal() {
    console.log('大家好，我叫取消，听说你们想点我？傲娇脸👸')
    const { onClose } = this.props
    onClose && onClose()
    this.setState({ visible: false })
  }

  confirm() {
    console.log('大家好，我叫确认，楼上的取消是我儿子，脑子有点那个~')
    const { confirm } = this.props
    confirm && confirm()
    this.setState({ visible: false })
  }

  maskClick() {
    console.log('大家好，我是蒙层，我被点击了')
    this.setState({ visible: false })
  }

  render() {
    const { visible } = this.state;
    const { title, children } = this.props;
    return <NewPortal>
      {/* 引入transition组件，去掉了外层的modal-wrapper */}
      <Transition
        visible={visible}
        transitionName="modal"
        enterActiveTimeout={200}
        enterEndTimeout={100}
        leaveActiveTimeout={100}
        leaveEndTimeout={200}
      >
        <div className="modal">
          <div className="modal-title">{title}</div>
          <div className="modal-content">{children}</div>
          <div className="modal-operator">
            <button
              onClick={this.closeModal}
              className="modal-operator-close"
            >取消</button>
            <button
              onClick={this.confirm}
              className="modal-operator-confirm"
            >确认</button>
          </div>
        </div>
        {/* 这里的mask也可以用transition组件包裹，添加淡入淡出的过渡效果，这里不再添加，有兴趣的读者可以自己实践下 */}
        {/* <div
          className="mask"
          onClick={this.maskClick}
        ></div> */}
      </Transition>
    </NewPortal>
  }
}
export default Modal;
```

文章到这里就写完了，为了阅读的完整性，每个步骤都是贴的完整的代码，导致全文篇幅过长，感谢您的阅读。
  
[本文代码地址](https://github.com/binnear/react-modal-transition)，欢迎star~