<p align="center">
<img width=150 src="https://github.com/zchholmes/tsp_image/blob/master/logo.png">
</p>
<h1 align="center">TensorSpace.js</h1>
<p align="center"><b>Present Tensor in Space</b></p>

<p align="center">
<a href=""><strong>English</strong></a> | <strong>中文</strong>
</p>

TensorSpace是一套用于构建神经网络3D可视化应用的框架。
开发者可以使用类Keras风格的TensorSpace API，轻松创建可视化网络、加载神经网络模型并在浏览器中基于已加载的模型进行3D可交互呈现。
TensorSpace可以使您更直观地观察神经网络模型，并了解该模型是如何通过中间层 tensor 的运算来得出最终结果的。
TensorSpace 支持3D可视化所有经过适当预处理之后的 TensorFlow、Keras、TensorFlow.js 模型。

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_lenet.gif">
</p>
<p align="center">
<b>图1</b> - TensorSpace 应用展示
</p>

## 目录

* [TensorSpace 使用场景](#motivation)
* [开始使用](#getting-start)
* [使用样例](#example)
* [文档](#documentation)
* [开发人员](#contributors)
* [许可证](#license)

## <div id="motivation">TensorSpace 使用场景</div>

TensorSpace 基于TensorFlow.js和Three.js开发，用于对神经网络进行3D可视化呈现。通过使用 TensorSpace，不仅仅能展示神经网络的结构，还可以呈现网络的内部特征提取、中间层的数据交互以及最终的结果预测等一系列过程。

通过使用 TensorSpace，可以帮助您更直观地观察并理解基于TensorFlow、Keras或者TensorFlow.js开发的神经网络模型。
TensorSpace 降低了前端开发者进行深度学习相关应用开发的门槛。
我们期待看到更多基于 TensorSpace 开发的3D可视化应用。

* **交互** -- 使用类 Keras 的API构建浏览器端可交互模型。

* **直观** -- 展示并观察模型中间层预测数据，直观展示模型推测过程。

* **集成** -- 支持基于使用 TensorFlow、Keras及TensorFlow.js 训练的模型。

## <div id="getting-start">开始使用</div>

### 安装

* **第一步: 下载 TensorSpace.js**

我们提供了三种下载 TensorSpace.js 的方法，它们分别是 npm、yarn 以及 来自官方网站。

途径 1: NPM
```bash
npm install tensorspace
```

途径 2: Yarn
```bash
yarn add tensorspace
```

途径 3: [官方网站下载]()

* **第二步: 安装依赖库**

请在使用 TensorSapce.js 之前，引入[Tensorflow.js](https://github.com/tensorflow/tfjs)、 [Three.js](https://github.com/mrdoob/three.js)、 [Tween.js](https://github.com/tweenjs/tween.js) 和 [TrackballControl.js](https://github.com/mrdoob/three.js/blob/master/examples/js/controls/TrackballControls.js) 至所需要的 html 文件中，并置于 TensorSpace.js 的引用之前。

```html
<script src="three.min.js"></script>
<script src="tween.min.js"></script>
<script src="TrackballControls.js"></script>
<script src="tf.min.js"></script>
```

* **第三步: 安装 Tensorspace.js**

将 TensorSpace.js 引入 html 文件中：
```html
<script src="tensorspace.min.js"></script>
```

### 模型预处理

为了获得神经网络中间层的运算结果，我们需要对已有的模型进行[模型预处理](https://github.com/syt123450/tensorspace/wiki/%5BTutorial%5D-Introduction:-Model-Preprocessing)。

基于不同的机器学习库，我们提供了 [TensorFlow 模型预处理教程]()、[Keras 模型预处理教程]() 以及 [TensorFlow.js 模型预处理教程]()。

### 使用

在成功安装完成 TensorSpace 并完成神经网络模型预处理之后，我们可以来创建一个3D TensorSpace 模型。

我们会使用我们所提供的[经过预处理的LeNet神经网络模型]()作为使用样例来进行说明。

首先，我们需要新建一个 TensorSpace 模型实例：
```JavaScript
let container = document.getElementById( "container" );
let model = new TSP.model.Sequential( container );
```

然后，基于 LeNet 网络的结构：输入层 + 2 X (Conv2D层 & Maxpooling层) + 3 X (Dense层)，我们可以搭建其模型结构：
```JavaScript
model.add( new TSP.layers.Input2d({ shape: [28, 28, 1] }) );
model.add( new TSP.layers.Padding2d({ padding: [2, 2] }) );
model.add( new TSP.layers.Conv2d({ kernelSize: 5, filters: 6, strides: 1 }) );
model.add( new TSP.layers.Pooling2d({ poolSize: [2, 2], strides: [2, 2] }) );
model.add( new TSP.layers.Conv2d({ kernelSize: 5, filters: 16, strides: 1 }) );
model.add( new TSP.layers.Pooling2d({ poolSize: [2, 2], strides: [2, 2] }) );
model.add( new TSP.layers.Dense({ units: 120 }) );
model.add( new TSP.layers.Dense({ units: 84 }) );
model.add( new TSP.layers.Output1d({
    units: 10,
    outputs: ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
}) );
```

最后，我们需要载入[经过预处理的 TensorSpace 适配模型]()并使用`init()`方法来创建模型对象：
```JavaScript
model.load({
    type: "tfjs",
    url: './lenetModel/mnist.json',
    onComplete: function() {
        console.log( "\"Hello World!\" from TensorSpace Loader." );
    }
});
model.init();
```

我们可以在浏览器中看到以下模型：
<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/GettingStart/HelloWorld_empty_lenet.jpg">
</p>
<p align="center">
<b>图2</b> - 所创建的 LeNet 模型
</p>

我们可以使用我们已经提取好的手写“5”作为模型的输入：
```
model.init(function() {
    model.predict( image_5 );
});

```

我们在这里将预测方法放入`init()`的回调函数中以确保预测在初始化完成之后进行。

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/GettingStart/HelloWorld_5.jpg">
</p>
<p align="center">
<b>图3</b> - LeNet 模型判别输入 "5"
</p>


## <div id="example">样例展示</div>

* **LeNet**

 [➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_lenet.gif">
</p>
<p align="center">
<b>图4</b> - 使用 TensorSpace 构建 LeNet
</p>

* **AlexNet** 

[➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_alexnet.gif">
</p>
<p align="center">
<b>图5</b> - 使用 TensorSpace 构建 AlexNet
</p>

* **Yolov2-tiny** 

[➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_yolov2.gif">
</p>
<p align="center">
<b>图6</b> - 使用 TensorSpace 构建 YOLO-v2-tiny
</p>

* **ResNet-50**

[➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_lenet.gif">
</p>
<p align="center">
<b>图7</b> - 使用 TensorSpace 构建 ResNet-50
</p>

* **Vgg16**

[➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/tensorspace_vgg.gif">
</p>
<p align="center">
<b>图8</b> - 使用 TensorSpace 构建 VGG-16
</p>

* ACGAN

[➡ 在线演示]()

<p align="center">
<img width="100%" src="https://github.com/zchholmes/tsp_image/blob/master/acgan.gif">
</p>
<p align="center">
<b>图9</b> - 使用 TensorSpace 构建 ACGAN 生成网络
</p>

## <div id="documentation">文档</div>

* 迅速开始使用，参阅[开始使用]()。
* 下载并安装，查看[下载]()。
* 了解更多[基本概念]()。
* 如何使用神经网络模型，查看[模型预处理]()。
* 了解核心组成构件：[模型]()、[网络层]() 以及 [网络层融合]()。
* 希望获取更多TensorSpace的咨询，请访问 TensorSpace 官方网站 [TensorSpace.org]()。

## <div id="contributors">开发人员</div>

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars2.githubusercontent.com/u/7977100?v=4" width="100px;"/><br /><sub><b>syt123450</b></sub>](https://github.com/syt123450)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=syt123450 "Code") [🎨](#design-syt123450 "Design") [📖](https://github.com/tensorspace-team/tensorspace/commits?author=syt123450 "Documentation") [💡](#example-syt123450 "Examples") | [<img src="https://avatars3.githubusercontent.com/u/4524339?v=4" width="100px;"/><br /><sub><b>Chenhua Zhu</b></sub>](https://github.com/zchholmes)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=zchholmes "Code") [🎨](#design-zchholmes "Design") [✅](#tutorial-zchholmes "Tutorials") [💡](#example-zchholmes "Examples") | [<img src="https://avatars0.githubusercontent.com/u/21956621?v=4" width="100px;"/><br /><sub><b>YaoXing Liu</b></sub>](https://charlesliuyx.github.io/)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=CharlesLiuyx "Code") [🎨](#design-CharlesLiuyx "Design") [✅](#tutorial-CharlesLiuyx "Tutorials") [💡](#example-CharlesLiuyx "Examples") | [<img src="https://avatars2.githubusercontent.com/u/19629037?v=4" width="100px;"/><br /><sub><b>Qi(Nora)</b></sub>](https://github.com/lq3297401)<br />[💻](https://github.com/tensorspace-team/tensorspace/commits?author=lq3297401 "Code") [🎨](#design-lq3297401 "Design") |
| :---: | :---: | :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

## <div id="license">许可证</div>

[Apache License 2.0](https://github.com/syt123450/tensorspace/blob/master/LICENSE)