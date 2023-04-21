---
title: Three.js
date: 2021-12-01 10:12:33
categories: 
- web
---

# Threejs

## 开始

### 创建一个场景
场景、相机和渲染器
```
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );


```
- 相机：
three.js里有几种不同的相机，在这里，我们使用的是PerspectiveCamera（透视摄像机）。

第一个参数是视野角度（FOV）。视野角度就是无论在什么时候，你所能在显示器上看到的场景的范围，它的单位是角度(与弧度区分开)。

第二个参数是长宽比（aspect ratio）。 也就是你用一个物体的宽除以它的高的值。比如说，当你在一个宽屏电视上播放老电影时，可以看到图像仿佛是被压扁的。

接下来的两个参数是近截面（near）和远截面（far）。 当物体某些部分比摄像机的远截面远或者比近截面近的时候，该这些部分将不会被渲染到场景中。或许现在你不用担心这个值的影响，但未来为了获得更好的渲染性能，你将可以在你的应用程序里去设置它。

- 渲染器：
除了创建一个渲染器的实例之外，我们还需要在我们的应用程序里设置一个渲染器的尺寸。比如说，我们可以使用所需要的渲染区域的宽高，来让渲染器渲染出的场景填充满我们的应用程序。因此，我们可以将渲染器宽高设置为浏览器窗口宽高。对于性能比较敏感的应用程序来说，你可以使用setSize传入一个较小的值，例如window.innerWidth/2和window.innerHeight/2，这将使得应用程序在渲染时，以一半的长宽尺寸渲染场景。

如果你希望保持你的应用程序的尺寸，但是以较低的分辨率来渲染，你可以在调用setSize时，将updateStyle（第三个参数）设为false。例如，假设你的<canvas> 标签现在已经具有了100%的宽和高，调用setSize(window.innerWidth/2, window.innerHeight/2, false)将使得你的应用程序以一半的分辨率来进行渲染。

### 创建一个立方体
```
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;
```
创建一个立方体，我们需要一个BoxGeometry（立方体）对象. 这个对象包含了一个立方体中所有的顶点（vertices）和面（faces）。接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。第三步，我们需要一个Mesh（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

### 渲染场景
现在，如果将之前写好的代码复制到HTML文件中，你不会在页面中看到任何东西。这是因为我们还没有对它进行真正的渲染。为此，我们需要使用一个被叫做“渲染循环”（render loop）或者“动画循环”（animate loop）的东西。
```
function animate() {
	requestAnimationFrame( animate );
	renderer.render( scene, camera );
}
animate();
```
在这里我们创建了一个使渲染器能够在每次屏幕刷新时对场景进行绘制的循环（在大多数屏幕上，刷新率一般是60次/秒）。如果你是一个浏览器游戏开发的新手，你或许会说“为什么我们不直接用setInterval来实现刷新的功能呢？”当然啦，我们的确可以用setInterval，但是，requestAnimationFrame有很多的优点。最重要的一点或许就是当用户切换到其它的标签页时，它会暂停，因此不会浪费用户宝贵的处理器资源，也不会损耗电池的使用寿命。

## 进阶

### 相机
- 摄像机阵列（ArrayCamera）
ArrayCamera 用于更加高效地使用一组已经预定义的摄像机来渲染一个场景。这将能够更好地提升VR场景的渲染性能。
一个 ArrayCamera 的实例中总是包含着一组子摄像机，应当为每一个子摄像机定义viewport（视口）这个属性，这一属性决定了由该子摄像机所渲染的视口区域的大小。
```
 const cameras = [];
      for (let i = 1; i < 4; i++) {
        const subcamera = new THREE.PerspectiveCamera(40, 1, 0.1, 10);
        subcamera.viewport = new THREE.Vector4(
          Math.floor(i * 200),
          Math.floor(1 * 200), //视口高
          Math.ceil(200), //宽
          Math.ceil(200) //视口高
        );
        subcamera.position.x = i / 3 - 0.5;
        subcamera.position.y = 0.5 + i;
        subcamera.position.z = 1.5;
        subcamera.position.multiplyScalar(2);
        subcamera.lookAt(0, 0, 0);
        subcamera.updateMatrixWorld();
        cameras.push(subcamera);
      }
      let camera = new THREE.ArrayCamera(cameras);
```

- 立方相机（CubeCamera）
创建6个渲染到WebGLCubeRenderTarget的摄像机
构造器
CubeCamera( near : Number, far : Number, renderTarget : WebGLCubeRenderTarget )
near -- 近剪切面的距离
far -- 远剪切面的距离
renderTarget -- The destination cube render target.

构造一个包含6个PerspectiveCameras（透视摄像机）的立方摄像机， 并将其拍摄的场景渲染到一个WebGLCubeRenderTarget上。

- 正交相机（OrthographicCamera）
这一摄像机使用orthographic projection（正交投影）来进行投影。

在这种投影模式下，无论物体距离相机距离远或者近，在最终渲染的图片中物体的大小都保持不变。

这对于渲染2D场景或者UI元素是非常有用的。
```
const camera = new THREE.OrthographicCamera( width / - 2, width / 2, height / 2, height / - 2, 1, 1000 );
scene.add( camera );
```
构造器
OrthographicCamera( left : Number, right : Number, top : Number, bottom : Number, near : Number, far : Number )
left — 摄像机视锥体左侧面。
right — 摄像机视锥体右侧面。
top — 摄像机视锥体上侧面。
bottom — 摄像机视锥体下侧面。
near — 摄像机视锥体近端面。
far — 摄像机视锥体远端面。

- 透视相机（PerspectiveCamera）
这一摄像机使用perspective projection（透视投影）来进行投影。

这一投影模式被用来模拟人眼所看到的景象，它是3D场景的渲染中使用得最普遍的投影模式。
```
const camera = new THREE.PerspectiveCamera( 45, width / height, 1, 1000 );
scene.add( camera );
```
构造器
PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )
fov — 摄像机视锥体垂直视野角度
aspect — 摄像机视锥体长宽比
near — 摄像机视锥体近端面
far — 摄像机视锥体远端面

- 立体相机（StereoCamera）
双透视摄像机（立体相机）常被用于创建3D Anaglyph（3D立体影像） 或者Parallax Barrier（视差屏障）。

## 场景
### Scene
#### 构造器
Scene()
创建一个新的场景对象。

#### 属性
- .autoUpdate : Boolean
默认值为true，若设置了这个值，则渲染器会检查每一帧是否需要更新场景及其中物体的矩阵。 当设为false时，你必须亲自手动维护场景中的矩阵。

- .background : Object
若不为空，在渲染场景的时候将设置背景，且背景总是首先被渲染的。 可以设置一个用于的“clear”的Color（颜色）、一个覆盖canvas的Texture（纹理）， 或是 a cubemap as a CubeTexture or an equirectangular as a Texture。默认值为null。
`scene.background = new THREE.Color( 0xbfe3dd );`

- .environment : Texture
若该值不为null，则该纹理贴图将会被设为场景中所有物理材质的环境贴图。 然而，该属性不能够覆盖已存在的、已分配给 MeshStandardMaterial.envMap 的贴图。默认为null。
`scene.environment = pmremGenerator.fromScene( new RoomEnvironment(), 0.04 ).texture;`

- .overrideMaterial : Material
如果不为空，它将强制场景中的每个物体使用这里的材质来渲染。默认值为null。

- .fog : Fog
一个fog实例定义了影响场景中的每个物体的雾的类型。默认值为null。
`scene.fog=new THREE.FogExp2( 0xefd1b5, 0.0025 );`

### 雾（Fog）
这个类中的参数定义了线性雾。也就是说，雾的密度是随着距离线性增大的。
#### 构造器
Fog( color : Integer, near : Float, far : Float )
颜色参数传入Color构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是CSS风格的字符串。

#### 属性
- .name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

- .color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

- .near : Float
开始应用雾的最小距离。距离小于活动摄像机“near”个单位的物体将不会被雾所影响。
默认值是1。

- .far : Float
结束计算、应用雾的最大距离，距离大于活动摄像机“far”个单位的物体将不会被雾所影响。
默认值是1000。


### 雾-指数（FogExp2）
该类所包含的参数定义了指数雾，它可以在相机附近提供清晰的视野，且距离相机越远，雾的浓度随着指数增长越快。

#### 构造器
FogExp2( color : Integer, density : Float )
颜色参数传入Color构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是CSS风格的字符串。

#### 属性
- .name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

- .color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

- .density : Float
定义雾的密度将会增长多块。
默认值是0.00025.


## 全屏

```
window.addEventListener('dblclick',()=>{

  let isFull=document.fullscreenElement;
  if(!isFull){
    renderer.domElement.requestFullscreen(); 
  }else{

    document.exitFullscreen(); 
  }else{
  }
})
```
## 自适应屏幕

```
window.addEventListener('resize',()=>{
  //更新摄像头
  camera.aspect=window.innerWidth/window.innerHeight
  //更新摄像机投影矩阵
  camera.updateProjectionMatrix();
  //更新渲染器
  renderer.setSize(window.innerWidth,window.innerHeight)
  //设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio)
  }
```