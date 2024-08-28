---
title: Three.js
date: 2022-10-01 10:12:33
categories:
  - web
tags:
  - web
---

## 开始

### 创建一个场景

场景、相机和渲染器

```js
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
```

- 相机：
  three.js 里有几种不同的相机，在这里，我们使用的是 PerspectiveCamera（透视摄像机）。

第一个参数是视野角度（FOV）。视野角度就是无论在什么时候，你所能在显示器上看到的场景的范围，它的单位是角度(与弧度区分开)。

第二个参数是长宽比（aspect ratio）。 也就是你用一个物体的宽除以它的高的值。比如说，当你在一个宽屏电视上播放老电影时，可以看到图像仿佛是被压扁的。

接下来的两个参数是近截面（near）和远截面（far）。 当物体某些部分比摄像机的远截面远或者比近截面近的时候，该这些部分将不会被渲染到场景中。或许现在你不用担心这个值的影响，但未来为了获得更好的渲染性能，你将可以在你的应用程序里去设置它。

- 渲染器：
  除了创建一个渲染器的实例之外，我们还需要在我们的应用程序里设置一个渲染器的尺寸。比如说，我们可以使用所需要的渲染区域的宽高，来让渲染器渲染出的场景填充满我们的应用程序。因此，我们可以将渲染器宽高设置为浏览器窗口宽高。对于性能比较敏感的应用程序来说，你可以使用 setSize 传入一个较小的值，例如 window.innerWidth/2 和 window.innerHeight/2，这将使得应用程序在渲染时，以一半的长宽尺寸渲染场景。

如果你希望保持你的应用程序的尺寸，但是以较低的分辨率来渲染，你可以在调用 setSize 时，将 updateStyle（第三个参数）设为 false。例如，假设你的`<canvas>` 标签现在已经具有了 100%的宽和高，调用 setSize(window.innerWidth/2, window.innerHeight/2, false)将使得你的应用程序以一半的分辨率来进行渲染。

### 创建一个立方体

```js
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

camera.position.z = 5;
```

创建一个立方体，我们需要一个 BoxGeometry（立方体）对象. 这个对象包含了一个立方体中所有的顶点（vertices）和面（faces）。接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。第三步，我们需要一个 Mesh（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

### 渲染场景

现在，如果将之前写好的代码复制到 HTML 文件中，你不会在页面中看到任何东西。这是因为我们还没有对它进行真正的渲染。为此，我们需要使用一个被叫做“渲染循环”（render loop）或者“动画循环”（animate loop）的东西。

```js
function animate() {
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
}
animate();
```

在这里我们创建了一个使渲染器能够在每次屏幕刷新时对场景进行绘制的循环（在大多数屏幕上，刷新率一般是 60 次/秒）。如果你是一个浏览器游戏开发的新手，你或许会说“为什么我们不直接用 setInterval 来实现刷新的功能呢？”当然啦，我们的确可以用 setInterval，但是，requestAnimationFrame 有很多的优点。最重要的一点或许就是当用户切换到其它的标签页时，它会暂停，因此不会浪费用户宝贵的处理器资源，也不会损耗电池的使用寿命。

## 进阶

### 相机

#### Camera

##### 构造函数

`Camera()`
创建一个新的 Camera（摄像机）。注意：这个类并不是被直接调用的；你所想要的或许是一个 PerspectiveCamera（透视摄像机）或者 OrthographicCamera（正交摄像机）。

##### 属性

共有属性请参见其基类 Object3D

.isCamera : Boolean
Read-only flag to check if a given object is of type Camera.

.layers : Layers
摄像机是一个 layers 的成员. 这是一个从 Object3D 继承而来的属性。

当摄像机的视点被渲染的时候，物体必须和当前被看到的摄像机共享至少一个层。

.matrixWorldInverse : Matrix4
这是 matrixWorld 矩阵的逆矩阵。 MatrixWorld 包含了相机的世界变换矩阵。

.projectionMatrix : Matrix4
这是投影变换矩阵。

.projectionMatrixInverse : Matrix4
这是投影变换矩阵的逆矩阵。

##### 方法

共有方法请参见其基类 Object3D。

.clone ( ) : Camera
返回一个具有和当前相机的属性一样的新的相机。

.copy ( source : Camera, recursive : Boolean ) : this
将源摄像机的属性复制到新摄像机中。

.getWorldDirection ( target : Vector3 ) : Vector3
target — 调用该函数的结果将复制给该 Vector3 对象。

返回一个能够表示当前摄像机所正视的世界空间方向的 Vector3 对象。 （注意：摄像机俯视时，其 Z 轴坐标为负。）

#### 透视相机（PerspectiveCamera）

这一摄像机使用 perspective projection（透视投影）来进行投影。

这一投影模式被用来模拟人眼所看到的景象，它是 3D 场景的渲染中使用得最普遍的投影模式。
透视投影相机的四个参数 fov, aspect, near, far 构成一个四棱台 3D 空间，被称为视锥体，只有视锥体之内的物体，才会渲染出来，视锥体范围之外的物体不会显示在 Canvas 画布上。
透视投影相机的投影规律是远小近大，通过相机观察阵列立方体大小变化，可以看到距离相机越远，立方体的渲染视觉效果越小。增加相机视角 fov，视锥体范围更大，意味着可以看到渲染范围更大，远小近大的视觉效果更明显。

```js
const camera = new THREE.PerspectiveCamera(45, width / height, 1, 1000);
scene.add(camera);
```

##### 构造器

`PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )`
fov — 摄像机视锥体垂直视野角度
aspect — 摄像机视锥体长宽比,一般设置为 Canvas 画布宽高比 width / height
near — 摄像机视锥体近端面
far — 摄像机视锥体远端面

##### 属性

> 共有属性请参见其基类 Camera 。

请注意，在大多数属性发生改变之后，你将需要调用.updateProjectionMatrix 来使得这些改变生效。

.aspect : Float
摄像机视锥体的长宽比，通常是使用画布的宽/画布的高。默认值是 1（正方形画布）。

.far : Float
摄像机的远端面，默认值是 2000。

该值必须大于 near plane（摄像机视锥体近端面）的值。

.filmGauge : Float
胶片尺寸，其默认值为 35（毫米）。 这个参数不会影响摄像机的投影矩阵，除非.filmOffset 被设置为了一个非零的值。

.filmOffset : Float
水平偏离中心偏移量，和.filmGauge 单位相同。默认值为 0。

.focus : Float
用于立体视觉和景深效果的物体的距离。 这个参数不会影响摄像机的投影矩阵，除非使用了 StereoCamera。 默认值是 10。

.fov : Float
摄像机视锥体垂直视野角度，从视图的底部到顶部，以角度来表示。默认值是 50。

.isPerspectiveCamera : Boolean
Read-only flag to check if a given object is of type PerspectiveCamera.

.near : Float
摄像机的近端面，默认值是 0.1。

其有效值范围是 0 到当前摄像机 far plane（远端面）的值之间。 请注意，和 OrthographicCamera 不同，0 对于 PerspectiveCamera 的近端面来说不是一个有效值。

.view : Object
Frustum window specification or null. 这个值使用.setViewOffset 方法来进行设置，使用.clearViewOffset 方法来进行清除。

.zoom : number
获取或者设置摄像机的缩放倍数，其默认值为 1。

##### 方法

> 共有方法请参见其基类 Camera。

.clearViewOffset () : undefined
清除任何由.setViewOffset 设置的偏移量。

.getEffectiveFOV () : Float
结合.zoom（缩放倍数），以角度返回当前垂直视野角度。

.getFilmHeight () : Float
返回当前胶片上图像的高，如果.aspect 小于或等于 1（肖像格式、纵向构图），则结果等于.filmGauge。

.getFilmWidth () : Float
返回当前胶片上图像的宽，如果.aspect 大于或等于 1（景观格式、横向构图），则结果等于.filmGauge。

.getFocalLength () : Float
返回当前.fov（视野角度）相对于.filmGauge（胶片尺寸）的焦距。

.setFocalLength ( focalLength : Float ) : undefined
通过相对于当前.filmGauge 的焦距，设置 FOV。

默认情况下，焦距是为 35mm（全画幅）摄像机而指定的。

.setViewOffset ( fullWidth : Float, fullHeight : Float, x : Float, y : Float, width : Float, height : Float ) : undefined
fullWidth — 多视图的全宽设置
fullHeight — 多视图的全高设置
x — 副摄像机的水平偏移
y — 副摄像机的垂直偏移
width — 副摄像机的宽度
height — 副摄像机的高度

在较大的 viewing frustum（视锥体）中设置偏移量，对于多窗口或者多显示器的设置是很有用的。

例如，如果你有一个 3x2 的显示器阵列，每个显示器分辨率都是 1920x1080，且这些显示器排列成像这样的网格：
+---+---+---+
| A | B | C |
+---+---+---+
| D | E | F |
+---+---+---+

那对于每个显示器，你可以这样来设置、调用：
const w = 1920;
const h = 1080;
const fullWidth = w _3;
const fullHeight = h_ 2;

// A
camera.setViewOffset( fullWidth, fullHeight, w _0, h_ 0, w, h );
// B
camera.setViewOffset( fullWidth, fullHeight, w _1, h_ 0, w, h );
// C
camera.setViewOffset( fullWidth, fullHeight, w _2, h_ 0, w, h );
// D
camera.setViewOffset( fullWidth, fullHeight, w _0, h_ 1, w, h );
// E
camera.setViewOffset( fullWidth, fullHeight, w _1, h_ 1, w, h );
// F
camera.setViewOffset( fullWidth, fullHeight, w _2, h_ 1, w, h );请注意，显示器的不必具有相同的大小，或者不必在网格中。
.updateProjectionMatrix () : undefined
更新摄像机投影矩阵。在任何参数被改变以后必须被调用。

.toJSON (meta : Object) : Object
meta -- 包含有元数据的对象，例如对象后代中的纹理或图像
将摄像机转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

##### 其他

- 相机位置.position
  `camera.position.set(200, 200, 200);`
- 相机观察目标.lookAt()

```js
//相机观察目标指向Threejs 3D空间中某个位置
camera.lookAt(0, 0, 0); //坐标原点
camera.lookAt(0, 10, 0); //y轴上位置10
camera.lookAt(mesh.position); //指向mesh对应的位置
```

#### 正交相机（OrthographicCamera）

这一摄像机使用 orthographic projection（正交投影）来进行投影。

在这种投影模式下，无论物体距离相机距离远或者近，在最终渲染的图片中物体的大小都保持不变。

这对于渲染 2D 场景或者 UI 元素是非常有用的。

```js
const camera = new THREE.OrthographicCamera(
  width / -2,
  width / 2,
  height / 2,
  height / -2,
  1,
  1000
);
scene.add(camera);
```

##### 构造器

OrthographicCamera( left : Number, right : Number, top : Number, bottom : Number, near : Number, far : Number )
left — 摄像机视锥体左侧面。
right — 摄像机视锥体右侧面。
top — 摄像机视锥体上侧面。
bottom — 摄像机视锥体下侧面。
near — 摄像机视锥体近端面。
far — 摄像机视锥体远端面。

##### 属性

共有属性请参见其基类 Camera。
请注意，在大多数属性发生改变之后，你将需要调用.updateProjectionMatrix 来使得这些改变生效。

.bottom : Float
摄像机视锥体下侧面。

.far : Float
摄像机视锥体远端面，其默认值为 2000。

该值必须大于 near plane（摄像机视锥体近端面）的值。

.isOrthographicCamera : Boolean
Read-only flag to check if a given object is of type OrthographicCamera.

.left : Float
摄像机视锥体左侧面。

.near : Float
摄像机视锥体近端面。其默认值为 0.1.

其值的有效范围介于 0 和 far（摄像机视锥体远端面）之间。
请注意，和 PerspectiveCamera 不同，0 对于 OrthographicCamera 的近端面来说是一个有效值。

.right : Float
摄像机视锥体右侧面。

.top : Float
摄像机视锥体上侧面。

.view : Object
这个值是由 setViewOffset 来设置的，其默认值为 null。

.zoom : number
获取或者设置摄像机的缩放倍数，其默认值为 1。

##### 方法

共有方法请参见其基类 Camera。

.setViewOffset ( fullWidth : Float, fullHeight : Float, x : Float, y : Float, width : Float, height : Float ) : undefined
fullWidth — 多视图的全宽设置
fullHeight — 多视图的全高设置
x — 副摄像机的水平偏移
y — 副摄像机的垂直偏移
width — 副摄像机的宽度
height — 副摄像机的高度

在较大的 viewing frustum（视锥体）中设置偏移量，对于多窗口或者多显示器的设置是很有用的。 对于如何使用它，请查看 PerspectiveCamera 中的示例。

.clearViewOffset () : undefined
清除任何由.setViewOffset 设置的偏移量。

.updateProjectionMatrix () : undefined
更新摄像机投影矩阵。在任何参数被改变以后必须被调用。

.toJSON (meta : Object) : Object
meta -- 包含有元数据的对象，例如对象后代中的纹理或图像
将摄像机转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 摄像机阵列（ArrayCamera）

ArrayCamera 用于更加高效地使用一组已经预定义的摄像机来渲染一个场景。这将能够更好地提升 VR 场景的渲染性能。
一个 ArrayCamera 的实例中总是包含着一组子摄像机，应当为每一个子摄像机定义 viewport（视口）这个属性，这一属性决定了由该子摄像机所渲染的视口区域的大小。

```js
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

#### 立方相机（CubeCamera）

创建 6 个渲染到 WebGLCubeRenderTarget 的摄像机
构造器
CubeCamera( near : Number, far : Number, renderTarget : WebGLCubeRenderTarget )
near -- 近剪切面的距离
far -- 远剪切面的距离
renderTarget -- The destination cube render target.

构造一个包含 6 个 PerspectiveCameras（透视摄像机）的立方摄像机， 并将其拍摄的场景渲染到一个 WebGLCubeRenderTarget 上。

#### 立体相机（StereoCamera）

双透视摄像机（立体相机）常被用于创建 3D Anaglyph（3D 立体影像） 或者 Parallax Barrier（视差屏障）。

### 几何体

#### 缓冲属性 BufferAttribute

这个类用于存储与 BufferGeometry 相关联的 attribute（例如顶点位置向量，面片索引，法向量，颜色值，UV 坐标以及任何自定义 attribute ）。 利用 BufferAttribute，可以更高效的向 GPU 传递数据。

> 在 BufferAttribute 中，数据被存储为任意长度的矢量（通过 itemSize 进行定义），下列函数如无特别说明， 函数参数中的 index 会自动乘以矢量长度进行计算。 当想要处理类似向量的数据时， 可以使用在 Vector2，Vector3， Vector4 以及 Color 这些类中的.fromBufferAttribute( attribute, index ) 方法来更为便捷地处理。

##### 构造函数

`BufferAttribute( array : TypedArray, itemSize : Integer, normalized : Boolean )`
array -- 必须是 TypedArray. 类型，用于实例化缓存。
该队列应该包含：`itemSize * numVertices`个元素，numVertices 是 BufferGeometry 中的顶点数目

itemSize -- 队列中与顶点相关的数据值的大小。举例，如果 attribute 存储的是三元组（例如顶点空间坐标、法向量或颜色值）则 itemSize 的值应该是 3。

normalized -- (可选) 指明缓存中的数据如何与 GLSL 代码中的数据对应。例如，如果 array 是 UInt16Array 类型，且 normalized 的值是 true，则队列中的值将会从 0 - +65535 映射为 GLSL 中的 0.0f - +1.0f。 如果 array 是 Int16Array (有符号)，则值将会从 -32768 - +32767 映射为 -1.0f - +1.0f。若 normalized 的值为 false，则数据映射不会归一化，而会直接映射为 float 值，例如，32767 将会映射为 32767.0f.

##### 属性

- `.array` : TypedArray
  在 array 中保存着缓存中的数据。

- `.count` : Integer
  保存 array 除以 itemSize 之后的大小。若缓存存储三元组（例如顶点位置、法向量、颜色值），则该值应等于队列中三元组的个数。

- `.isBufferAttribute` : Boolean
  用于判断对象是否为 BufferAttribute 类型的只读标记.

`.itemSize` : Integer
保存在 array 中矢量的长度。

- `.name` : String
  该 attribute 实例的别名，默认值为空字符串。

- `.needsUpdate` : Boolean
  该标志位指明当前 attribute 已经被修改过，且需要再次送入 GPU 处理。当开发者改变了该队列的值，则标志位需要设置为 true。

将标志位设为 true 同样会增加 version 的值。

- `.normalized` : Boolean
  指明缓存中数据在转化为 GLSL 着色器代码中数据时是否需要被归一化。详见构造函数中的说明。

- `.onUploadCallback` : Function
  attribute 数据传输到 GPU 后的回调函数。

- `.updateRange` : Object
  对象包含如下成员:
  offset: 默认值为 0。 指明更新的起始位置。
  count: 默认值为 -1，表示不指定更新范围。

该值只可以被用于更新某些矢量数据（例如，颜色相关数据）。

- `.usage` : Usage
  为输入的数据定义最优的预估使用方式。等同于在 WebGLRenderingContext.bufferData() 中的 usage 参数。默认为 StaticDrawUsage。在 usage constants 中查看可用值。

- `.version` : Integer
  版本号，当 needsUpdate 被设置为 true 时，该值会自增。

##### 方法

- `.applyMatrix3 ( m : Matrix3 ) : this`
  将矩阵 m 应用此 BufferAttribute 中的每一个 Vector3 元素中。

- `.applyMatrix4 ( m : Matrix4 ) : this`
  将矩阵 m 应用到此 BufferAttribute 的每一个 Vector3 元素中

- `.applyNormalMatrix ( m : Matrix3 ) : this`
  将正规矩阵 m 应用到此 BufferAttribute 的每一个 Vector3 元素中

- `.transformDirection ( m : Matrix4 ) : this`
  将矩阵 m 应用到此 BufferAttribute 的每一个 Vector3 元素中，并将所有元素解释为方向向量。

- `.clone () : BufferAttribute`
  返回该 BufferAttribute 的拷贝。

- `.copyArray ( array ) : this`
  将参数中所给定的普通队列或 TypedArray 拷贝到 array 中。

拷贝 TypedArray 相关注意事项详见 TypedArray.set。

- `.copyAt ( index1 : Integer, bufferAttribute : BufferAttribute, index2 : Integer ) : this`
  将一个矢量从 `bufferAttribute[index2]` 拷贝到 `array[index1]` 中。

- `.getX ( index : Integer ) : Number`
  获取给定索引的矢量的第一维元素 （即 X 值）。

- `.getY ( index : Integer ) : Number`
  获取给定索引的矢量的第二维元素 （即 Y 值）。

- `.getZ ( index : Integer ) : Number`
  获取给定索引的矢量的第三维元素 （即 Z 值）。

- `.getW ( index : Integer ) : Number`
  获取给定索引的矢量的第四维元素 （即 W 值）。

- `.onUpload ( callback : Function ) : this`
  见 onUploadCallback 属性。

在 WebGL / Buffergeometry 中，该方在缓存数据传递给 GPU 后，用于释放内存。

- `.set ( value : Array, offset : Integer ) : this`
  value -- 被拷贝的 Array 或 TypedArray 类型的数据。
  offset -- (可选) array 中开始拷贝的位置索引。

对 array，调用 TypedArray.set( value, offset ) 方法。

特别的, 对将 value 转为 TypedArray 的要求详见上述链接。

- `.setUsage ( value : Usage ) : this`
  Set usage to value. See usage constants for all possible input values.

- `.setX ( index : Integer, x : Float ) : this`
  设置给定索引的矢量的第一维数据（设置 X 值）。

- `.setY ( index : Integer, y : Float ) : this`
  设置给定索引的矢量的第二维数据（设置 Y 值）。

- `.setZ ( index : Integer, z : Float ) : this`
  设置给定索引的矢量的第三维数据（设置 Z 值）。

- `.setW ( index : Integer, w : Float ) : this`
  设置给定索引的矢量的第四维数据（设置 W 值）。

- `.setXY ( index : Integer, x : Float, y : Float ) : this`
  设置给定索引的矢量的第一、二维数据（设置 X 和 Y 值）。

- `.setXYZ ( index : Integer, x : Float, y : Float, z : Float ) : this`
  设置给定索引的矢量的第一、二、三维数据（设置 X、Y 和 Z 值）。

- `.setXYZW ( index : Integer, x : Float, y : Float, z : Float, w : Float ) : this`
  设置给定索引的矢量的第一、二、三、四维数据（设置 X、Y、Z 和 W 值）。

#### 缓冲几何体 BufferGeometry

是面片、线或点几何体的有效表述。包括顶点位置，面片索引、法相量、颜色值、UV 坐标和自定义缓存属性值。使用 BufferGeometry 可以有效减少向 GPU 传输上述数据所需的开销。
BufferGeometry 是一个没有任何形状的空几何体，你可以通过 BufferGeometry 自定义任何几何形状

```js
const geometry = new THREE.BufferGeometry();
// 创建一个简单的矩形. 在这里我们左上和右下顶点被复制了两次。
// 因为在两个三角面片里，这两个顶点都需要被用到。
//通过javascript类型化数组 Float32Array创建一组xyz坐标数据用来表示几何体的顶点坐标
const vertices = new Float32Array([
  -1.0, -1.0, 1.0, 1.0, -1.0, 1.0, 1.0, 1.0, 1.0,

  1.0, 1.0, 1.0, -1.0, 1.0, 1.0, -1.0, -1.0, 1.0,
]);
// 创建属性缓冲区对象
// itemSize = 3 因为每个顶点都是一个三元组。
//3个为一组，表示一个顶点的xyz坐标
geometry.setAttribute("position", new THREE.BufferAttribute(vertices, 3)); //设置几何体顶点
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const mesh = new THREE.Mesh(geometry, material);
```

##### 构造函数

`BufferGeometry()`创建一个新的 BufferGeometry. 同时将预置属性设置为默认值.

##### 属性

- `.attributes` : Object
  通过 hashmap 存储该几何体相关的属性，hashmap 的 id 是当前 attribute 的名称，值是相应的 buffer。 你可以通过 `.setAttribute` 和 `.getAttribute` 添加和访问与当前几何体有关的 attribute。

- `.boundingBox` : Box3
  当前 bufferGeometry 的外边界矩形。可以通过 .computeBoundingBox() 计算。默认值是 null。

- `.boundingSphere` : Sphere
  当前 bufferGeometry 的外边界球形。可以通过 .computeBoundingSphere() 计算。默认值是 null。

- `.drawRange` : Object
  用于判断几何体的哪个部分需要被渲染。该值不应该直接被设置，而需要通过 .setDrawRange 进行设置。默认值为`{ start: 0, count: Infinity }`
- `.groups` : Array
  将当前几何体分割成组进行渲染，每个部分都会在单独的 WebGL 的 draw call 中进行绘制。该方法可以让当前的 bufferGeometry 可以使用一个材质队列进行描述。分割后的每个部分都是一个如下的表单：`{ start: Integer, count: Integer, materialIndex: Integer }`start 表明当前 draw call 中的没有索引的几何体的几何体的第一个顶点；或者第一个三角面片的索引。 count 指明当前分割包含多少顶点（或 indices）。 materialIndex 指出当前用到的材质队列的索引。通过 `.addGroup` 来增加组，而不是直接更改当前队列。

- `.id` : Integer
  当前 bufferGeometry 的唯一编号。

- `.index` : BufferAttribute
  允许顶点在多个三角面片间可以重用。这样的顶点被称为"已索引的三角面片(indexed triangles)。 每个三角面片都和三个顶点的索引相关。该 attribute 因此所存储的是每个三角面片的三个顶点的索引。 如果该 attribute 没有设置过，则 renderer 假设每三个连续的位置代表一个三角面片。 默认值是 null。

`.isBufferGeometry` : Boolean
用于判断对象是否为 BufferGeometry 的只读标记.

`.morphAttributes` : Object
存储 BufferAttribute 的 Hashmap，存储了几何体 morph targets 的细节信息。
注意：当这个 geometry 渲染之后，morph attribute 数据无法更改。你需要调用.dispose()，并重新创建一个新的 BufferGeometry 实例。

`.morphTargetsRelative` : Boolean
用于控制 morph target 的行为，如果设置为 true，morph target 数据作为相对的偏移量，而非绝对的位置/法向。 默认为 false。

`.name` : String
当前 bufferGeometry 实例的可选别名。默认值是空字符串。

`.userData` : Object
存储 BufferGeometry 的自定义数据的对象。为保持对象在克隆时完整，该对象不应该包括任何函数的引用。

`.uuid` : String
当前对象实例的 UUID，该值会自动被分配，且不应被修改。

##### 方法

> EventDispatcher 在该类上可用的所有方法。

- `.setAttribute ( name : String, attribute : BufferAttribute ) : this`
  为当前几何体设置一个 attribute 属性。在类的内部，有一个存储 .attributes 的 hashmap， 通过该 hashmap，遍历 attributes 的速度会更快。而使用该方法，可以向 hashmap 内部增加 attribute。 所以，你需要使用该方法来添加 attributes。

- `.addGroup ( start : Integer, count : Integer, materialIndex : Integer ) : undefined`
  为当前几何体增加一个 group，详见 groups 属性。

- `.applyMatrix4 ( matrix : Matrix4 ) : this`
  用给定矩阵转换几何体的顶点坐标。

- `.center () : this`
  根据边界矩形将几何体居中。

- `.clone () : BufferGeometrya`
  克隆当前的 BufferGeometry。

- `.copy ( bufferGeometry : BufferGeometry ) : this`
  将参数指定的 BufferGeometry 的值拷贝到当前 BufferGeometry 中。

- `.clearGroups ( ) : undefined`
  清空所有的 groups。

- `.computeBoundingBox () : undefined`
  计算当前几何体的的边界矩形，该操作会更新已有 `[param:.boundingBox]`。
  边界矩形不会默认计算，需要调用该接口指定计算边界矩形，否则保持默认值 null。

- `.computeBoundingSphere () : undefined`
  计算当前几何体的的边界球形，该操作会更新已有 `[param:.boundingSphere]`。
  边界球形不会默认计算，需要调用该接口指定计算边界球形，否则保持默认值 null。

- `.computeTangents () : undefined`
  计算并向此 geometry 中添加 tangent attribute。
  只支持索引化的几何体对象，并且必须拥有 position(位置)，normal(法向)和 uv attributes。如果使用了切线空间法向贴图，最好使用 BufferGeometryUtils.computeMikkTSpaceTangents 中的 MikkTSpace 算法。

- `.computeVertexNormals () : undefined`
  通过面片法向量的平均值计算每个顶点的法向量。

`.dispose () : undefined`
从内存中销毁对象。
如果在运行时需要从内存中删除 BufferGeometry，则需要调用该函数。

`.getAttribute ( name : String ) : BufferAttribute`
返回指定名称的 attribute。

`.getIndex () : BufferAttribute`
返回缓存相关的 .index。

`.hasAttribute ( name : String ) : Boolean`
检查是否存在有指定名称的 attribute，如果有返回 true。

`.lookAt ( vector : Vector3 ) : this`
vector - 几何体所朝向的世界坐标。

旋转几何体朝向控件中的一点。该过程通常在一次处理中完成，不会循环处理。典型的用法是过通过调用 Object3D.lookAt 实时改变 mesh 朝向。

`.normalizeNormals () : undefined`
几何体中的每个法向量长度将会为 1。这样操作会更正光线在表面的效果。

`.deleteAttribute ( name : String ) : BufferAttribute`
删除具有指定名称的 attribute。

`.rotateX ( radians : Float ) : this`
在 X 轴上旋转几何体。该操作一般在一次处理中完成，不会循环处理。典型的用法是通过调用 Object3D.rotation 实时旋转几何体。

`.rotateY ( radians : Float ) : this`
在 Y 轴上旋转几何体。该操作一般在一次处理中完成，不会循环处理。典型的用法是通过调用 Object3D.rotation 实时旋转几何体。

`.rotateZ ( radians : Float ) : this`
在 Z 轴上旋转几何体。该操作一般在一次处理中完成，不会循环处理。典型的用法是通过调用 Object3D.rotation 实时旋转几何体。

`.scale ( x : Float, y : Float, z : Float ) : this`
缩放几何体。该操作一般在一次处理中完成，不会循环处理。典型的用法是通过调用 Object3D.scale 实时旋转几何体。

`.setIndex ( index : BufferAttribute ) : this`
设置缓存的 .index。

`.setDrawRange ( start : Integer, count : Integer ) : undefined`
设置缓存的 .drawRange。详见相关属性说明。

`.setFromPoints ( points : Array ) : this`
通过点队列设置该 BufferGeometry 的 attribute。

`.toJSON () : Object`
返回代表该 BufferGeometry 的 JSON 对象。

`.toNonIndexed () : BufferGeometry`
返回已索引的 BufferGeometry 的非索引版本。

`.translate ( x : Float, y : Float, z : Float ) : this`
移动几何体。该操作一般在一次处理中完成，不会循环处理。典型的用法是通过调用 Object3D.rotation 实时旋转几何体。

#### 立方体（BoxGeometry）

```js
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);
```

##### 构造器

`BoxGeometry(width : Float, height : Float, depth : Float, widthSegments : Integer, heightSegments : Integer, depthSegments : Integer)`
width — X 轴上面的宽度，默认值为 1。
height — Y 轴上面的高度，默认值为 1。
depth — Z 轴上面的深度，默认值为 1。
widthSegments — （可选）宽度的分段数，默认值是 1。
heightSegments — （可选）高度的分段数，默认值是 1。
depthSegments — （可选）深度的分段数，默认值是 1。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆形（CircleGeometry）

```js
const geometry = new THREE.CircleGeometry(5, 32);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const circle = new THREE.Mesh(geometry, material);
scene.add(circle);
```

##### 构造器

`CircleGeometry(radius : Float, segments : Integer, thetaStart : Float, thetaLength : Float)`
radius — 圆形的半径，默认值为 1
segments — 分段（三角面）的数量，最小值为 3，默认值为 32。
thetaStart — 第一个分段的起始角度，默认为 0。（three o'clock position）
thetaLength — 圆形扇区的中心角，通常被称为“θ”（西塔）。默认值是 2\*Pi，这使其成为一个完整的圆。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 平面（PlaneGeometry）

```js
const geometry = new THREE.PlaneGeometry(1, 1);
const material = new THREE.MeshBasicMaterial({
  color: 0xffff00,
  side: THREE.DoubleSide,
});
const plane = new THREE.Mesh(geometry, material);
scene.add(plane);
```

##### 构造器

`PlaneGeometry(width : Float, height : Float, widthSegments : Integer, heightSegments : Integer)`
width — 平面沿着 X 轴的宽度。默认值是 1。
height — 平面沿着 Y 轴的高度。默认值是 1。
widthSegments — （可选）平面的宽度分段数，默认值是 1。
heightSegments — （可选）平面的高度分段数，默认值是 1。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆柱（CylinderGeometry）

```js
const geometry = new THREE.CylinderGeometry(5, 5, 20, 32);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const cylinder = new THREE.Mesh(geometry, material);
scene.add(cylinder);
```

##### 构造器

`CylinderGeometry(radiusTop : Float, radiusBottom : Float, height : Float, radialSegments : Integer, heightSegments : Integer, openEnded : Boolean, thetaStart : Float, thetaLength : Float)`
radiusTop — 圆柱的顶部半径，默认值是 1。
radiusBottom — 圆柱的底部半径，默认值是 1。
height — 圆柱的高度，默认值是 1。
radialSegments — 圆柱侧面周围的分段数，默认为 32。
heightSegments — 圆柱侧面沿着其高度的分段数，默认值为 1。
openEnded — 一个 Boolean 值，指明该圆锥的底面是开放的还是封顶的。默认值为 false，即其底面默认是封顶的。
thetaStart — 第一个分段的起始角度，默认为 0。（three o'clock position）
thetaLength — 圆柱底面圆扇区的中心角，通常被称为“θ”（西塔）。默认值是 2\*Pi，这使其成为一个完整的圆柱。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 挤压（ExtrudeGeometry）

```js
const length = 12,
  width = 8;

const shape = new THREE.Shape();
shape.moveTo(0, 0);
shape.lineTo(0, width);
shape.lineTo(length, width);
shape.lineTo(length, 0);
shape.lineTo(0, 0);

const extrudeSettings = {
  steps: 2,
  depth: 16,
  bevelEnabled: true,
  bevelThickness: 1,
  bevelSize: 1,
  bevelOffset: 0,
  bevelSegments: 1,
};

const geometry = new THREE.ExtrudeGeometry(shape, extrudeSettings);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

##### 构造器

`ExtrudeGeometry(shapes : Array, options : Object)`
shapes — 形状或者一个包含形状的数组。
options — 一个包含有下列参数的对象：

curveSegments — int，曲线上点的数量，默认值是 12。
steps — int，用于沿着挤出样条的深度细分的点的数量，默认值为 1。
depth — float，挤出的形状的深度，默认值为 1。
bevelEnabled — bool，对挤出的形状应用是否斜角，默认值为 true。
bevelThickness — float，设置原始形状上斜角的厚度。默认值为 0.2。
bevelSize — float。斜角与原始形状轮廓之间的延伸距离，默认值为 bevelThickness-0.1。
bevelOffset — float. Distance from the shape outline that the bevel starts. Default is 0.
bevelSegments — int。斜角的分段层数，默认值为 3。
extrudePath — THREE.Curve 对象。一条沿着被挤出形状的三维样条线。Bevels not supported for path extrusion.
UVGenerator — Object。提供了 UV 生成器函数的对象。
该对象将一个二维形状挤出为一个三维几何体。

当使用这个几何体创建 Mesh 的时候，如果你希望分别对它的表面和它挤出的侧面使用单独的材质，你可以使用一个材质数组。 第一个材质将用于其表面；第二个材质则将用于其挤压出的侧面。##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 形状（ShapeGeometry）

```js
const x = 0,
  y = 0;

const heartShape = new THREE.Shape();

heartShape.moveTo(x + 5, y + 5);
heartShape.bezierCurveTo(x + 5, y + 5, x + 4, y, x, y);
heartShape.bezierCurveTo(x - 6, y, x - 6, y + 7, x - 6, y + 7);
heartShape.bezierCurveTo(x - 6, y + 11, x - 3, y + 15.4, x + 5, y + 19);
heartShape.bezierCurveTo(x + 12, y + 15.4, x + 16, y + 11, x + 16, y + 7);
heartShape.bezierCurveTo(x + 16, y + 7, x + 16, y, x + 10, y);
heartShape.bezierCurveTo(x + 7, y, x + 5, y + 5, x + 5, y + 5);

const geometry = new THREE.ShapeGeometry(heartShape);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

##### 构造器

`ShapeGeometry(shapes : Array, curveSegments : Integer)`
shapes — 一个单独的 shape，或者一个包含形状的 Array。Default is a single triangle shape.
curveSegments - Integer - 每一个形状的分段数，默认值为 12。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆环（RingGeometry）

```js
const geometry = new THREE.RingGeometry(1, 5, 32);
const material = new THREE.MeshBasicMaterial({
  color: 0xffff00,
  side: THREE.DoubleSide,
});
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

##### 构造器

`RingGeometry(innerRadius : Float, outerRadius : Float, thetaSegments : Integer, phiSegments : Integer, thetaStart : Float, thetaLength : Float)`
innerRadius — 内部半径，默认值为 0.5。
outerRadius — 外部半径，默认值为 1。
thetaSegments — 圆环的分段数。这个值越大，圆环就越圆。最小值为 3，默认值为 32。
phiSegments — 最小值为 1，默认值为 8。
thetaStart — 起始角度，默认值为 0。
thetaLength — 圆心角，默认值为 Math.PI \* 2。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆锥（ConeGeometry）

```js
const geometry = new THREE.ConeGeometry(5, 20, 32);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const cone = new THREE.Mesh(geometry, material);
scene.add(cone);
```

##### 构造器

`ConeGeometry(radius : Float, height : Float, radialSegments : Integer, heightSegments : Integer, openEnded : Boolean, thetaStart : Float, thetaLength : Float)`
radius — 圆锥底部的半径，默认值为 1。
height — 圆锥的高度，默认值为 1。
radialSegments — 圆锥侧面周围的分段数，默认为 32。
heightSegments — 圆锥侧面沿着其高度的分段数，默认值为 1。
openEnded — 一个 Boolean 值，指明该圆锥的底面是开放的还是封顶的。默认值为 false，即其底面默认是封顶的。
thetaStart — 第一个分段的起始角度，默认为 0。（three o'clock position）
thetaLength — 圆锥底面圆扇区的中心角，通常被称为“θ”（西塔）。默认值是 2\*Pi，这使其成为一个完整的圆锥。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 球（SphereGeometry）

```js
const geometry = new THREE.SphereGeometry(15, 32, 16);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const sphere = new THREE.Mesh(geometry, material);
scene.add(sphere);
```

##### 构造器

`SphereGeometry(radius : Float, widthSegments : Integer, heightSegments : Integer, phiStart : Float, phiLength : Float, thetaStart : Float, thetaLength : Float)`
radius — 球体半径，默认为 1。
widthSegments — 水平分段数（沿着经线分段），最小值为 3，默认值为 32。
heightSegments — 垂直分段数（沿着纬线分段），最小值为 2，默认值为 16。
phiStart — 指定水平（经线）起始角度，默认值为 0。。
phiLength — 指定水平（经线）扫描角度的大小，默认值为 Math.PI \* 2。
thetaStart — 指定垂直（纬线）起始角度，默认值为 0。
thetaLength — 指定垂直（纬线）扫描角度大小，默认值为 Math.PI。
该几何体是通过扫描并计算围绕着 Y 轴（水平扫描）和 X 轴（垂直扫描）的顶点来创建的。 因此，不完整的球体（类似球形切片）可以通过为 phiStart，phiLength，thetaStart 和 thetaLength 设置不同的值来创建， 以定义我们开始（或结束）计算这些顶点的起点（或终点）。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆环（TorusGeometry）

```js
const geometry = new THREE.TorusGeometry(10, 3, 16, 100);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const torus = new THREE.Mesh(geometry, material);
scene.add(torus);
```

##### 构造器

`TorusGeometry(radius : Float, tube : Float, radialSegments : Integer, tubularSegments : Integer, arc : Float)`
radius - 环面的半径，从环面的中心到管道横截面的中心。默认值是 1。
tube — 管道的半径，默认值为 0.4。
radialSegments — 管道横截面的分段数，默认值为 12。
tubularSegments — 管道的分段数，默认值为 48。
arc — 圆环的圆心角（单位是弧度），默认值为 Math.PI \* 2。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 管道（TubeGeometry）

```js
class CustomSinCurve extends THREE.Curve {
  constructor(scale = 1) {
    super();

    this.scale = scale;
  }

  getPoint(t, optionalTarget = new THREE.Vector3()) {
    const tx = t * 3 - 1.5;
    const ty = Math.sin(2 * Math.PI * t);
    const tz = 0;

    return optionalTarget.set(tx, ty, tz).multiplyScalar(this.scale);
  }
}

const path = new CustomSinCurve(10);
const geometry = new THREE.TubeGeometry(path, 20, 2, 8, false);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

##### 构造器

`TubeGeometry(path : Curve, tubularSegments : Integer, radius : Float, radialSegments : Integer, closed : Boolean)`
path — Curve - 一个由基类 Curve 继承而来的 3D 路径。 Default is a quadratic bezier curve.
tubularSegments — Integer - 组成这一管道的分段数，默认值为 64。
radius — Float - 管道的半径，默认值为 1。
radialSegments — Integer - 管道横截面的分段数目，默认值为 8。
closed — Boolean 管道的两端是否闭合，默认值为 false。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。
`.tangents` : Array
一个 Vector3 切线数组。

`.normals` : Array
一个 Vector3 法线数组。

`.binormals` : Array
一个 Vector3 次法线数组。

#### 多面缓冲几何体（PolyhedronGeometry）

```js
const verticesOfCube = [
  -1, -1, -1, 1, -1, -1, 1, 1, -1, -1, 1, -1, -1, -1, 1, 1, -1, 1, 1, 1, 1, -1,
  1, 1,
];

const indicesOfFaces = [
  2, 1, 0, 0, 3, 2, 0, 4, 7, 7, 3, 0, 0, 1, 5, 5, 4, 0, 1, 2, 6, 6, 5, 1, 2, 3,
  7, 7, 6, 2, 4, 5, 6, 6, 7, 4,
];

const geometry = new THREE.PolyhedronGeometry(
  verticesOfCube,
  indicesOfFaces,
  6,
  2
);
```

##### 构造器

`PolyhedronGeometry(vertices : Array, indices : Array, radius : Float, detail : Integer)`
vertices — 一个顶点 Array（数组）：[1,1,1, -1,-1,-1, ... ]。
indices — 一个构成面的索引 Array（数组）， [0,1,2, 2,3,0, ... ]。
radius — Float - 最终形状的半径。
detail — Integer - 将对这个几何体细分多少个级别。细节越多，形状就越平滑。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 边缘几何体（EdgesGeometry）

```js
const geometry = new THREE.BoxGeometry(100, 100, 100);
const edges = new THREE.EdgesGeometry(geometry);
const line = new THREE.LineSegments(
  edges,
  new THREE.LineBasicMaterial({ color: 0xffffff })
);
scene.add(line);
```

##### 构造器

`EdgesGeometry( geometry : BufferGeometry, thresholdAngle : Integer )`
geometry — 任何一个几何体对象。
thresholdAngle — 仅当相邻面的法线之间的角度（单位为角度）超过这个值时，才会渲染边缘。默认值为 1。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。
> `.parameters` : Object
> 一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 网格几何体（WireframeGeometry）

```js
const geometry = new THREE.SphereGeometry(100, 100, 100);

const wireframe = new THREE.WireframeGeometry(geometry);

const line = new THREE.LineSegments(wireframe);
line.material.depthTest = false;
line.material.opacity = 0.25;
line.material.transparent = true;

scene.add(line);
```

##### 构造器

`WireframeGeometry( geometry : BufferGeometry )`
geometry — 任意几何体对象。

##### 属性|方法

> 共有方法请参见其基类 BufferGeometry。
> 共有属性请参见其基类 BufferGeometry。

### 材质

#### Material

材质描述了对象 objects 的外观。它们的定义方式与渲染器无关， 因此，如果您决定使用不同的渲染器，不必重写材质。

##### 构造器

`Material()`该方法创建一个通用材质。

#### 属性

- `.alphaTest` : Float
  设置运行 alphaTest 时要使用的 alpha 值。如果不透明度低于此值，则不会渲染材质。默认值为 0。

- `.alphaToCoverage` : Boolean
  启用 alpha to coverage. 只能在开启了 MSAA 的渲染环境中使用 (当渲染器创建的时候 antialias 属性要 true 才能使用). 默认为 false.

- `.blendDst` : Integer
  混合目标。默认值为 OneMinusSrcAlphaFactor。 目标因子所有可能的取值请参阅 constants。 必须将材质的 blending 设置为 CustomBlending 才能生效。
- `.blendDstAlpha` : Integer
  .blendDst 的透明度。 默认值为 null.

- `.blendEquation` : Integer
  使用混合时所采用的混合方程式。默认值为 AddEquation。 混合方程式所有可能的取值请参阅 constants。 必须将材质的 blending 设置为 CustomBlending 才能生效。
  .blendEquationAlpha : Integer
  .blendEquation 的透明度. 默认值为 null.

- `.blending` : Blending
  在使用此材质显示对象时要使用何种混合。
  必须将其设置为 CustomBlending 才能使用自定义 blendSrc, blendDst 或者 [page:Constant blendEquation]。 混合模式所有可能的取值请参阅 constants。默认值为 NormalBlending。

- `.blendSrc` : Integer
  混合源。默认值为 SrcAlphaFactor。 源因子所有可能的取值请参阅 constants。
  必须将材质的 blending 设置为 CustomBlending 才能生效。

- `.blendSrcAlpha` : Integer
  .blendSrc 的透明度。 默认值为 null.

- `.clipIntersection` : Boolean
  更改剪裁平面的行为，以便仅剪切其交叉点，而不是它们的并集。默认值为 false。

- `.clippingPlanes` : Array
  用户定义的剪裁平面，在世界空间中指定为 THREE.Plane 对象。这些平面适用于所有使用此材质的对象。空间中与平面的有符号距离为负的点被剪裁（未渲染）。 这需要 WebGLRenderer.localClippingEnabled 为 true。 示例请参阅 WebGL / clipping /intersection。默认值为 null。

- `.clipShadows` : Boolean
  定义是否根据此材质上指定的剪裁平面剪切阴影。默认值为 false。

- `.colorWrite` : Boolean
  是否渲染材质的颜色。 这可以与网格的 renderOrder 属性结合使用，以创建遮挡其他对象的不可见对象。默认值为 true。

- `.defines` : Object
  注入 shader 的自定义对象。 以键值对形式的对象传递，{ MY_CUSTOM_DEFINE: '' , PI2: Math.PI \* 2 }。 这些键值对在顶点和片元着色器中定义。默认值为 undefined。

- `.depthFunc` : Integer
  使用何种深度函数。默认为 LessEqualDepth。 深度模式所有可能的取值请查阅 constants。

- `.depthTest` : Boolean
  是否在渲染此材质时启用深度测试。默认为 true。

- `.depthWrite` : Boolean
  渲染此材质是否对深度缓冲区有任何影响。默认为 true。

在绘制 2D 叠加时，将多个事物分层在一起而不创建 z-index 时，禁用深度写入会很有用。

- `.forceSinglePass` : Boolean
  决定双面透明的东西是否强制使用单通道渲染，默认为 false。

为了减少一些半透明物体的渲染错误，此引擎调用两次绘制来渲染渲染双面透明的东西。 但是此方案可能会导致在某些情况下使绘制调用次数翻倍，例如渲染一些平面的植物例如草精灵之类的。 在这些情况下，将 forceSinglePass 设置为 true 来使用单通道渲染来避免性能问题。

- `.isMaterial` : Boolean
  检查这个对象是否为材质 Material 的只读标记.

- `.stencilWrite` : Boolean
  是否对模板缓冲执行模板操作，如果执行写入或者与模板缓冲进行比较，这个值需要设置为 true。默认为 false。

- `.stencilWriteMask` : Integer
  写入模板缓冲区时所用的位元遮罩，默认为 0xFF。

- `.stencilFunc` : Integer
  使用模板比较时所用的方法，默认为 AlwaysStencilFunc。在模板函数 constants 中查看可用的值

- `.stencilRef` : Integer
  在进行模板比较或者模板操作的时候所用的基准值，默认为 0。

- `.stencilFuncMask` : Integer
  与模板缓冲进行比较时所使用的位元遮罩，默认为 0xFF

- `.stencilFail` : Integer
  当比较函数没有通过的时候要执行的模板操作，默认为 KeepStencilOp，在模板操作 constants 查看可用值。

- `.stencilZFail` : Integer
  当比较函数通过了但是深度检测没有通过的时候要执行的模板操作， 默认为 KeepStencilOp，在模板操作 constants 查看可用值。

- `.stencilZPass` : Integer
  当比较函数和深度检测都通过时要执行的模板操作，默认为 KeepStencilOp，在模板操作 constants 中查看可用值。

- `.id` : Integer
  此材质实例的唯一编号。

- `.name` : String
  对象的可选名称（不必是唯一的）。默认值为空字符串。

- `.needsUpdate` : Boolean
  指定需要重新编译材质。

- `.opacity` : Float
  在 0.0 - 1.0 的范围内的浮点数，表明材质的透明度。值 0.0 表示完全透明，1.0 表示完全不透明。
  如果材质的 transparent 属性未设置为 true，则材质将保持完全不透明，此值仅影响其颜色。 默认值为 1.0。
- `.polygonOffset` : Boolean
  是否使用多边形偏移。默认值为 false。这对应于 WebGL 的 GL_POLYGON_OFFSET_FILL 功能。

- `.polygonOffsetFactor` : Integer
  设置多边形偏移系数。默认值为 0。

- `.polygonOffsetUnits` : Integer
  设置多边形偏移单位。默认值为 0。

- `.precision` : String
  重写此材质渲染器的默认精度。可以是"highp", "mediump" 或 "lowp"。默认值为 null。

- `.premultipliedAlpha` : Boolean
  是否预乘 alpha（透明度）值。有关差异的示例，请参阅 WebGL / Materials / Physical / Transmission。 默认值为 false。

- `.dithering` : Boolean
  是否对颜色应用抖动以消除条带的外观。默认值为 false。

- `.shadowSide` : Integer
  定义投影的面。设置时，可以是 THREE.FrontSide, THREE.BackSide, 或 Materials。默认值为 null。
  如果为 null， 则面投射阴影确定如下：
  |||
  |-|-|
  |THREE.FrontSide| 背面|
  |THREE.BackSide |前面|
  |THREE.DoubleSide |双面|

- `.side` : Integer
  定义将要渲染哪一面 - 正面，背面或两者。 默认为 THREE.FrontSide。其他选项有 THREE.BackSide 和 THREE.DoubleSide。

- `.toneMapped` : Boolean
  定义这个材质是否会被渲染器的 toneMapping 设置所影响，默认为 true 。

- `.transparent` : Boolean
  定义此材质是否透明。这对渲染有影响，因为透明对象需要特殊处理，并在非透明对象之后渲染。
  设置为 true 时，通过设置材质的 opacity 属性来控制材质透明的程度。
  默认值为 false。

- `.type` : String
  值是字符串'Material'。不应该被更改，并且可以用于在场景中查找此类型的所有对象。

- `.uuid` : String
  此材质实例的 UUID，会自动分配，不应该被更改。

- `.version` : Integer
  开始为 0，会记录 .needsUpdate : Boolean 设置为 true 的次数。

- `.vertexColors` : Boolean
  是否使用顶点着色。默认值为 false。 此引擎支持 RGB 或者 RGBA 两种顶点颜色，取决于缓冲 attribute 使用的是三分量（RGB）还是四分量（RGBA）。

- `.visible` : Boolean
  此材质是否可见。默认为 true。

- `.userData` : Object
  一个对象，可用于存储有关 Material 的自定义数据。它不应该包含对函数的引用，因为这些函数不会被克隆。

##### 方法

> EventDispatcher 方法在此类中可用。

- `.clone ( ) : Material`
  返回与此材质具有相同参数的新材质。

- `.copy ( material : material ) : this`
  将被传入材质中的参数复制到此材质中。

- `.dispose () : undefined`
  处理材质。材质的纹理不会被处理。需要通过 Texture 处理。

- `.onBeforeCompile ( shader : Shader, renderer : WebGLRenderer ) : undefined`
  在编译 shader 程序之前立即执行的可选回调。此函数使用 shader 源码作为参数。用于修改内置材质。

和其他属性不一样的是，这个回调在.clone()，.copy() 和 .toJSON() 中不支持。

- `.customProgramCacheKey () : String`
  当用到 onBeforeCompile 回调的时候，这个回调函数可以用来定义在 onBeforeCompile 中使用的配置项，这样 three.js 就可以根据这个回调返回的字符串来判定使用一个缓存的编译好的着色器代码还是根据需求重新编译一段新的着色器代码。

例如一个 onBeforeCompile 回调函数包含了下面的条件语句:

```js
if (black) {
  shader.fragmentShader = shader.fragmentShader.replace(
    "gl_FragColor = vec4(1)",
    "gl_FragColor = vec4(0)"
  );
}
```

那么 customProgramCacheKey 就可以设置为:

```js
material.customProgramCacheKey = function () {
  return black ? "1" : "0";
};
```

和其他属性不一样的是，这个回调在.clone()，.copy() 和 .toJSON() 中不支持。

- `.setValues ( values : Object ) : undefined`
  values -- 具有参数的容器。 根据 values 设置属性。
- `.toJSON ( meta : Object ) : Object`
  meta -- 包含有元数据的对象，例如该对象的纹理或图片。 将 material 对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 基础网格材质(MeshBasicMaterial)

##### 构造器

`MeshBasicMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 Material 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

`.alphaMap` : Texture
alpha 贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为 null。

仅使用纹理的颜色，忽略 alpha 通道（如果存在）。 对于 RGB 和 RGBA 纹理，WebGL 渲染器在采样此纹理时将使用绿色通道， 因为在 DXT 压缩和未压缩 RGB 565 格式中为绿色提供了额外的精度。 Luminance-only 以及 luminance/alpha 纹理也仍然有效。

`.aoMap` : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为 null。aoMap 需要第二组 UV。

`.aoMapIntensity` : Float
环境遮挡效果的强度。默认值为 1。零是不遮挡效果。

`.color` : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

`.combine` : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为 THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity 在两种颜色之间进行混合。

`.envMap` : Texture
环境贴图。默认值为 null。

`.fog` : Boolean
材质是否受雾影响。默认为 true。

`.lightMap` : Texture
光照贴图。默认值为 null。lightMap 需要第二组 UV。

`.lightMapIntensity` : Float
烘焙光的强度。默认值为 1。

`.map` : Texture
颜色贴图。可以选择包括一个 alpha 通道，通常与.transparent 或.alphaTest。默认为 null。

`.reflectivity` : Float
环境贴图对表面的影响程度; 见.combine。默认值为 1，有效范围介于 0（无反射）和 1（完全反射）之间。

`.refractionRatio` : Float
空气的折射率（IOR）（约为 1）除以材质的折射率。它与环境映射模式 THREE.CubeRefractionMapping 和 THREE.EquirectangularRefractionMapping 一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过 1。默认值为 0.98。

`.specularMap` : Texture
材质使用的高光贴图。默认值为 null。

`.wireframe` : Boolean
将几何体渲染为线框。默认值为 false（即渲染为平面多边形）。

`.wireframeLinecap` : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

`.wireframeLinejoin` : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

`.wireframeLinewidth` : Float
控制线框宽度。默认值为 1。

由于 OpenGL Core Profile 与大多数平台上 WebGL 渲染器的限制， 无论如何设置该值，线宽始终为 1。

#### Lambert 网格材质(MeshLambertMaterial)

一种非光泽表面的材质，没有镜面高光。
由于反射率和光照模型的简单性，MeshPhongMaterial，MeshStandardMaterial 或者 MeshPhysicalMaterial 上使用这种材质时会以一些图形精度为代价，得到更高的性能。

##### 构造器

`MeshLambertMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从 Material 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

.alphaMap : Texture
alpha 贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为 null。

仅使用纹理的颜色，忽略 alpha 通道（如果存在）。 对于 RGB 和 RGBA 纹理，WebGL 渲染器在采样此纹理时将使用绿色通道， 因为在 DXT 压缩和未压缩 RGB 565 格式中为绿色提供了额外的精度。 Luminance-only 以及 luminance/alpha 纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为 null。aoMap 需要第二组 UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为 1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是 0-1。默认值为 1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.combine : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为 THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity 在两种颜色之间进行混合。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为 1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为 0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为 null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为 1。

.envMap : Texture
环境贴图。默认值为 null。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为 false。

.fog : Boolean
材质是否受雾影响。默认为 true。

.lightMap : Texture
光照贴图。默认值为 null。lightMap 需要第二组 UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为 1。

.map : Texture
颜色贴图。可以选择包括一个 alpha 通道，通常与.transparent 或.alphaTest。默认为 null。

.normalMap : Texture
用于创建法线贴图的纹理。RGB 值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为 THREE.TangentSpaceNormalMap（默认）和 THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是 0-1。默认值是 Vector2 设置为（1,1）。

.reflectivity : Float
环境贴图对表面的影响程度; 见.combine。默认值为 1，有效范围介于 0（无反射）和 1（完全反射）之间。

.refractionRatio : Float
空气的折射率（IOR）（约为 1）除以材质的折射率。它与环境映射模式 THREE.CubeRefractionMapping 和 THREE.EquirectangularRefractionMapping 一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过 1。默认值为 0.98。

.specularMap : Texture
材质使用的高光贴图。默认值为 null。

.wireframe : Boolean
将几何体渲染为线框。默认值为 false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为 1。

由于 OpenGL Core Profile 与 大多数平台上 WebGL 渲染器的限制，无论如何设置该值，线宽始终为 1。

#### Phong 网格材质(MeshPhongMaterial)

一种用于具有镜面高光的光泽表面的材质。

在 MeshStandardMaterial 或 MeshPhysicalMaterial 上使用此材质时，性能通常会更高 ，但会牺牲一些图形精度。

##### 构造器

`MeshPhongMaterial( parameters : Object )`

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从 Material 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

.alphaMap : Texture
alpha 贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为 null。

仅使用纹理的颜色，忽略 alpha 通道（如果存在）。 对于 RGB 和 RGBA 纹理，WebGL 渲染器在采样此纹理时将使用绿色通道， 因为在 DXT 压缩和未压缩 RGB 565 格式中为绿色提供了额外的精度。 Luminance-only 以及 luminance/alpha 纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为 null。aoMap 需要第二组 UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为 1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是 0-1。默认值为 1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.combine : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为 THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity 在两种颜色之间进行混合。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为 1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为 0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为 null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为 1。

.envMap : Texture
环境贴图。默认值为 null。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为 false。

.fog : Boolean
材质是否受雾影响。默认为 true。

.lightMap : Texture
光照贴图。默认值为 null。lightMap 需要第二组 UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为 1。

.map : Texture
颜色贴图。可以选择包括一个 alpha 通道，通常与.transparent 或.alphaTest。默认为 null。 纹理贴图颜色由漫反射颜色.color 调节。

.normalMap : Texture
用于创建法线贴图的纹理。RGB 值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为 THREE.TangentSpaceNormalMap（默认）和 THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是 0-1。默认值是 Vector2 设置为（1,1）。

.reflectivity : Float
环境贴图对表面的影响程度; 见.combine。默认值为 1，有效范围介于 0（无反射）和 1（完全反射）之间。

.refractionRatio : Float
空气的折射率（IOR）（约为 1）除以材质的折射率。它与环境映射模式 THREE.CubeRefractionMapping 和 THREE.EquirectangularRefractionMapping 一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过 1。默认值为 0.98。

.shininess : Float
.specular 高亮的程度，越高的值越闪亮。默认值为 30。

.specular : Color
材质的高光颜色。默认值为 0x111111（深灰色）的颜色 Color。

这定义了材质的光泽度和光泽的颜色。

.specularMap : Texture
镜面反射贴图值会影响镜面高光以及环境贴图对表面的影响程度。默认值为 null。

.wireframe : Boolean
将几何体渲染为线框。默认值为 false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为 1。

由于 OpenGL Core Profile 与 大多数平台上 WebGL 渲染器的限制，无论如何设置该值，线宽始终为 1。

#### 标准网格材质(MeshStandardMaterial)

一种基于物理的标准材质，使用 Metallic-Roughness 工作流程。

该材质提供了比 MeshLambertMaterial 或 MeshPhongMaterial 更精确和逼真的结果，代价是计算成本更高

##### 构造器

`MeshStandardMaterial( parameters : Object )`

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从 Material 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

.alphaMap : Texture
alpha 贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为 null。

仅使用纹理的颜色，忽略 alpha 通道（如果存在）。 对于 RGB 和 RGBA 纹理，WebGL 渲染器在采样此纹理时将使用绿色通道， 因为在 DXT 压缩和未压缩 RGB 565 格式中为绿色提供了额外的精度。 Luminance-only 以及 luminance/alpha 纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为 null。aoMap 需要第二组 UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为 1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是 0-1。默认值为 1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.defines : Object
如下形式的对象:`{ 'STANDARD': '' }`;WebGLRenderer 使用它来选择 shaders。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为 1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为 0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为 null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为 1。

.envMap : Texture
环境贴图，为了能够保证物理渲染准确，您应该添加由 PMREMGenerator 预处理过的环境贴图，默认为 null。

.envMapIntensity : Float
通过乘以环境贴图的颜色来缩放环境贴图的效果。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为 false。

.fog : Boolean
材质是否受雾影响。默认为 true。

.isMeshStandardMaterial : Boolean
检查当前对象是否为标准网格材质的标记。

.lightMap : Texture
光照贴图。默认值为 null。lightMap 需要第二组 UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为 1。

.map : Texture
颜色贴图。可以选择包括一个 alpha 通道，通常与.transparent 或.alphaTest。默认为 null。 纹理贴图颜色由漫反射颜色.color 调节。

.metalness : Float
材质与金属的相似度。非金属材质，如木材或石材，使用 0.0，金属使用 1.0，通常没有中间值。 默认值为 0.0。0.0 到 1.0 之间的值可用于生锈金属的外观。如果还提供了 metalnessMap，则两个值相乘。

.metalnessMap : Texture
该纹理的蓝色通道用于改变材质的金属度。

.normalMap : Texture
用于创建法线贴图的纹理。RGB 值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为 THREE.TangentSpaceNormalMap（默认）和 THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是 0-1。默认值是 Vector2 设置为（1,1）。

.refractionRatio : Float
空气的折射率（IOR）（约为 1）除以材质的折射率。它与环境映射模式 THREE.CubeRefractionMapping 和 THREE.EquirectangularRefractionMapping 一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过 1。默认值为 0.98。

.roughness : Float
材质的粗糙程度。0.0 表示平滑的镜面反射，1.0 表示完全漫反射。默认值为 1.0。如果还提供 roughnessMap，则两个值相乘。

.roughnessMap : Texture
该纹理的绿色通道用于改变材质的粗糙度。

.wireframe : Boolean
将几何体渲染为线框。默认值为 false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为 1。
由于 OpenGL Core Profile 与 大多数平台上 WebGL 渲染器的限制，无论如何设置该值，线宽始终为 1。

#### 物理网格材质(MeshPhysicalMaterial)

提供了更高级的基于物理的渲染属性
物理网格材质使用了更复杂的着色器功能，所以在每个像素的渲染都要比 three.js 中的其他材质更费性能，大部分的特性是默认关闭的，需要手动开启，每开启一项功能在开启的时候才会更耗性能。请注意，为获得最佳效果，您在使用此材质时应始终指定 environment map。

##### 构造器

`MeshPhysicalMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从 Material 和 MeshStandardMaterial 继承的任何属性)

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

#### 基础线条材质（LineBasicMaterial）

```js
const material = new THREE.LineBasicMaterial({
  color: 0xffffff,
  linewidth: 1,
  linecap: "round", //ignored by WebGLRenderer
  linejoin: "round", //ignored by WebGLRenderer
});
```

##### 构造器

`LineBasicMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 Material 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

- `.color : Color`
  材质的颜色(Color)，默认值为白色 (0xffffff)。

- `.fog : Boolean`
  材质是否受雾影响。默认为 true。

- `.linewidth : Float`
  控制线宽。默认值为 1。

由于 OpenGL Core Profile 与 大多数平台上 WebGL 渲染器的限制，无论如何设置该值，线宽始终为 1。

- `.linecap : String`
  定义线两端的样式。可选值为 'butt', 'round' 和 'square'。默认值为 'round'。

该属性对应 2D Canvas lineCap 属性， 并且会被 WebGL 渲染器忽略。

- `.linejoin : String`
  定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应 2D Canvas lineJoin 属性， 并且会被 WebGL 渲染器忽略。

- `.map : Texture`
  Sets the color of the lines using data from a Texture.

#### 虚线材质(LineDashedMaterial)

```js
const material = new THREE.LineDashedMaterial({
  color: 0xffffff,
  linewidth: 1,
  scale: 1,
  dashSize: 3,
  gapSize: 1,
});
```

##### 构造器

`LineDashedMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 LineBasicMaterial 继承的任何属性)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。

- `.dashSize` : number
  虚线的大小，是指破折号和间隙之和。默认值为 3。

- `.gapSize` : number
  间隙的大小，默认值为 1。

- `.isLineDashedMaterial` : Boolean
  Read-only flag to check if a given object is of type LineDashedMaterial.

- `.scale` : number
  线条中虚线部分的占比。默认值为 1。

#### 卡通着色的材质（MeshToonMaterial）

##### 构造器

`MeshToonMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 LineBasicMaterial 继承的任何属性)。

#### 阴影材质(ShadowMaterial)

此材质可以接收阴影，但在其他方面完全透明。

```js
const geometry = new THREE.PlaneGeometry(2000, 2000);
geometry.rotateX(-Math.PI / 2);

const material = new THREE.ShadowMaterial();
material.opacity = 0.2;

const plane = new THREE.Mesh(geometry, material);
plane.position.y = -200;
plane.receiveShadow = true;
scene.add(plane);
```

##### 构造器

`ShadowMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 LineBasicMaterial 继承的任何属性)。

##### 属性|方法

> 共有方法请参见其基类 Material。
> 共有属性请参见其基类 Material。
> .color : Color
> Color of the material, by default set to black (0x000000).

.fog : Boolean
材质是否受雾影响。默认为 true。

.transparent : Boolean
定义此材质是否透明。默认值为 true。

#### 点材质(PointsMaterial)

##### 构造器

`PointsMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从 LineBasicMaterial 继承的任何属性)。
属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用 Color.set(color)。

#### 点精灵材质(SpriteMaterial)

```js
const map = new THREE.TextureLoader().load("textures/sprite.png");
const material = new THREE.SpriteMaterial({ map: map, color: 0xffffff });

const sprite = new THREE.Sprite(material);
sprite.scale.set(200, 200, 1);
scene.add(sprite);
```

##### 构造器

`SpriteMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从 Material 和 ShaderMaterial 继承的任何属性)。

属性 color 例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色）， 内部调用 Color.set(color)。 SpriteMaterials 不会被 Material.clippingPlanes 裁剪。

##### 属性|方法

.alphaMap : Texture
alpha 贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为 null。

仅使用纹理的颜色，忽略 alpha 通道（如果存在）。 对于 RGB 和 RGBA 纹理，WebGL 渲染器在采样此纹理时将使用绿色通道， 因为在 DXT 压缩和未压缩 RGB 565 格式中为绿色提供了额外的精度。 Luminance-only 以及 luminance/alpha 纹理也仍然有效。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。 .map 会和 color 相乘。

.fog : Boolean
材质是否受雾影响。默认为 true。

.isSpriteMaterial : Boolean
Read-only flag to check if a given object is of type SpriteMaterial.

.map : Texture
颜色贴图。可以选择包括一个 alpha 通道，通常与.transparent 或.alphaTest。默认为 null。

.rotation : Radians
sprite 的转动，以弧度为单位。默认值为 0。

.sizeAttenuation : Boolean
精灵的大小是否会被相机深度衰减。（仅限透视摄像头。）默认为 true。

.transparent : Boolean
定义此材质是否透明。默认值为 true。

### 物体

#### 三维物体（Object3D）

这是 Three.js 中大部分对象的基类，提供了一系列的属性和方法来对三维空间中的物体进行操纵。

请注意，可以通过.add( object )方法来将对象进行组合，该方法将对象添加为子对象，但为此最好使用 Group（来作为父对象）

##### 构造器

`Object3D()`
构造器中不带有参数。

##### 属性

- `.animations` : AnimationClip
  三维物体所属的动画剪辑数组.

- `.castShadow` : Boolean
  对象是否被渲染到阴影贴图中。默认值为 false。

- `.children` : Array
  含有对象的子级的数组。请参阅 Group 来了解将手动对象进行分组的相关信息。

- `.customDepthMaterial` : Material
  在渲染到深度图的时候所用的自定义深度材质。 只能在网格中使用。 当使用 DirectionalLight（平行光）或者 SpotLight（聚光灯光）生成影子的时候, 如果你调整过顶点着色器中的顶点位置，就需要定义一个自定义深度材质来生成正确的影子。默认为 undefined.

- `.customDistanceMaterial` : Material
  与 customDepthMaterial 相同，但与 PointLight(点光源）一起使用。默认值为 undefined。

- `.frustumCulled` : Boolean
  当这个设置了的时候，每一帧渲染前都会检测这个物体是不是在相机的视椎体范围内。 如果设置为 false 物体不管是不是在相机的视椎体范围内都会渲染。默认为 true。

- `.id` : Integer
  只读 —— 表示该对象实例 ID 的唯一数字。

- `.isObject3D` : Boolean
  查看所给对象是不是 Object3D 类型的只读标记.

- `.layers` : Layers
  物体的层级关系。 物体只有和一个正在使用的 Camera 至少在同一个层时才可见。当使用 Raycaster 进行射线检测的时候此项属性可以用于过滤不参与检测的物体.

- `.matrix` : Matrix4
  局部变换矩阵。

- `.matrixAutoUpdate` : Boolean
  当这个属性设置了之后，它将计算每一帧的位移、旋转（四元变换）和缩放矩阵，并重新计算 matrixWorld 属性。默认值是 Object3D.DEFAULT_MATRIX_AUTO_UPDATE (true)。

- `.matrixWorld` : Matrix4
  物体的世界变换。若这个 Object3D 没有父级，则它将和 local transform .matrix（局部变换矩阵）相同。

- `.matrixWorldAutoUpdate` : Boolean
  默认为 true. 当设置的时候，渲染器在每一帧都会检查物体自身以及它的自带是否需要更新世界变换矩阵。 如果不需要的话它自身以及它的子代的所有世界变换矩阵都需要你来维护。

- `.matrixWorldNeedsUpdate` : Boolean
  当这个属性设置了之后，它将计算在那一帧中的 matrixWorld，并将这个值重置为 false。默认值为 false。

- `.modelViewMatrix` : Matrix4
  这个值传递给着色器，用于计算物体的位置。

- `.name` : String
  对象的名称，可选、不必唯一。默认值是一个空字符串。

- `.normalMatrix` : Matrix3
  这个值传递给着色器，用于计算物体的光照。 它是物体的 modelViewMatrix 矩阵中，左上角 3x3 子矩阵的逆的转置矩阵。

使用这个特殊矩阵的原因，是只需使用 modelViewMatrix 就可以得出一个法线（缩放时）的非单位长度或者非垂直的方向（不规则缩放时）。

另一方面，modelViewMatrix 矩阵中的位移部分和法线的计算无关，因此 Matrix3 就已经足够了。

- `.onAfterRender` : Function
  一个可选的回调函数，在 Object3D 渲染之后直接执行。 使用以下参数来调用此函数：renderer，scene，camera，geometry，material，group。

注意此回调函数只会在*可渲染*的 3D 物体上执行。可渲染的 3D 物体指的是那种拥有视觉表现的、定义了几何体与材质的物体，例如像是 Mesh、Line、Points 或者 Sprite。 Object3D、 Group 或者 Bone 这些是不可渲染的物体，因此此回调函数不会在这样的物体上执行。

- `.onBeforeRender` : Function
  一个可选的回调函数，在 Object3D 渲染之前直接执行。 使用以下参数来调用此函数：renderer，scene，camera，geometry，material，group。

注意此回调函数只会在*可渲染*的 3D 物体上执行。可渲染的 3D 物体指的是那种拥有视觉表现的、定义了几何体与材质的物体，例如像是 Mesh、Line、Points 或者 Sprite。 Object3D、 Group 或者 Bone 这些是不可渲染的物体，因此此回调函数不会在这样的物体上执行。

- `.parent` : Object3D
  在 scene graph（场景图）中，一个对象的父级对象。 一个对象最多仅能有一个父级对象。

- `.position` : Vector3
  表示对象局部位置的 Vector3。默认值为(0, 0, 0)。

- `.quaternion` : Quaternion
  表示对象局部旋转的 Quaternion（四元数）。

- `.receiveShadow` : Boolean
  材质是否接收阴影。默认值为 false。

- `.renderOrder` : Number
  这个值将使得 scene graph（场景图）中默认的的渲染顺序被覆盖， 即使不透明对象和透明对象保持独立顺序。 渲染顺序是由低到高来排序的，默认值为 0。

- `.rotation` : Euler
  物体的局部旋转，以弧度来表示。（请参阅 Euler angles-欧拉角）

- `.scale` : Vector3
  物体的局部缩放。默认值是 Vector3( 1, 1, 1 )。

- `.up` : Vector3
  这个属性由 lookAt 方法所使用，例如，来决定结果的朝向。 默认值是 Object3D.DEFAULT_UP，即( 0, 1, 0 )。

- `.userData` : Object
  一个用于存储 Object3D 自定义数据的对象。 它不应当包含对函数的引用，因为这些函数将不会被克隆。

- `.uuid` : String
  该对象实例的 UUID。 这是一个自动生成的值，不应当对其进行修改。

- `.visible` : Boolean
  可见性。这个值为 true 时，物体将被渲染。默认值为 true。

静态属性
静态属性和方法由每个类所定义，并非由每个类的实例所定义。 也就是说，改变 Object3D.DEFAULT_UP 或 Object3D.DEFAULT_MATRIX_AUTO_UPDATE 的值， 将改变每个在此之后由 Object3D 类（或派生类）创建的实例中的 up 和 matrixAutoUpdate 的值。（已经创建好的 Object3D 不会受到影响）。

- `.DEFAULT_UP` : Vector3
  默认的物体的 up 方向，同时也作为 DirectionalLight、HemisphereLight 和 Spotlight（自顶向下创建的灯光）的默认方向。 默认设为( 0, 1, 0 )。

- `.DEFAULT_MATRIX_AUTO_UPDATE` : Boolean
  matrixAutoUpdate 的默认设置，用于新创建的 Object3D。
  .DEFAULT_MATRIX_WORLD_AUTO_UPDATE : Boolean
  matrixWorldAutoUpdate 的默认设置，用于新创建的 Object3D。

##### 方法

EventDispatcher 在该类上可用的所有方法。

- `.add ( object : Object3D, ... ) : this`
  添加对象到这个对象的子级，可以添加任意数量的对象。 当前传入的对象中的父级将在这里被移除，因为一个对象仅能有一个父级。

请参阅 Group 来查看手动编组对象的相关信息。

- `.applyMatrix4 ( matrix : Matrix4 ) : undefined`
  对当前物体应用这个变换矩阵，并更新物体的位置、旋转和缩放。

- `.applyQuaternion ( quaternion : Quaternion ) : this`
  对当前物体应用由四元数所表示的变换。

- `.attach ( object : Object3D ) : this`
  将 object 作为子级来添加到该对象中，同时保持该 object 的世界变换。

- `.clone ( recursive : Boolean ) : Object3D`
  recursive —— 如果值为 true，则该物体的后代也会被克隆。默认值为 true。

返回对象前物体的克隆（以及可选的所有后代）。

- `.copy ( object : Object3D, recursive : Boolean ) : this`
  recursive —— 如果值为 true，则该物体的后代也会被复制。默认值为 true。

复制给定的对象到这个对象中。 请注意，事件监听器和用户定义的回调函数（.onAfterRender 和 .onBeforeRender）不会被复制。

- `.getObjectById ( id : Integer ) : Object3D`
  id —— 标识该对象实例的唯一数字。

从该对象开始，搜索一个对象及其子级，返回第一个带有匹配 id 的子对象。
请注意，id 是按照时间顺序来分配的：1、2、3、……，每增加一个新的对象就自增 1。

- `.getObjectByName ( name : String ) : Object3D`
  name —— 用于来匹配子物体中 Object3D.name 属性的字符串。

从该对象开始，搜索一个对象及其子级，返回第一个带有匹配 name 的子对象。
请注意，大多数的对象中 name 默认是一个空字符串，要使用这个方法，你将需要手动地设置 name 属性。

- `.getObjectByProperty ( name : String, value : Any ) : Object3D`
  name —— 将要用于查找的属性的名称。
  value —— 给定的属性的值。

从该对象开始，搜索一个对象及其子级，返回第一个给定的属性中包含有匹配的值的子对象。

- `.getObjectsByProperty ( name : String, value : Any ) : Object3D`
  name —— 将要用于查找的属性的名称。
  value —— 给定的属性的值。

从此对象开始，搜索一个对象及其子对象，返回包含给定属性的匹配值的所有子对象。

- `.getWorldPosition ( target : Vector3 ) : Vector3`
  target — 结果将被复制到这个 Vector3 中。

返回一个表示该物体在世界空间中位置的矢量。

- `.getWorldQuaternion ( target : Quaternion ) : Quaternion`
  target — 结果将被复制到这个 Quaternion 中。

返回一个表示该物体在世界空间中旋转的四元数。

- `.getWorldScale ( target : Vector3 ) : Vector3`
  target — 结果将被复制到这个 Vector3 中。

返回一个包含着该物体在世界空间中各个轴向上所应用的缩放因数的矢量。

- `.getWorldDirection ( target : Vector3 ) : Vector3`
  target — 结果将被复制到这个 Vector3 中。

返回一个表示该物体在世界空间中 Z 轴正方向的矢量。

- `.localToWorld ( vector : Vector3 ) : Vector3`
  vector - 一个表示在该物体局部空间中位置的向量。

将该向量从物体的局部空间转换到世界空间。

- `.lookAt ( vector : Vector3 ) : undefined`
  .lookAt ( x : Float, y : Float, z : Float ) : undefined
  vector - 一个表示世界空间中位置的向量。

也可以使用世界空间中 x、y 和 z 的位置分量。

旋转物体使其在世界空间中面朝一个点。

这一方法不支持其父级被旋转过或者被位移过的物体。

- `.raycast ( raycaster : Raycaster, intersects : Array ) : undefined`
  抽象（空方法），在一条被投射出的射线与这个物体之间获得交点。 在一些子类，例如 Mesh, Line, and Points 实现了这个方法，以用于光线投射。

- `.remove ( object : Object3D, ... ) : this`
  从当前对象的子级中移除对象。可以移除任意数量的对象。

- `.removeFromParent () : this`
  Removes this object from its current parent.

- `.rotateOnAxis ( axis : Vector3, angle : Float ) : this`
  axis —— 一个在局部空间中的标准化向量。
  angle —— 角度，以弧度来表示。

在局部空间中绕着该物体的轴来旋转一个物体，假设这个轴已被标准化。

- `.rotateOnWorldAxis ( axis : Vector3, angle : Float ) : this`
  axis -- 一个在世界空间中的标准化向量。
  angle -- 角度，以弧度来表示。

在世界空间中绕着该物体的轴来旋转一个物体，假设这个轴已被标准化。 方法假设该物体没有旋转过的父级。

- `.rotateX ( rad : Float ) : this`
  rad - 将要旋转的角度（以弧度来表示）。

绕局部空间的 X 轴旋转这个物体。

- `.rotateY ( rad : Float ) : this`
  rad - 将要旋转的角度（以弧度来表示）。

绕局部空间的 Y 轴旋转这个物体。

- `.rotateZ ( rad : Float ) : this`
  rad - 将要旋转的角度（以弧度来表示）。

绕局部空间的 Z 轴旋转这个物体。

- `.setRotationFromAxisAngle ( axis : Vector3, angle : Float ) : undefined`
  axis -- 一个在局部空间中的标准化向量。
  angle -- 角度（以弧度来表示）。

调用.quaternion 中的 setFromAxisAngle( axis, angle )。

- `.setRotationFromEuler ( euler : Euler ) : undefined`
  euler -- 指定了旋转量的欧拉角。
  调用.quaternion 中的 setRotationFromEuler( euler)。

- `.setRotationFromMatrix ( m : Matrix4 ) : undefined`
  m -- 通过该矩阵中的旋转分量来旋转四元数。
  调用.quaternion 中的 setFromRotationMatrix( m)。

请注意，这里假设 m 上的 3x3 矩阵是一个纯旋转矩阵（即未缩放的矩阵）。

- `.setRotationFromQuaternion ( q : Quaternion ) : undefined`
  q -- 标准化的四元数。

将所给的四元数复制到.quaternion 中。

- `.toJSON ( meta : Object ) : Object`
  meta -- 包含有元数据的对象，例如该对象的材质、纹理或图片。 将对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

- `.translateOnAxis ( axis : Vector3, distance : Float ) : this`
  axis -- 一个在局部空间中的标准化向量。
  distance -- 将要平移的距离。

在局部空间中沿着一条轴来平移物体，假设轴已被标准化。

- `.translateX ( distance : Float ) : this`
  沿着 X 轴将平移 distance 个单位。

- `.translateY ( distance : Float ) : this`
  沿着 Y 轴将平移 distance 个单位。

- `.translateZ ( distance : Float ) : this`
  沿着 Z 轴将平移 distance 个单位。

- `.traverse ( callback : Function ) : undefined`
  callback - 以一个 object3D 对象作为第一个参数的函数。

在对象以及后代中执行的回调函数。

- `.traverseVisible ( callback : Function ) : undefined`
  callback - 以一个 object3D 对象作为第一个参数的函数。

类似 traverse 函数，但在这里，回调函数仅对可见的对象执行，不可见对象的后代将不遍历。

- `.traverseAncestors ( callback : Function ) : undefined`
  callback - 以一个 object3D 对象作为第一个参数的函数。

在所有的祖先中执行回调函数。

- `.updateMatrix () : undefined`
  更新局部变换。

- `.updateMatrixWorld ( force : Boolean ) : undefined`
  更新物体及其后代的全局变换。

- `.updateWorldMatrix ( updateParents : Boolean, updateChildren : Boolean ) : undefined`
  updateParents - 递归更新物体的所有祖先的全局变换.
  updateChildren - 递归更新物体的所有后代的全局变换.

更新物体的全局变换。

- `.worldToLocal ( vector : Vector3 ) : Vector3`
  vector - 一个表示在世界空间中位置的向量。

将该向量从世界空间转换到物体的局部空间。

#### 网格模型（Mesh）

表示基于以三角形为 polygon mesh（多边形网格）的物体的类。 同时也作为其他类的基类
网格模型 Mesh 其实就一个一个三角形(面)拼接构成。使用使用网格模型 Mesh 渲染几何体 geometry，就是几何体所有顶点坐标三个为一组，构成一个三角形，多组顶点构成多个三角形，就可以用来模拟表示物体的表面。
空间中一个三角形有正反两面，相机对着三角形的一个面，如果三个顶点的顺序是逆时针方向，该面视为正面，如果三个顶点的顺序是顺时针方向，该面视为反面。

```js
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

网格模型 Mesh 对应的几何体 BufferGeometry，拆分为多个三角后，很多三角形重合的顶点位置坐标是相同的，这时候如果你想减少顶点坐标数据量，可以借助几何体顶点索引 geometry.index 来实现。

```js
//每个三角形3个顶点坐标，矩形平面可以拆分为两个三角形，也就是6个顶点坐标。
const vertices = new Float32Array([
  0,
  0,
  0, //顶点1坐标
  80,
  0,
  0, //顶点2坐标
  80,
  80,
  0, //顶点3坐标
  0,
  0,
  0, //顶点4坐标   和顶点1位置相同
  80,
  80,
  0, //顶点5坐标  和顶点3位置相同
  0,
  80,
  0, //顶点6坐标
]);
//如果几何体有顶点索引geometry.index，那么你可以吧三角形重复的顶点位置坐标删除。
const vertices = new Float32Array([
  0,
  0,
  0, //顶点1坐标
  80,
  0,
  0, //顶点2坐标
  80,
  80,
  0, //顶点3坐标
  0,
  80,
  0, //顶点4坐标
]);
```

BufferAttribute 定义顶点索引.index 数据
通过 javascript 类型化数组 Uint16Array 创建顶点索引.index 数据。

```js
// Uint16Array类型数组创建顶点索引数据
const indexes = new Uint16Array([
  // 下面索引值对应顶点位置数据中的顶点坐标
  0, 1, 2, 0, 2, 3,
]);
```

通过 threejs 的属性缓冲区对象 BufferAttribute 表示几何体顶点索引.index 数据。

```js
// 索引数据赋值给几何体的index属性
geometry.index = new THREE.BufferAttribute(indexes, 1); //1个为一组
```

#### 构造器

`Mesh( geometry : BufferGeometry, material : Material )`
geometry —— （可选）BufferGeometry 的实例，默认值是一个新的 BufferGeometry。
material —— （可选）一个 Material，或是一个包含有 Material 的数组，默认是一个新的 MeshBasicMa`terial。

##### 属性

共有属性请参见其基类 Object3D。

.geometry : BufferGeometry
BufferGeometry 的实例或者派生类，定义了物体的结构。

.isMesh : Boolean
Read-only flag to check if a given object is of type Mesh.

.material : Material
由 Material 基类或者一个包含材质的数组派生而来的材质实例，定义了物体的外观。默认值是一个 MeshBasicMaterial。

.morphTargetInfluences : Array
一个包含有权重（值一般在 0-1 范围内）的数组，指定应用了多少变形。 默认情况下是未定义的，但是会被 updateMorphTargets 重置为一个空数组。

.morphTargetDictionary : Object
基于 morphTarget.name 属性的 morphTargets 字典。 默认情况下是未定义的，但是会被 updateMorphTargets 重建。

##### 方法

共有方法请参见其基类 Object3D。

.clone () : Mesh
返回这个 Mesh 对象及其子级的克隆。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的 Ray（射线）和这个网格之间产生交互。 Raycaster.intersectObject 将会调用这个方法。

.updateMorphTargets () : undefined
更新 morphTargets，使其不对对象产生影响，重置 morphTargetInfluences and morphTargetDictionary 属性。

#### 点（Points）

一个用于显示点的类。 由 WebGLRenderer 渲染的点使用 gl.POINTS。

##### 构造器

`Points( geometry : BufferGeometry, material : Material )`
geometry —— （可选）是一个 BufferGeometry 的实例，默认值是一个新的 BufferGeometry。几何体 geometry 作为点模型 Points 参数，会把几何体渲染为点
material —— （可选） 是一个对象，默认值是一个 PointsMaterial。

##### 属性

共有属性请参见其基类 Object3D。

.geometry : BufferGeometry
一个 BufferGeometry 的实例（或者派生类），定义了物体的结构。

.isPoints : Boolean
Read-only flag to check if a given object is of type Points.

.material : Material
Material 的实例。定义了物体的外观。默认值是一个的 PointsMaterial。

.morphTargetInfluences : Array
一个包含有权重（值一般在 0-1 范围内）的数组，指定应用了多少变形。 默认情况下是未定义的，但是会被 updateMorphTargets 重置为一个空数组。

.morphTargetDictionary : Object
基于 morphTarget.name 属性的 morphTargets 字典。 默认情况下是未定义的，但是会被 updateMorphTargets 重建。

##### 方法

共有方法请参见其基类 Object3D。

.clone () : Mesh
返回这个 Mesh 对象及其子级的克隆。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的 Ray（射线）和这个网格之间产生交互。 Raycaster.intersectObject 将会调用这个方法。

.updateMorphTargets () : undefined
更新 morphTargets，使其不对对象产生影响，重置 morphTargetInfluences and morphTargetDictionary 属性。

#### 精灵（Sprite）

精灵是一个总是面朝着摄像机的平面，通常含有使用一个半透明的纹理。精灵不会投射任何阴影

```js
const map = new THREE.TextureLoader().load("sprite.png");
const material = new THREE.SpriteMaterial({ map: map });

const sprite = new THREE.Sprite(material);
scene.add(sprite);
```

##### 构造器

Sprite( material : Material )
material - （可选值）是 SpriteMaterial 的一个实例。 默认值是一个白色的 SpriteMaterial。

创建一个新的 Sprite。

##### 属性

共有属性请参见其基类 Object3D。

.isSprite : Boolean
Read-only flag to check if a given object is of type Sprite.

.material : SpriteMaterial
SpriteMaterial 的一个实例，定义了这个对象的外观。默认值是一个白色的 SpriteMaterial。

.center : Vector2
这个精灵的锚点，也就是精灵旋转时，围绕着旋转的点。当值为(0.5,0.5)时，对应着这个精灵的中心点；当值为(0,0)时，对应着这个精灵左下角的点。
其默认值是(0.5,0.5)。

##### 方法

共有方法请参见其基类 Object3D。

.clone () : Sprite
返回当前 Sprite 对象的一个克隆及其任何后代。

.copy ( sprite : Sprite ) : this
将前一个 Sprite 对象的属性复制给当前的这个对象。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在投射的光线和精灵之前产生交互。Raycaster.intersectObject 将会调用这个方法。 在对 sprite 进行射线投射之前，射线投射必须通过调用 Raycaster.setFromCamera()来初始化。

#### 线（Line）

它几乎和 LineSegments 是一样的，唯一的区别是它在渲染时使用的是 gl.LINE_STRIP， 而不是 gl.LINES。

```js
const material = new THREE.LineBasicMaterial({
  color: 0x0000ff,
});

const points = [];
points.push(new THREE.Vector3(-10, 0, 0));
points.push(new THREE.Vector3(0, 10, 0));
points.push(new THREE.Vector3(10, 0, 0));

const geometry = new THREE.BufferGeometry().setFromPoints(points);

const line = new THREE.Line(geometry, material);
scene.add(line);
```

##### 构造器

`Line( geometry : BufferGeometry, material : Material )`
geometry —— 表示线段的顶点，默认值是一个新的 BufferGeometry。几何体作为线模型 Line (opens new window)的参数，你会发现渲染效果是从第一个点开始到最后一个点，依次连成线。
material —— 线的材质，默认值是一个新的具有随机颜色的 LineBasicMaterial。

##### 属性

共有属性请参见其基类 Object3D。

.geometry : BufferGeometry
表示线段的顶点。

.isLine : Boolean
Read-only flag to check if a given object is of type Line.

.material : Material
线的材质。

.morphTargetInfluences : Array
An array of weights typically from 0-1 that specify how much of the morph is applied. Undefined by default, but reset to a blank array by .updateMorphTargets().

.morphTargetDictionary : Object
A dictionary of morphTargets based on the morphTarget.name property. Undefined by default, but rebuilt .updateMorphTargets().

##### 方法

共有方法请参见其基类 Object3D。

.computeLineDistances () : this
计算 LineDashedMaterial 所需的距离的值的数组。 对于几何体中的每一个顶点，这个方法计算出了当前点到线的起始点的累积长度。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的 Ray（射线）和这条线之间产生交互。 Raycaster.intersectObject 将会调用这个方法。

.clone () : Line
返回这条线及其子集的一个克隆对象。

.updateMorphTargets () : undefined
Updates the morphTargets to have no influence on the object. Resets the .morphTargetInfluences and .morphTargetDictionary properties.

#### 组（Group）

它几乎和 Object3D 是相同的，其目的是使得组中对象在语法上的结构更加清晰。

```js
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });

const cubeA = new THREE.Mesh(geometry, material);
cubeA.position.set(100, 100, 0);

const cubeB = new THREE.Mesh(geometry, material);
cubeB.position.set(-100, -100, 0);

//create a group and add the two cubes
//These cubes can now be rotated / scaled etc as a group
const group = new THREE.Group();
group.add(cubeA);
group.add(cubeB);

scene.add(group);
```

##### 构造器

`Group( )`

##### 属性|方法

> 共有属性请参见其基类 Object3D。
> 共有方法请参见其基类 Object3D。

.isGroup : Boolean
Read-only flag to check if a given object is of type Group.

.type : String
一个字符串：“Group”。这个属性不应当被改变。

#### 实例化网格（InstancedMesh）

一种具有实例化渲染支持的特殊版本的 Mesh。你可以使用 InstancedMesh 来渲染大量具有相同几何体与材质、但具有不同世界变换的物体。 使用 InstancedMesh 将帮助你减少 draw call 的数量，从而提升你应用程序的整体渲染性能。

##### 构造器

`InstancedMesh( geometry : BufferGeometry, material : Material, count : Integer )`
geometry - 一个 BufferGeometry 的实例。
material - 一个 Material 的实例。默认为一个新的 MeshBasicMaterial 。
count - 实例的数量

### 渲染器

#### WebGLRenderer

构造器
`WebGLRenderer( parameters : Object )`
parameters - (可选) 该对象的属性定义了渲染器的行为。也可以完全不传参数。在所有情况下，当缺少参数时，它将采用合理的默认值。 以下是合法参数：

`canvas` - 一个供渲染器绘制其输出的 canvas 它和下面的 domElement 属性对应。 如果没有传这个参数，会创建一个新 canvas
`context` - 可用于将渲染器附加到已有的渲染环境(RenderingContext)中。默认值是 null
`precision` - 着色器精度. 可以是 "highp", "mediump" 或者 "lowp". 如果设备支持，默认为"highp" .
`alpha` - controls the default clear alpha value. When set to true, the value is 0. Otherwise it's 1. Default is false.
`premultipliedAlpha` - renderer 是否假设颜色有 premultiplied alpha. 默认为 true
`antialias` - 是否执行抗锯齿。默认为 false.
`stencil` - 绘图缓存是否有一个至少 8 位的模板缓存(stencil buffer)。默认为 true
`preserveDrawingBuffer` -是否保留缓直到手动清除或被覆盖。 默认 false.
`powerPreference` - 提示用户代理怎样的配置更适用于当前 WebGL 环境。 可能是"high-performance", "low-power" 或 "default"。默认是"default". 详见 WebGL spec
`failIfMajorPerformanceCaveat` - 检测渲染器是否会因性能过差而创建失败。默认为 false。详见 WebGL spec for details.
`depth` - 绘图缓存是否有一个至少 6 位的深度缓存(depth buffer )。 默认是 true.
`logarithmicDepthBuffer` - 是否使用对数深度缓存。如果要在单个场景中处理巨大的比例差异，就有必要使用。 Note that this setting uses gl_FragDepth if available which disables the Early Fragment Test optimization and can cause a decrease in performance. 默认是 false。 示例：camera / logarithmicdepthbuffer

##### 属性

- `.autoClear` : Boolean
  定义渲染器是否在渲染每一帧之前自动清除其输出。

- `.autoClearColor` : Boolean
  如果 autoClear 为 true, 定义 renderer 是否清除颜色缓存。 默认是 true

- `.autoClearDepth` : Boolean
  如果 autoClear 是 true, 定义 renderer 是否清除深度缓存。 默认是 true

- `.autoClearStencil` : Boolean
  如果 autoClear 是 true, 定义 renderer 是否清除模板缓存. 默认是 true

- `.debug` : Object

  - checkShaderErrors: 如果为 true，定义是否检查材质着色器程序 编译和链接过程中的错误。 禁用此检查生产以获得性能增益可能很有用。 强烈建议在开发期间保持启用这些检查。 如果着色器没有编译和链接 - 它将无法工作，并且相关材料将不会呈现。 默认是 true
  - onShaderError( gl, program, glVertexShader, glFragmentShader ): A callback function that can be used for custom error reporting. The callback receives the WebGL context, an instance of WebGLProgram as well two instances of WebGLShader representing the vertex and fragment shader. Assigning a custom function disables the default error reporting. Default is null.

- `.capabilities` : Object
  一个包含当前渲染环境(RenderingContext)的功能细节的对象。

  - floatFragmentTextures: 环境是否支持 OES_texture_float 扩展
  - floatVertexTextures: 如果 floatFragmentTextures 和 vertexTextures 都是 true， 则此值为 true
  - getMaxAnisotropy(): 返回最大可用各向异性。
  - getMaxPrecision(): 返回顶点着色器和片元着色器的最大可用精度。
  - isWebGL2: true if the context in use is a WebGL2RenderingContext object.
  - logarithmicDepthBuffer: 如果 logarithmicDepthBuffer 在构造器中被设为 true 且 环境支持 EXT_frag_depth 扩展，则此值为 true
  - maxAttributes: gl.MAX_VERTEX_ATTRIBS 的值
  - maxCubemapSize: gl.MAX_CUBE_MAP_TEXTURE_SIZE 的值，着色器可使用的立方体贴图纹理的最大宽度\*高度
  - maxFragmentUniforms: gl.MAX_FRAGMENT_UNIFORM_VECTORS 的值，片元着色器可使用的全局变量(uniforms)数量
  - maxSamples: The value of gl.MAX_SAMPLES. Maximum number of samples in context of Multisample anti-aliasing (MSAA).
  - maxTextureSize: gl.MAX_TEXTURE_SIZE 的值，着色器可使用纹理的最大宽度\*高度
  - maxTextures: \*gl.MAX_TEXTURE_IMAGE_UNITS 的值，着色器可使用的纹理数量
  - maxVaryings: gl.MAX_VARYING_VECTORS 的值，着色器可使用矢量的数量
  - maxVertexTextures: gl.MAX_VERTEX_TEXTURE_IMAGE_UNITS 的值，顶点着色器可使用的纹理数量。
  - maxVertexUniforms: gl.MAX_VERTEX_UNIFORM_VECTORS 的值，顶点着色器可使用的全局变量(uniforms)数量
  - precision: 渲染器当前使用的着色器的精度
  - vertexTextures: 如果 .maxVertexTextures : Integer 大于 0，此值为 true (即可以使用顶点纹理)

- `.clippingPlanes` : Array
  用户自定义的剪裁平面，在世界空间中被指定为 THREE.Plane 对象。 这些平面全局使用。空间中与该平面点积为负的点将被切掉。 默认值是[]

- `.domElement` : DOMElement
  一个 canvas，渲染器在其上绘制输出。
  渲染器的构造函数会自动创建(如果没有传入 canvas 参数);你需要做的仅仅是像下面这样将它加页面里去:`document.body.appendChild( renderer.domElement );`
- `.extensions` : Object

  - get( extensionName : String ): 用于检查是否支持各种扩展，并返回一个对象，其中包含扩展的详细信息。 该方法检查以下扩展：
    WEBGL_depth_texture
    EXT_texture_filter_anisotropic
    WEBGL_compressed_texture_s3tc
    WEBGL_compressed_texture_pvrtc
    WEBGL_compressed_texture_etc1
    .outputColorSpace : string
    定义渲染器的输出编码。默认为 THREE.SRGBColorSpace

如果渲染目标已经使用 .setRenderTarget、之后将直接使用 renderTarget.texture.colorSpace

查看 texture constants 页面以获取其他格式细节

- `.info` : Object
  一个对象，包含有关图形板内存和渲染过程的一系列统计信息。这些信息可用于调试或仅仅满足下好奇心。该对象包含以下字段:

memory:
geometries
textures
render:
calls
triangles
points
lines
frame
programs
默认情况下，这些字段在每次渲染调用时都会重置，但是当每帧有多个渲染通道时（例如，使用后处理时），最好使用自定义模式重置。先将 autoReset 设置为 false.
renderer.info.autoReset = false;然后在单个帧时渲染完成后调用 reset().
renderer.info.reset();

- `.localClippingEnabled` : Boolean
  定义渲染器是否考虑对象级剪切平面。 默认为 false.

- `.useLegacyLights` : Boolean
  Whether to use the legacy lighting mode or not. 默认是 true。

- `.properties` : Object
  渲染器内部使用，以跟踪各种子对象属性。

- `.renderLists` : WebGLRenderLists
  在内部用于处理场景渲染对象的排序。

- `.shadowMap` : WebGLShadowMap
  如果使用，它包含阴影贴图的引用。

  - enabled: 如果设置开启，允许在场景中使用阴影贴图。默认是 false。
  - autoUpdate: 启用场景中的阴影自动更新。默认是 true
    如果不需要动态光照/阴影, 则可以在实例化渲染器时将之设为 false
  - needsUpdate: 当被设为 true, 场景中的阴影贴图会在下次 render 调用时刷新。默认是 false
    如果你已经禁用了阴影贴图的自动更新(shadowMap.autoUpdate = false), 那么想要在下一次渲染时更新阴影的话就需要将此值设为 true
  - type: 定义阴影贴图类型 (未过滤, 关闭部分过滤, 关闭部分双线性过滤), 可选值有:

THREE.BasicShadowMap
THREE.PCFShadowMap (默认)
THREE.PCFSoftShadowMap
THREE.VSMShadowMap
详见 Renderer constants

- `.sortObjects` : Boolean
  定义渲染器是否应对对象进行排序。默认是 true.

说明: 排序用于尝试正确渲染出具有一定透明度的对象。根据定义，排序可能不总是有用。根据应用的需求，可能需要关闭排序并使其他方法来处理透明度的渲染，例如， 手动确定每个对象的渲染顺序。

- `.state` : Object
  包含设置 WebGLRenderer.context 状态的各种属性的函数。

- `.toneMapping` : Constant
  默认是 NoToneMapping。查看 Renderer constants 以获取其它备选项

- `.toneMappingExposure` : Number
  色调映射的曝光级别。默认是 1

-`.xr` : WebXRManager
Provides access to the WebXR related interface of the renderer.

##### 方法

- `.render ( scene : Object3D, camera : Camera ) : undefined`
  用相机(camera)渲染一个场景(scene)或是其它类型的 object。
  渲染一般是在 canvas 上完成的，或者是 renderTarget(如果有指定)
  如果 forceClear 值是 true，那么颜色、深度及模板缓存将会在渲染之前清除，即使渲染器的 autoClear 属性值是 false
  即便 forceClear 设为 true, 也可以通过将 autoClearColor、autoClearStencil 或 autoClearDepth 属性的值设为 false 来阻止对应缓存被清除。

- `.setSize ( width : Integer, height : Integer, updateStyle : Boolean ) : undefined`
  将输出 canvas 的大小调整为(width, height)并考虑设备像素比，且将视口从(0, 0)开始调整到适合大小 将 updateStyle 设置为 false 以阻止对 canvas 的样式做任何改变。

- `.setViewport ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined`
  将视口大小设置为(x, y)到 (x + width, y + height).

- `.clear ( color : Boolean, depth : Boolean, stencil : Boolean ) : undefined`
  告诉渲染器清除颜色、深度或模板缓存. 此方法将颜色缓存初始化为当前颜色。参数们默认都是 true

- `.clearColor ( ) : undefined`
  清除颜色缓存。 相当于调用.clear( true, false, false )

- `.clearDepth ( ) : undefined`
  清除深度缓存。相当于调用.clear( false, true, false )

- `.clearStencil ( ) : undefined`
  清除模板缓存。相当于调用.clear( false, false, true )

- `.compile ( scene : Object3D, camera : Camera ) : undefined`
  使用相机编译场景中的所有材质。这对于在首次渲染之前预编译着色器很有用。

- `.copyFramebufferToTexture ( position : Vector2, texture : FramebufferTexture, level : Number ) : undefined`
  将当前 WebGLFramebuffer 中的像素复制到 2D 纹理中。可访问 WebGLRenderingContext.copyTexImage2D.

- `.copyTextureToTexture ( position : Vector2, srcTexture : Texture, dstTexture : Texture, level : Number ) : undefined`
  将纹理的所有像素复制到一个已有的从给定位置开始的纹理中。可访问 WebGLRenderingContext.texSubImage2D.

- `.dispose ( ) : undefined`
  处理当前的渲染环境

- `.forceContextLoss () : undefined`
  模拟 WebGL 环境的丢失。需要支持 WEBGL_lose_context 扩展才能用。

- `.forceContextRestore ( ) : undefined`
  模拟 WebGL 环境的恢复。需要支持 WEBGL_lose_context 扩展才能用。

- `.getClearAlpha () : Float`
  返回一个表示当前 alpha 值的 float，范围 0 到 1

- `.getClearColor ( target : Color ) : Color`
  返回一个表示当前颜色值的 THREE.Color 实例

- `.getContext () : WebGL2RenderingContext`
  返回当前 WebGL 环境

- `.getContextAttributes () : WebGLContextAttributes`
  返回一个对象，这个对象中存有在 WebGL 环境在创建的时候所设置的属性

- `.getActiveCubeFace () : Integer`
  Returns the current active cube face.

- `.getActiveMipmapLevel () : Integer`
  Returns the current active mipmap level.

- `.getRenderTarget () : RenderTarget`
  如果当前存在 RenderTarget，则返回该值；否则返回 null。

- `.getCurrentViewport () : RenderTarget`
  返回当前视口

- `.getDrawingBufferSize () : Object`
  返回一个包含渲染器绘图缓存宽度和高度(单位像素)的对象。

- `.getPixelRatio () : number`
  返回当前使用设备像素比

- `.getSize ( target : Vector2 ) : Vector2`
  返回包含渲染器输出 canvas 的宽度和高度(单位像素)的对象。

- `.initTexture ( texture : Texture ) : undefined`
  初始化给定的纹理。用于预加载纹理而不是等到第一次渲染（可能会由于解码和 GPU 上传的开销而导致明显的延迟）.

- `.resetGLState ( ) : undefined`
  将 GL 状态重置为默认值。WebGL 环境丢失时会内部调用

- `.readRenderTargetPixels ( renderTarget : WebGLRenderTarget, x : Float, y : Float, width : Float, height : Float, buffer : TypedArray, activeCubeFaceIndex : Integer ) : undefined`
  buffer - Uint8Array is the only destination type supported in all cases, other types are renderTarget and platform dependent. See WebGL spec for details.

将 renderTarget 中的像素数据读取到传入的缓冲区中。这是 WebGLRenderingContext.readPixels()的包装器
示例：interactive / cubes / gpu

For reading out a WebGLCubeRenderTarget use the optional parameter activeCubeFaceIndex to determine which face should be read.

- `.resetState () : undefined`
  可用于重置内部 WebGL 状态。此方法主要与跨多个 WebGL 库共享单个 WebGL 上下文的应用程序相关。

- `.setAnimationLoop ( callback : Function ) : undefined`
  callback — 每个可用帧都会调用的函数。 如果传入‘null’,所有正在进行的动画都会停止。

可用来代替 requestAnimationFrame 的内置函数. 对于 WebXR 项目，必须使用此函数。

- `.setClearAlpha ( alpha : Float ) : undefined`
  设置 alpha。合法参数是一个 0.0 到 1.0 之间的浮点数

- `.setClearColor ( color : Color, alpha : Float ) : undefined`
  设置颜色及其透明度

- `.setPixelRatio ( value : number ) : undefined`
  设置设备像素比。通常用于避免 HiDPI 设备上绘图模糊

- `.setRenderTarget ( renderTarget : WebGLRenderTarget, activeCubeFace : Integer, activeMipmapLevel : Integer ) : undefined`
  renderTarget -- 需要被激活的 renderTarget(可选)。若此参数为空，则将 canvas 设置成活跃 render target。
  activeCubeFace -- Specifies the active cube side (PX 0, NX 1, PY 2, NY 3, PZ 4, NZ 5) of WebGLCubeRenderTarget. When passing a WebGLArrayRenderTarget or WebGL3DRenderTarget this indicates the z layer to render in to (optional).
  activeMipmapLevel -- Specifies the active mipmap level (optional).

该方法设置活跃 rendertarget。

- `.setScissor ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined`
  将剪裁区域设为(x, y)到(x + width, y + height) Sets the scissor area from

- `.setScissorTest ( boolean : Boolean ) : undefined`
  启用或禁用剪裁检测. 若启用，则只有在所定义的裁剪区域内的像素才会受之后的渲染器影响。

#### WebGL1Renderer

自 r118 起，WebGLRenderer 会自动使用 WebGL 2 渲染上下文。当升级一个已存在的项目到 => r118 ， 应用程序可能会因为下列两种原因而损坏：

自定义着色器代码必须符合 GLSL 3.0。
WebGL 1 扩展检查到被更改
如果你不能够花时间来升级你的代码，但仍然想要使用最新版本，那你就可以使用 WebGL1Renderer。 这一版本的渲染器会强制使用 WebGL 1 渲染上下文。
构造函数
WebGL1Renderer( parameters : Object )
Creates a new WebGL1Renderer.

#### SVG 渲染器（SVGRenderer）

SVGRenderer 被用于使用 SVG 来渲染几何数据，所产生的矢量图形在以下几个方面十分有用：

动画标志（logo）或者图标（icon）
可交互的 2D 或 3D 图表或图形
交互式地图
复杂的或包含动画的用户界面
SVGRenderer 具有很多优势。它产生清晰并且锐利的图像输出，它和实际视口分辨率无关。
SVG 元素可以通过 CSS 来控制样式；并且由于它可以添加诸如标题或者描述文字之类的元数据（对于搜索引擎或者屏幕阅读器十分有用），因此它具有十分良好的可访问性。

然而，SVG 也有一些十分重要的限制：

没有高级的着色器
不支持纹理
不支持阴影

SVGRenderer 是一个附加组件，必须显式导入。 See Installation / Addons.

`import { SVGRenderer } from 'three/addons/renderers/SVGRenderer.js';`

#### CSS 3D 渲染器（CSS3DRenderer）

CSS3DRenderer 用于通过 CSS3 的 transform 属性， 将层级的 3D 变换应用到 DOM 元素上。 如果你希望不借助基于 canvas 的渲染来在你的网站上应用 3D 变换，那么这一渲染器十分有趣。 同时，它也可以将 DOM 元素与 WebGL 的内容相结合。

然而，这一渲染器也有一些十分重要的限制：

它不可能使用 three.js 中的材质系统。
同时也不可能使用几何体。
CSS3DRenderer 仅支持 100%的浏览器和显示缩放。
因此，CSS3DRenderer 仅仅关注普通的 DOM 元素，这些元素被包含到了特殊的对象中（CSS3DObject 或者 CSS3DSprite），然后被加入到场景图中。
进口
CSS3DRenderer 是一个附加组件，必须显式导入。 See Installation / Addons.

`import { CSS3DRenderer } from 'three/addons/renderers/CSS3DRenderer.js';`

### 场景

#### Scene

场景能够让你在什么地方、摆放什么东西来交给 three.js 来渲染，这是你放置物体、灯光和摄像机的地方。

##### 构造器

Scene()
创建一个新的场景对象。

##### 属性

.background : Object
若不为空，在渲染场景的时候将设置背景，且背景总是首先被渲染的。 可以设置一个用于的“clear”的 Color（颜色）、一个覆盖 canvas 的 Texture（纹理）， 或是 a cubemap as a CubeTexture or an equirectangular as a Texture。默认值为 null。

.backgroundBlurriness : Float
Sets the blurriness of the background. Only influences environment maps assigned to Scene.background. Valid input is a float between 0 and 1. Default is 0.

.environment : Texture
若该值不为 null，则该纹理贴图将会被设为场景中所有物理材质的环境贴图。 然而，该属性不能够覆盖已存在的、已分配给 MeshStandardMaterial.envMap 的贴图。默认为 null。

.fog : Fog
一个 fog 实例定义了影响场景中的每个物体的雾的类型。默认值为 null。

.isScene : Boolean

.overrideMaterial : Material
如果不为空，它将强制场景中的每个物体使用这里的材质来渲染。默认值为 null。

##### 方法

.toJSON : Object
meta -- 包含有元数据的对象，例如场景中的的纹理或图片。 将 scene 对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 雾（Fog）

这个类中的参数定义了线性雾。也就是说，雾的密度是随着距离线性增大的。

##### 构造器

Fog( color : Integer, near : Float, far : Float )
颜色参数传入 Color 构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是 CSS 风格的字符串。

##### 属性

.isFog : Boolean
Read-only flag to check if a given object is of type Fog.

.name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

.color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

.near : Float
开始应用雾的最小距离。距离小于活动摄像机“near”个单位的物体将不会被雾所影响。

默认值是 1。

.far : Float
结束计算、应用雾的最大距离，距离大于活动摄像机“far”个单位的物体将不会被雾所影响。

默认值是 1000。

##### 方法

.clone () : Fog
返回一个具有和当前雾参数相同的新的 Fog 实例。

.toJSON () : Object
以 JSON 格式返回 Fog 的数据。

#### 雾-指数（FogExp2）

该类所包含的参数定义了指数雾，它可以在相机附近提供清晰的视野，且距离相机越远，雾的浓度随着指数增长越快。

##### 构造器

FogExp2( color : Integer, density : Float )
颜色参数传入 Color 构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是 CSS 风格的字符串。

##### 属性

.isFogExp2 : Boolean
Read-only flag to check if a given object is of type FogExp2.

.name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

.color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

.density : Float
定义雾的密度将会增长多块。

默认值是 0.00025.

##### 方法

.clone () : FogExp2
返回一个具有和当前雾参数相同的新的 FogExp2 实例。

.toJSON () : Object
以 JSON 格式返回 FogExp2 的数据。

### 光源

#### Light

##### 构造器（Constructor）

`Light( color : Integer, intensity : Float )`
color - (可选参数) 16 进制表示光的颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。

创造一个新的光源。注意，这并不是直接调用的（而是使用派生类之一）。

##### 属性（Properties）

公共属性请查看基类 Object3D。

.color : Color
光源的颜色。如果构造的时候没有传递，默认会创建一个新的 Color 并设置为白色。

.intensity : Float
光照的强度，或者说能量。 在 physically correct 模式下, color 和强度 的乘积被解析为以坎德拉（candela）为单位的发光强度。 默认值 - 1.0
.isLight : Boolean
Read-only flag to check if a given object is of type Light.

##### Methods

公共方法请查看基类 Object3D。

.copy ( source : Light ) : this
从 source 复制 color, intensity 的值到当前光源对象中。

.toJSON ( meta : Object ) : Object
以 JSON 格式返回光数据。

meta -- 包含有元数据的对象，例如该对象的材质、纹理或图片。 将该 light 对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 环境光 AmbientLight

环境光会均匀的照亮场景中的所有物体。环境光不能用来投射阴影，因为它没有方向。

```js
const light = new THREE.AmbientLight(0x404040); // soft white light
scene.add(light);
```

构造函数
`AmbientLight( color : Integer, intensity : Float )`
color - （参数可选）颜色的 rgb 数值。缺省值为 0xffffff。
intensity - (参数可选)光照的强度。缺省值为 1。

创建一个环境光对象。

##### 属性|方法

> 公共属性请查看基类 Light。
> 公共方法请查看基类 Light。

.castShadow : Boolean
这个参数在对象构造的时候就被设置成了 undefined 。因为环境光不能投射阴影。

.isAmbientLight : Boolean
Read-only flag to check if a given object is of type AmbientLight.

#### 平行光（DirectionalLight）

平行光是沿着特定方向发射的光。这种光的表现像是无限远,从它发出的光线都是平行的。常常用平行光来模拟太阳光 的效果;

```js
const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
scene.add(directionalLight);
```

##### 构造函数

`DirectionalLight( color : Integer, intensity : Float )`
color - (可选参数) 16 进制表示光的颜色。 缺省值为 0xffffff (白色)。
intensity - (可选参数) 光照的强度。缺省值为 1。

##### 属性|方法

> 公共属性请查看基类 Light。
> 公共方法请查看基类 Light。

.castShadow : Boolean
如果设置为 true 该平行光会产生动态阴影。 警告: 这样做的代价比较高而且需要一直调整到阴影看起来正确. 查看 DirectionalLightShadow 了解详细信息。该属性默认为 false。

.isDirectionalLight : Boolean
Read-only flag to check if a given object is of type DirectionalLight.

.position : Vector3
假如这个值设置等于 Object3D.DEFAULT_UP (0, 1, 0),那么光线将会从上往下照射。

.shadow : DirectionalLightShadow
这个 DirectionalLightShadow 对象用来计算该平行光产生的阴影。

.target : Object3D
平行光的方向是从它的位置到目标位置。默认的目标位置为原点 (0,0,0)。
注意: 对于目标的位置，要将其更改为除缺省值之外的任何位置,它必须被添加到 scene 场景中去。`scene.add( light.target );`
这使得属性 target 中的 matrixWorld 会每帧自动更新。

它也可以设置 target 为场景中的其他对象(任意拥有 position 属性的对象), 示例如下:

```js
const targetObject = new THREE.Object3D();
scene.add(targetObject);

light.target = targetObject;
```

完成上述操作后，平行光现在就可以追踪到目标对像了。

##### 方法

`.copy ( source : DirectionalLight ) : this`
复制 source 的值到这个平行光源对象。

#### 点光源（PointLight）

从一个点向各个方向发射的光源。一个常见的例子是模拟一个灯泡发出的光。该光源可以投射阴影

```js
const light = new THREE.PointLight(0xff0000, 1, 100);
light.position.set(50, 50, 50);
scene.add(light);
```

##### 构造函数

`PointLight( color : Integer, intensity : Float, distance : Number, decay : Float )`
color - (可选参数)) 十六进制光照颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。
distance - 这个距离表示从光源到光照强度为 0 的位置。 当设置为 0 时，光永远不会消失(距离无穷大)。缺省值 0.
decay - 沿着光照距离的衰退量。缺省值 2。

##### 属性|方法

> 公共属性请查看基类 Light。
> 公共方法请查看基类 Light。

.castShadow : Boolean
如果设置为 true 该平行光会产生动态阴影。 警告: 这样做的代价比较高而且需要一直调整到阴影看起来正确. 查看 DirectionalLightShadow 了解详细信息。该属性默认为 false。
.distance : Float
如果非零，那么光强度将会从最大值当前灯光位置处按照距离线性衰减到 0。 缺省值为 0.0。

.power : Float
光功率
在 physically correct 模式中, 表示以"流明（光通量单位）"为单位的光功率。 缺省值 - 4Math.PI。

该值与 intensity 直接关联
`power = intensity * 4π`修改该值也会导致光强度的改变。

.shadow : PointLightShadow
PointLightShadow 用与计算此光照的阴影。

此对象的摄像机被设置为 fov 为 90 度，aspect 为 1， 近裁剪面 near 为 0，远裁剪面 far 为 500 的透视摄像机 PerspectiveCamera。

.decay : Float
The amount the light dims along the distance of the light. Default is 2.
In context of physically-correct rendering the default value should not be changed.

##### 方法

`.copy ( source : PointLight ) : this`
将所有属性的值从源 source 复制到此点光源对象。

#### 聚光灯（SpotLight）

光线从一个点沿一个方向射出，随着光线照射的变远，光线圆锥体的尺寸也逐渐增大。该光源可以投射阴影

```js
const spotLight = new THREE.SpotLight(0xffffff);
spotLight.position.set(100, 1000, 100);
spotLight.map = new THREE.TextureLoader().load(url);

spotLight.castShadow = true;

spotLight.shadow.mapSize.width = 1024;
spotLight.shadow.mapSize.height = 1024;

spotLight.shadow.camera.near = 500;
spotLight.shadow.camera.far = 4000;
spotLight.shadow.camera.fov = 30;

scene.add(spotLight);
```

##### 构造函数

`SpotLight( color : Integer, intensity : Float, distance : Float, angle : Radians, penumbra : Float, decay : Float )`
color - (可选参数) 十六进制光照颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。
distance - 从光源发出光的最大距离，其强度根据光源的距离线性衰减。
angle - 光线散射角度，最大为 Math.PI/2。
penumbra - 聚光锥的半影衰减百分比。在 0 和 1 之间的值。默认为 0。
decay - 沿着光照距离的衰减量。

##### 属性|方法

> 公共属性请查看基类 Light。
> 公共方法请查看基类 Light。

.angle : Float
从聚光灯的位置以弧度表示聚光灯的最大范围。应该不超过 Math.PI/2。默认值为 Math.PI/3。

.castShadow : Boolean
此属性设置为 true 聚光灯将投射阴影。警告: 这样做的代价比较高而且需要一直调整到阴影看起来正确。 查看 SpotLightShadow 了解详细信息。 默认值为 false

.decay : Float
The amount the light dims along the distance of the light. Default is 2.
In context of physically-correct rendering the default value should not be changed.

.distance : Float
如果非零，那么光强度将会从最大值当前灯光位置处按照距离线性衰减到 0。 缺省值为 0.0。

.isSpotLight : Boolean
Read-only flag to check if a given object is of type SpotLight.

.penumbra : Float
聚光锥的半影衰减百分比。在 0 和 1 之间的值。 默认值 — 0.0。

.position : Vector3
假如这个值设置等于 Object3D.DEFAULT_UP (0, 1, 0)，那么光线将会从上往下照射。

.power : Float
光功率
在 physically correct 模式中， 表示以"流明（光通量单位）"为单位的光功率。 缺省值 - 4Math.PI。

该值与 intensity 直接关联
power = intensity \* 4π 修改该值也会导致光强度的改变。

.shadow : SpotLightShadow
SpotLightShadow 用与计算此光照的阴影。

.target : Object3D
聚光灯的方向是从它的位置到目标位置.默认的目标位置为原点 (0,0,0)。
注意: 对于目标的位置，要将其更改为除缺省值之外的任何位置，它必须被添加到 scene 场景中去。
scene.add( light.target );这使得属性 target 中的 matrixWorld 会每帧自动更新。

它也可以设置 target 为场景中的其他对象(任意拥有 position 属性的对象), 示例如下:
const targetObject = new THREE.Object3D();
scene.add(targetObject);

light.target = targetObject;完成上述操作后，聚光灯现在就可以追踪到目标对像了。

.map : Texture
A Texture used to modulate the color of the light. The spot light color is mixed with the RGB value of this texture, with a ratio corresponding to its alpha value. The cookie-like masking effect is reproduced using pixel values (0, 0, 0, 1-cookie_value). Warning: map : SpotLight is disabled if castShadow : SpotLight is false.

##### 方法（Methods）

公共方法请查看基类 Light。

.copy ( source : SpotLight ) : this
将所有属性的值从源 source 复制到此聚光灯光源对象。

#### 平面光光源（RectAreaLight）

平面光光源从一个矩形平面上均匀地发射光线。这种光源可以用来模拟像明亮的窗户或者条状灯光光源。

注意事项:

不支持阴影。
只支持 MeshStandardMaterial 和 MeshPhysicalMaterial 两种材质。
你必须在你的场景中加入 RectAreaLightUniformsLib ，并调用 init()。

### 三维坐标系

AxesHelper 的 xyz 轴
three.js 坐标轴颜色红 R、绿 G、蓝 B 分别对应坐标系的 x、y、z 轴，对于 three.js 的 3D 坐标系默认 y 轴朝上。

```js
// AxesHelper：辅助观察的坐标系
const axesHelper = new THREE.AxesHelper(150);
scene.add(axesHelper);
```

## 坐标系

### 屏幕坐标,空间坐标

ThreeJS 是使用了 canvas 画布绘制图形的，因此屏幕坐标系就是 canvas 中的坐标系，也就是左上角是坐标原点

Mesh 的坐标是用一个 Vector3 来表示的，Vector3 中包含了 x、y、z 坐标。

空间坐标系是三维的，其原点默认在屏幕中心，且 x y z 的范围是 [-1,1]，通过 Vector3 对象的方法 project，方法的参数是相机对象，语句 worldVector.project(camera);返回的结果是世界坐标 worldVector 在 camera 相机对象矩阵变化下对应的标准设备坐标， 标准设备坐标 xyz 的范围是[-1,1]。

ThreeJS 中，画布一般是全屏的，因此画布的宽高 w,h 就是：window.innerWidth 和 window.innerHeight，所以 Three 的空间坐标系中点 (cx, cy)在屏幕坐标系中就是：(w / 2,h / 2)。

假设 canvas 中有一点 (x,y)，这个点在空间坐标系中为 (x1,y1)，那么这个转换公式是：

```js
x1=(x/w)∗2−1
y1=−(y/h)∗2+1

//推导如下
//#####################
cx=w/2
cy=h/2
//屏幕坐标系中的点(x,y),空间坐标中的点（x1,y1）
x1=x-cx
y2=cy-y
//标准化[-1,1]之间
x1/cx=(x-cx)/cx=x/cx-1=x/w/2-1=2x/w-1
y1/cy=(cy-y)/cy=1-y/cy=1-2y/h
```

```js
let worldVetor = boxMesh.position.cone();
//世界坐标系转标准设备坐标
//提取相机参数的视图矩阵、投影矩阵对世界坐标进行转换
let standard = worldVetor.project(camera);
//
```

## 全屏

```js
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

```js
window.addEventListener('resize',()=>{
  //更新摄像头
  camera.aspect=window.innerWidth/window.innerHeight
   // 渲染器执行render方法的时候会读取相机对象的投影矩阵属性projectionMatrix
    // 但是不会每渲染一帧，就通过相机的属性计算投影矩阵(节约计算资源)
  //更新摄像机投影矩阵
  camera.updateProjectionMatrix();
  //更新渲染器
  renderer.setSize(window.innerWidth,window.innerHeight)
  //设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio)
  }
```
