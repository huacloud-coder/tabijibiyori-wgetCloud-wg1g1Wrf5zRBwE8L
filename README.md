
# 概念介绍


在开始前，我们先看一下效果，我在场景中创建了一个立方体，当我们点击鼠标左键并拖动时，可以旋转相机视角，滚动鼠标滚轮可以缩放相机视角。


![file](https://img2024.cnblogs.com/other/367486/202412/367486-20241201181317836-734777633.gif)


相信看了动图效果，大家对相机控件有了一个直观的认识。它是 Three.js 中用于控制相机的工具，可以帮助用户在 3D 场景中自由旋转、缩放和\*移相机，提供更加友好的交互体验。


在 Three.js 中，相机（Camera）是决定如何渲染场景的关键元素。它是用户从哪个角度看到 3D 场景的“窗口”。相机控制器（Camera Controls）则是允许开发者在场景中动态调整相机视角、位置和缩放的工具。对于大多数 3D 应用程序，尤其是需要交互的应用程序，控制相机的行为是非常重要的一部分。


## 常见的相机类型：


### 透视相机（PerspectiveCamera）


这是最常见的相机类型，它模仿了人眼的视觉效果。通过设定视野角度、\*剪裁\*面和远剪裁\*面来决定观察的范围。


### 正交相机（OrthographicCamera）


不同于透视相机，正交相机的视图没有透视失真，适合需要\*行视角的场景，比如 2D 游戏、CAD 或者是某些类型的数据可视化。


## 常见的相机控件


Three.js 提供了一些非常强大的相机控制器，用于增强用户与 3D 场景的交互。这些控件一般会利用鼠标、触控板等输入设备，允许用户旋转、缩放、\*移相机。


2\.1 OrbitControls
OrbitControls 是 Three.js 中最常用的相机控件之一。它允许用户绕着一个目标点进行旋转、缩放和拖动，适用于大多数需要相机交互的场景。


用法


1. 先在场景中创建一个立方体，作为 OrbitControls 的目标点。



```
import * as THREE from "three";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
const renderer = new THREE.WebGLRenderer();

renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

camera.position.set(0, 0, 5);

const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

function render() {
  renderer.render(scene, camera);
}

render();

```

![file](https://img2024.cnblogs.com/other/367486/202412/367486-20241201181318293-575498070.png)


2. 接着，我们引入一个坐标系辅助，用于等会操作时更好地观察变化



```
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

```

![file](https://img2024.cnblogs.com/other/367486/202412/367486-20241201181318480-1273732762.png)


3. 最后，引入相机控制器。相机控制器不能直接通过 Three.OrbitControls()的方式引入，如果你是通过 npm 包管理器安装的 three.js，那么你可以在 node\_modules/three/examples/jsm/controls 中找到 OrbitControls.js 文件，然后通过 import 的方式引入。


![file](https://img2024.cnblogs.com/other/367486/202412/367486-20241201181319012-272599918.png)
所以你可以通过以下方式引入 OrbitControls：



```
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

```

最后，完整的引入代码如下：



```
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
// 创建一个控制器
const controls = new OrbitControls(camera, renderer.domElement);
// 更新控件
controls.update();
// 添加事件监听器，在控件变化时重新渲染
controls.addEventListener("change", render);

```

可以看到，引入一个控制器非常简单，只需要传入相机和渲染器的 dom 元素即可。然后通过 update() 方法更新控件，添加事件监听器，当控件变化时重新渲染。


完整的代码如下：



```
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
const renderer = new THREE.WebGLRenderer();

renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

const controls = new OrbitControls(camera, renderer.domElement);

camera.position.set(0, 0, 5);
controls.update();

const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  renderer.render(scene, camera);
}

controls.addEventListener("change", render); // add this line to re-render on control changes

render(); // initial render

```

Three.js学习：[https://www.threejs3d.com/](https://github.com)


 本博客参考[slowerssr加速器](https://slowerss.com)。转载请注明出处！
