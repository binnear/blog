之前在公司项目中实现了一个可配置的转盘抽奖，主要是用canvas进行绘制，写这篇文章是为了通过一个大家可能会感兴趣的点，来熟悉canvas的一些api，我自己也算是进行一些复习，当然文章不会像项目中实现的那样复杂，只是一个简易版本的，尽量做到通俗易懂，给感兴趣的你指引一条路（其实是懒~）。😁

## 一、实现一个转盘抽奖

在实现之前我们要有一个整体的思路，回想一下，我们曾经在网上或者现实中见过的抽奖转盘都有那些元素：

* 一个大的转盘
* 转盘上有一个个区间，表示不同的奖品
* 在转盘的中间有一个按钮，按钮上有一个指针，来指向抽中的奖品

我们再想想抽奖的过程：当我们点击中间的按钮，转盘开始旋转，当获取到奖品后，旋转的速度从快到慢，缓缓停下，最后指向奖品所在的那个区域，我们的实现步骤就可以这样

- 绘制所有的静态元素
- 给转盘添加旋转动画
- 指针所指区域为我们指定的奖品

### 1.绘制所有静态元素

#### 1.1.开发环境搭建

全局安装`create-react-app`，快速生成`react`的开发环境，这个与我们要开发的转盘没有耦合关系，只是为了方便调试，用`vue`也是可以的。

```javascript
// 全局安装
npm install create-react-app -g

// 生成开发环境，lottery为目录名
create-react-app lottery
```
安装完成后修改成如下图目录结构
  
![](https://user-gold-cdn.xitu.io/2018/10/14/16672136c078fe2c?w=525&h=324&f=png&s=20740)

`turntable.jsx`内容如下：

```javascript
export default class Turntable {
}
```

`App.js`内容如下：

```javascript
import React, { Component } from 'react'
import Turntable from './turntable/turntable'
class App extends Component {
  constructor(props) {
    super(props)
  }
  render() {
    return <div>抽奖转盘</div>
  }
}
export default App
```

完成以上工作后，在当前目录打开命令行工具，输入`npm start`启动项目

#### 1.2.绘制大转盘

修改`App.js`中的内容如下:

```javascript
import React, { Component } from 'react'
import Turntable from './turntable/turntable'
class App extends Component {
  constructor(props) {
    super(props)
    // react中获取dom元素
    this.canvas = React.createRef()
  }
  componentDidMount() {
    // canvas元素保存在this.canvas的current属性中
    const canvas = this.canvas.current
    // 获取canvas的上下文,context含有各种api用来操作canvas
    const context = canvas.getContext('2d')
    // 设置canvas的宽高
    canvas.width = 300
    canvas.height = 300
    // 创建turntable对象,并将canvas元素和context传入
    const turntable = new Turntable({canvas: canvas, context: context})
    turntable.render()
  }
  render() {
    return <canvas
      ref={this.canvas}
      style={{
        width: '300px',
        height: '300px',
        position: 'absolute',
        top: 0,
        left: 0,
        right: 0,
        bottom: 0,
        margin: 'auto'
      }}>
    </canvas>
  }
}
export default App
```

修改`turntable.jsx`中的内容:

```javascript
export default class Turntable {
  constructor(options) {
    // 获取并保存传入的canvas,context
    this.canvas = options.canvas
    this.context = options.context
  }
  drawPanel() {
    const context = this.context
    // 保存当前画布的状态,使用restore调用后,保证了当前
    // 绘制不受之前绘制的影响
    context.save()
    // 新建一个路径,画笔的位置回到默认的坐标(0,0)的位置
    // 保证了当前的绘制不会影响到之前的绘制
		context.beginPath()
    // 设置填充转盘用的颜色,fill是填充而不是绘制.
    context.fillStyle = '#FD6961'
    // 绘制一个圆,有六个参数,分别表示:圆心的x坐标,圆心的
    // y坐标,圆的半径,开始绘制的角度,结束的角度,绘制方向
    // (false表示顺时针)
    context.arc(150, 150, 150, 0, Math.PI * 2, false)
    // 将我们设置的颜色填充到圆中,这里不用closePath是因
    // 为closePath对fill无效.
    context.fill()
    // 将画布的状态恢复到我们上一次save()时的状态
    context.restore()
  } 
  render() {
		this.drawPanel()
	}
}
```

保存后,浏览器中结果显示如下图:
  
![](https://user-gold-cdn.xitu.io/2018/10/14/16672f7ef2327125?w=887&h=451&f=png&s=14715)

#### 1.2.绘制奖品块

在`turntable.jsx`文件中的`Turntable`类中添加和修改内容如下(为了避免重复内容导致篇幅过长,所有不变的部分都不会展示):

```javascript
// add
drawPrizeBlock() {
  const context = this.context
  // 第一个奖品色块开始绘制时开始的弧度及结束的弧度,因为我们这里
  // 暂时固定设置为6个奖品,所以以6为基数
  let startRadian = 0, RadianGap = Math.PI * 2 / 6, endRadian = startRadian + RadianGap
  for (let i = 0; i < 6; i++) {
    context.save()
    context.beginPath()
    // 为了区分不同的色块,我们使用随机生成的颜色作为色块的填充色
    context.fillStyle = '#'+Math.floor(Math.random()*16777215).toString(16)
    // 这里需要使用moveTo方法将初始位置定位在圆点处,这样绘制的圆
    // 弧都会以圆点作为闭合点,下面有使用moveTo和不使用moveTo的对比图
    context.moveTo(150, 150)
    // 画圆弧时,每次都会自动调用moveTo,将画笔移动到圆弧的起点,半
    // 径我们设置的比转盘稍小一点
    context.arc(150, 150, 140, startRadian, endRadian, false)
    // 每个奖品色块绘制完后,下个奖品的弧度会递增
    startRadian += RadianGap
    endRadian += RadianGap
    context.fill()
    context.restore()
  }
}
// modify
render() {
  this.drawPanel()
  this.drawPrizeBlock()
}
```

保存后,我们的转盘变为如下图样式:
  
![](https://user-gold-cdn.xitu.io/2018/10/14/166730141b370137?w=887&h=451&f=png&s=24686)

使用绘制奖品色块使用`context.moveTo(150, 150)`和不使用的区别:

使用了`context.moveTo(150, 150)`:
  
![](https://user-gold-cdn.xitu.io/2018/10/14/1667307ebb51d650?w=887&h=451&f=png&s=37248)

没有使用`context.moveTo(150, 150)`:
  
![](https://user-gold-cdn.xitu.io/2018/10/14/166730ae7602e398?w=887&h=451&f=png&s=36050)

奖品块绘制好了,我们需要添加每个奖品的名称,假定我们有三个奖品,另外三个就设置为未中奖

`turntable.jsx`文件中内容改变如下:

```javascript
constructor(options) {
  this.canvas = options.canvas
  this.context = options.context
  // add 初始化时添加了奖品的配置
  this.awards = [
    { level: '特等奖', name: '我的亲笔签名', color: '#576c0a' },
    { level: '未中奖', name: '未中奖', color: '#ad4411' },
    { level: '一等奖', name: '玛莎拉蒂超级经典限量跑车', color: '#43ed04' },
    { level: '未中奖', name: '未中奖', color: '#d5ed1d' },
    { level: '二等奖', name: '辣条一包', color: '#32acc6' },
    { level: '未中奖', name: '未中奖', color: '#e06510' },
  ]
}
// add
// 想一想,如我们一等奖那样,文字特别长的,超出我们的奖品块,而canvas
// 却不是那么智能给你提供自动换行的机制,于是我们只有手动处理换行
/**
 * 
 * @param {*} context         这个就不用解释了~
 * @param {*} text            这个是我们需要处理的长文本
 * @param {*} maxLineWidth    这个是我们自己定义的一行文本最大的宽度
 */
// 整个思路就是将满足我们定义的宽度的文本作为value单独添加到数组中
// 最后返回的数组的每一项就是我们处理后的每一行了.
getLineTextList(context, text, maxLineWidth) {
  let wordList = text.split(''), tempLine = '', lineList = []
  for (let i = 0; i < wordList.length; i++) {
    // measureText方法是测量文本的宽度的,这个宽度相当于我们设置的
    // fontSize的大小,所以基于这个,我们将maxLineWidth设置为当前字体大小的倍数
    if (context.measureText(tempLine).width >= maxLineWidth) {
      lineList.push(tempLine)
      maxLineWidth -= context.measureText(text[0]).width
      tempLine = ''
    }
    tempLine += wordList[i]
  }
  lineList.push(tempLine)
  return lineList
}
// modify 
drawPrizeBlock() {
  const context = this.context
  const awards = this.awards
  let startRadian = 0, RadianGap = Math.PI * 2 / 6, endRadian = startRadian + RadianGap
  for (let i = 0; i < awards.length; i++) {
    context.save()
    context.beginPath()
    context.fillStyle = awards[i].color
    context.moveTo(150, 150)
    context.arc(150, 150, 140, startRadian, endRadian, false)
    context.fill()
    context.restore()
    // 开始绘制我们的文字
    context.save();
    // 设置文字颜色
    context.fillStyle = '#FFFFFF';
    // 设置文字样式
    context.font = "14px Arial";
    // 改变canvas原点的位置,简单来说,translate到哪个坐标点,那么那个坐标点就将变为坐标(0, 0)
    context.translate(
      150 + Math.cos(startRadian + RadianGap / 2) * 140,
      150 + Math.sin(startRadian + RadianGap / 2) * 140
    );
    // 旋转角度,这个旋转是相对于原点进行旋转的.
    context.rotate(startRadian + RadianGap / 2 + Math.PI / 2);
    // 这里就是根据我们获取的各行的文字进行绘制,maxLineWidth我们取70,相当与
    // 一行我们最多展示5个文字
    this.getLineTextList(context, awards[i].name, 70).forEach((line, index) => {
      // 绘制文字的方法,三个参数分别带别:要绘制的文字,开始绘制的x坐标,开始绘制的y坐标
      context.fillText(line, -context.measureText(line).width / 2, ++index * 25);
    })
    context.restore();

    startRadian += RadianGap
    endRadian += RadianGap
  }
}
```

保存后,我们转盘变成如下模样:
  
![](https://user-gold-cdn.xitu.io/2018/10/15/1667861365645186?w=887&h=451&f=png&s=48853)

#### 1.3.绘制按钮与箭头

`Turntable`类添加和修改内容如下:

```javascript
// add
// 绘制按钮，以及按钮上start的文字，这里没有新的点，不再赘述
drawButton() {
  const context = this.context
  context.save()
  context.beginPath()
  context.fillStyle = '#FF0000'
  context.arc(150, 150, 30, 0, Math.PI * 2, false)
  context.fill()
  context.restore()
  
  context.save()
  context.beginPath()
  context.fillStyle = '#FFF'
  context.font = '20px Arial'
  context.translate(150, 150)
  context.fillText('Start', -context.measureText('Start').width / 2, 8)
  context.restore()
}
// add
// 绘制箭头，用来指向我们抽中的奖品
drawArrow() {
  const context = this.context
  context.save()
  context.beginPath()
  context.fillStyle = '#FF0000'
  context.moveTo(140, 125)
  context.lineTo(150, 100)
  context.lineTo(160, 125)
  context.closePath()
  context.fill()
  context.restore()
}
render() {
  this.drawPanel()
  this.drawPrizeBlock()
  this.drawButton()
  this.drawArrow()
}
```

保存后，转盘如下图所示：
  
![](https://user-gold-cdn.xitu.io/2018/10/16/1667d96bd509b2ed?w=887&h=451&f=png&s=27442)

#### 1.4.点击按钮，让转盘转动起来

效果应该是：当我们点击按钮时，按钮和指针保持不动，而转盘以及转盘上的奖品块会同时旋转起来。

而如何让转盘旋转起来呢？还记得我们是如何绘制转盘和奖品块的吗，我们是以绘制弧的方式，从角度为0的位置开始绘制，如果我们将绘制的开始角度增加一点，那么转盘的位置就相当与转动了一点，那么我们当我们不停的改变开始角度时，转盘看起来就像是旋转了。

`Turntable`类中内容修改如下：

```javascript
// modify
constructor(options) {
  this.canvas = options.canvas
  this.context = options.context
  // 添加了这个属性，来记录我们的初始角度
  this.startRadian = 0
  this.awards = [
    { level: '特等奖', name: '我的亲笔签名', color: '#576c0a' },
    { level: '未中奖', name: '未中奖', color: '#ad4411' },
    { level: '一等奖', name: '玛莎拉蒂超级经典限量跑车', color: '#43ed04' },
    { level: '未中奖', name: '未中奖', color: '#d5ed1d' },
    { level: '二等奖', name: '辣条一包', color: '#32acc6' },
    { level: '未中奖', name: '未中奖', color: '#e06510' },
  ]
}
// modify
drawPanel() {
  const context = this.context
  const startRadian = this.startRadian
  context.save()
  context.beginPath()
  context.fillStyle = '#FD6961'
  // 根据我们设定的初始角度来绘制转盘
  context.arc(150, 150, 150, startRadian, Math.PI * 2 + startRadian, false)
  context.fill()
  context.restore()
}
// modify
drawPrizeBlock() {
  const context = this.context
  const awards = this.awards
  // 根据初始角度来绘制奖品块
  let startRadian = this.startRadian, RadianGap = Math.PI * 2 / 6, endRadian = startRadian + RadianGap
  for (let i = 0; i < awards.length; i++) {
    context.save()
    context.beginPath()
    context.fillStyle = awards[i].color
    context.moveTo(150, 150)
    context.arc(150, 150, 140, startRadian, endRadian, false)
    context.fill()
    context.restore()

    context.save()
    context.fillStyle = '#FFF'
    context.font = "14px Arial"
    context.translate(
      150 + Math.cos(startRadian + RadianGap / 2) * 140,
      150 + Math.sin(startRadian + RadianGap / 2) * 140
    )
    context.rotate(startRadian + RadianGap / 2 + Math.PI / 2)
    this.getLineTextList(context, awards[i].name, 70).forEach((line, index) => {
      context.fillText(line, -context.measureText(line).width / 2, ++index * 25)
    })
    context.restore()

    startRadian += RadianGap
    endRadian += RadianGap
  }
}
// add
// 这个方法是为了将canvas再window中的坐标点转化为canvas中的坐标点
windowToCanvas(canvas, e) {
  // getBoundingClientRect这个方法返回html元素的大小及其相对于视口的位置
  const canvasPostion = canvas.getBoundingClientRect(), x = e.clientX, y = e.clientY
  return {
    x: x - canvasPostion.left,
    y: y - canvasPostion.top
  }
};
// add
// 这个方法将作为真正的初始化方法
startRotate() {
  const canvas = this.canvas
  const context = this.context
  // getAttribute这个方法可以获取到元素的属性值，我们获取了canvas的样式将之保存在canvasStyle变量中
  const canvasStyle = canvas.getAttribute('style');
  // 这里绘制我们初始化时候的canvas元素
  this.render()
  // 添加一个点击事件，点击按钮后，我们开始旋转转盘
  canvas.addEventListener('mousedown', e => {
    let postion = this.windowToCanvas(canvas, e)
    context.beginPath()
    // 这里是在按钮区域在次绘制了一个没有颜色的圆，然后判断我们点击的落点是否在这个圆内，相当于判断是否点击我们的按钮
    context.arc(150, 150, 30, 0, Math.PI * 2, false)
    if (context.isPointInPath(postion.x, postion.y)) {
      // 点击按钮后，我们会调用这个方法来改变我们的初始角度startRadian
      this.rotatePanel()
    }
  })
  // 添加鼠标移动事件，仅仅是为了设置鼠标指针的样式
  canvas.addEventListener('mousemove', e => {
    let postion = this.windowToCanvas(canvas, e)
    context.beginPath()
    context.arc(150, 150, 30, 0, Math.PI * 2, false)
    if (context.isPointInPath(postion.x, postion.y)) {
      canvas.setAttribute('style', `cursor: pointer;${canvasStyle}`)
    } else {
      canvas.setAttribute('style', canvasStyle)
    }
  })
}
// add
// 处理旋转的关键方法
rotatePanel() {
  // 每次调用都将初始角度增加1度
  this.startRadian += Math.PI / 180
  // 初始角度改变后，我们需要重新绘制
  this.render()
  // 循环调用rotatePanel函数，使得转盘的绘制连续，造成旋转的视觉效果
  window.requestAnimationFrame(this.rotatePanel.bind(this));
}
// modify
render() {
  this.drawPanel()
  this.drawPrizeBlock()
  this.drawButton()
  this.drawArrow()
}
```

`App.js`内容修改如下：

```javascript
// modify
componentDidMount() {
  const canvas = this.canvas.current
  const context = canvas.getContext('2d')
  canvas.width = 300
  canvas.height = 300
  const turntable = new Turntable({ canvas: canvas, context: context })
  // 将render替换为调用startRotate
  turntable.startRotate()
}
```

保存后，将如下图操作效果：
  
![](https://user-gold-cdn.xitu.io/2018/10/19/1668cc83e9bf93ea?w=880&h=446&f=gif&s=760863)

可以看到，当我们点击点击按钮后，转盘开始缓缓的旋转了。
  
#### 1.5.让转盘缓缓停留在我们指定的奖品处

`Turntable`类中内容修改如下：

```javascript
// modify
constructor(options) {
  this.canvas = options.canvas
  this.context = options.context
  this.startRadian = 0
  // 我们添加了一个点击限制，这里为了控制抽奖中不让再抽奖
  this.canBeClick = true
  this.awards = [
    { level: '特等奖', name: '我的亲笔签名', color: '#576c0a' },
    { level: '未中奖', name: '未中奖', color: '#ad4411' },
    { level: '一等奖', name: '玛莎拉蒂超级经典限量跑车', color: '#43ed04' },
    { level: '未中奖', name: '未中奖', color: '#d5ed1d' },
    { level: '二等奖', name: '辣条一包', color: '#32acc6' },
    { level: '未中奖', name: '未中奖', color: '#e06510' },
  ]
}
// modify
startRotate() {
  const canvas = this.canvas
  const context = this.context
  const canvasStyle = canvas.getAttribute('style');
  this.render()
  canvas.addEventListener('mousedown', e => {
    // 只要抽奖没有结束，就不让再次抽奖
    if (!this.canBeClick) return
    this.canBeClick = false
    let loc = this.windowToCanvas(canvas, e)
    context.beginPath()
    context.arc(150, 150, 30, 0, Math.PI * 2, false)
    if (context.isPointInPath(loc.x, loc.y)) {
      // 每次点击抽奖，我们都将初始化角度重置
      this.startRadian = 0
      // distance是我们计算出的将指定奖品旋转到指针处需要旋转的角度距离，distanceToStop下面会又说明
      const distance = this.distanceToStop()
      this.rotatePanel(distance)
    }
  })
  canvas.addEventListener('mousemove', e => {
    let loc = this.windowToCanvas(canvas, e)
    context.beginPath()
    context.arc(150, 150, 30, 0, Math.PI * 2, false)
    if (context.isPointInPath(loc.x, loc.y)) {
      canvas.setAttribute('style', `cursor: pointer;${canvasStyle}`)
    } else {
      canvas.setAttribute('style', canvasStyle)
    }
  })
}
// modify
rotatePanel(distance) {
  // 我们这里用一个很简单的缓动函数来计算每次绘制需要改变的角度，这样可以达到一个转盘从块到慢的渐变的过程
  let changeRadian = (distance - this.startRadian) / 10
  this.startRadian += changeRadian
  // 当最后我们的目标距离与startRadian之间的差距低于0.05时，我们就默认奖品抽完了，可以继续抽下一个了。
  if (distance - this.startRadian <= 0.05) {
    this.canBeClick = true;
    return
  }
  this.render()
  window.requestAnimationFrame(this.rotatePanel.bind(this, distance))
}
// add
distanceToStop() {
  // middleDegrees为奖品块的中间角度（我们最终停留都是以中间角度进行计算的）距离初始的startRadian的距离，distance就是当前奖品跑到指针位置要转动的距离。
  let middleDegrees = 0, distance = 0
  // 映射出每个奖品的middleDegrees
  const awardsToDegreesList = this.awards.map((data, index) => {
    let awardRadian = (Math.PI * 2) / this.awards.length
    return awardRadian * index + (awardRadian * (index + 1) - awardRadian * index) / 2
  });
  // 随机生成一个索引值，来表示我们此次抽奖应该中的奖品
  const currentPrizeIndex = Math.floor(Math.random() * this.awards.length)
  console.log('当前奖品应该中的奖品是：'+this.awards[currentPrizeIndex].name)
  middleDegrees = awardsToDegreesList[currentPrizeIndex];
  // 因为指针是垂直向上的，相当坐标系的Math.PI/2,所以我们这里要进行判断来移动角度
  distance = Math.PI * 3 / 2 - middleDegrees
  distance = distance > 0 ? distance : Math.PI * 2 + distance
  // 这里额外加上后面的值，是为了让转盘多转动几圈，看上去更像是在抽奖
  return distance + Math.PI * 10;
}
```

保存后，我们便可以开始抽奖了，如下图：

![](https://user-gold-cdn.xitu.io/2018/10/19/1668d07c7626dbdb?w=879&h=447&f=gif&s=2770125)

比较幸运，第一次就中了两玛莎拉蒂😀

文章写到这里就差不多结束，为了方便理解（偷懒），文章中很多参数都是写死的，大家在工作中千万别这么作，要被骂的~~
  
这个只是一个非常非常简易的转盘，还有很多很多地方可以进行优化和扩展，比如为每个奖品添加图片，转盘颜色及每个奖品块颜色可配置，指针转动进行抽奖，美化转盘（示例的转盘颜色是随机生成的六个颜色，感觉还不丑哈~~），移动端适配等等，考验你们的时候到了。
  
最后的最后，安利两部国漫，《星辰变》，细节感人，只是太短了，到现在才3集；另一个是《狐妖小红娘》，苏苏的配音简直萌到心都化了，bilibili看哦~

[源码地址](https://github.com/binnear/lottery)