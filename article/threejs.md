---
title: Three.js
date: 2022-10-01 10:12:33
categories: 
- web
---


## 开始

### 创建一个场景

场景、相机和渲染器

```js
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

如果你希望保持你的应用程序的尺寸，但是以较低的分辨率来渲染，你可以在调用setSize时，将updateStyle（第三个参数）设为false。例如，假设你的`<canvas>` 标签现在已经具有了100%的宽和高，调用setSize(window.innerWidth/2, window.innerHeight/2, false)将使得你的应用程序以一半的分辨率来进行渲染。

### 创建一个立方体

```js
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

camera.position.z = 5;
```

创建一个立方体，我们需要一个BoxGeometry（立方体）对象. 这个对象包含了一个立方体中所有的顶点（vertices）和面（faces）。接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。第三步，我们需要一个Mesh（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

### 渲染场景

现在，如果将之前写好的代码复制到HTML文件中，你不会在页面中看到任何东西。这是因为我们还没有对它进行真正的渲染。为此，我们需要使用一个被叫做“渲染循环”（render loop）或者“动画循环”（animate loop）的东西。

```js
function animate() {
 requestAnimationFrame( animate );
 renderer.render( scene, camera );
}
animate();
```

在这里我们创建了一个使渲染器能够在每次屏幕刷新时对场景进行绘制的循环（在大多数屏幕上，刷新率一般是60次/秒）。如果你是一个浏览器游戏开发的新手，你或许会说“为什么我们不直接用setInterval来实现刷新的功能呢？”当然啦，我们的确可以用setInterval，但是，requestAnimationFrame有很多的优点。最重要的一点或许就是当用户切换到其它的标签页时，它会暂停，因此不会浪费用户宝贵的处理器资源，也不会损耗电池的使用寿命。

## 进阶

### 相机

#### Camera

##### 构造函数

`Camera()`
创建一个新的Camera（摄像机）。注意：这个类并不是被直接调用的；你所想要的或许是一个 PerspectiveCamera（透视摄像机）或者 OrthographicCamera（正交摄像机）。

##### 属性

共有属性请参见其基类Object3D

.isCamera : Boolean
Read-only flag to check if a given object is of type Camera.

.layers : Layers
摄像机是一个layers的成员. 这是一个从Object3D继承而来的属性。

当摄像机的视点被渲染的时候，物体必须和当前被看到的摄像机共享至少一个层。

.matrixWorldInverse : Matrix4
这是matrixWorld矩阵的逆矩阵。 MatrixWorld包含了相机的世界变换矩阵。

.projectionMatrix : Matrix4
这是投影变换矩阵。

.projectionMatrixInverse : Matrix4
这是投影变换矩阵的逆矩阵。

##### 方法

共有方法请参见其基类Object3D。

.clone ( ) : Camera
返回一个具有和当前相机的属性一样的新的相机。

.copy ( source : Camera, recursive : Boolean ) : this
将源摄像机的属性复制到新摄像机中。

.getWorldDirection ( target : Vector3 ) : Vector3
target — 调用该函数的结果将复制给该Vector3对象。

返回一个能够表示当前摄像机所正视的世界空间方向的Vector3对象。 （注意：摄像机俯视时，其Z轴坐标为负。）

#### 透视相机（PerspectiveCamera）

这一摄像机使用perspective projection（透视投影）来进行投影。

这一投影模式被用来模拟人眼所看到的景象，它是3D场景的渲染中使用得最普遍的投影模式。
透视投影相机的四个参数fov, aspect, near, far构成一个四棱台3D空间，被称为视锥体，只有视锥体之内的物体，才会渲染出来，视锥体范围之外的物体不会显示在Canvas画布上。
透视投影相机的投影规律是远小近大，通过相机观察阵列立方体大小变化，可以看到距离相机越远，立方体的渲染视觉效果越小。增加相机视角fov，视锥体范围更大，意味着可以看到渲染范围更大，远小近大的视觉效果更明显。

```js
const camera = new THREE.PerspectiveCamera( 45, width / height, 1, 1000 );
scene.add( camera );
```

##### 构造器

`PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )`
fov — 摄像机视锥体垂直视野角度
aspect — 摄像机视锥体长宽比,一般设置为Canvas画布宽高比width / height
near — 摄像机视锥体近端面
far — 摄像机视锥体远端面

##### 属性
>
>共有属性请参见其基类 Camera 。

请注意，在大多数属性发生改变之后，你将需要调用.updateProjectionMatrix来使得这些改变生效。

.aspect : Float
摄像机视锥体的长宽比，通常是使用画布的宽/画布的高。默认值是1（正方形画布）。

.far : Float
摄像机的远端面，默认值是2000。

该值必须大于near plane（摄像机视锥体近端面）的值。

.filmGauge : Float
胶片尺寸，其默认值为35（毫米）。 这个参数不会影响摄像机的投影矩阵，除非.filmOffset被设置为了一个非零的值。

.filmOffset : Float
水平偏离中心偏移量，和.filmGauge单位相同。默认值为0。

.focus : Float
用于立体视觉和景深效果的物体的距离。 这个参数不会影响摄像机的投影矩阵，除非使用了StereoCamera。 默认值是10。

.fov : Float
摄像机视锥体垂直视野角度，从视图的底部到顶部，以角度来表示。默认值是50。

.isPerspectiveCamera : Boolean
Read-only flag to check if a given object is of type PerspectiveCamera.

.near : Float
摄像机的近端面，默认值是0.1。

其有效值范围是0到当前摄像机far plane（远端面）的值之间。 请注意，和OrthographicCamera不同，0对于PerspectiveCamera的近端面来说不是一个有效值。

.view : Object
Frustum window specification or null. 这个值使用.setViewOffset方法来进行设置，使用.clearViewOffset方法来进行清除。

.zoom : number
获取或者设置摄像机的缩放倍数，其默认值为1。

##### 方法

>共有方法请参见其基类Camera。

.clearViewOffset () : undefined
清除任何由.setViewOffset设置的偏移量。

.getEffectiveFOV () : Float
结合.zoom（缩放倍数），以角度返回当前垂直视野角度。

.getFilmHeight () : Float
返回当前胶片上图像的高，如果.aspect小于或等于1（肖像格式、纵向构图），则结果等于.filmGauge。

.getFilmWidth () : Float
返回当前胶片上图像的宽，如果.aspect大于或等于1（景观格式、横向构图），则结果等于.filmGauge。

.getFocalLength () : Float
返回当前.fov（视野角度）相对于.filmGauge（胶片尺寸）的焦距。

.setFocalLength ( focalLength : Float ) : undefined
通过相对于当前.filmGauge的焦距，设置FOV。

默认情况下，焦距是为35mm（全画幅）摄像机而指定的。

.setViewOffset ( fullWidth : Float, fullHeight : Float, x : Float, y : Float, width : Float, height : Float ) : undefined
fullWidth — 多视图的全宽设置
fullHeight — 多视图的全高设置
x — 副摄像机的水平偏移
y — 副摄像机的垂直偏移
width — 副摄像机的宽度
height — 副摄像机的高度

在较大的viewing frustum（视锥体）中设置偏移量，对于多窗口或者多显示器的设置是很有用的。

例如，如果你有一个3x2的显示器阵列，每个显示器分辨率都是1920x1080，且这些显示器排列成像这样的网格：
+---+---+---+
| A | B | C |
+---+---+---+
| D | E | F |
+---+---+---+
  
那对于每个显示器，你可以这样来设置、调用：
const w = 1920;
const h = 1080;
const fullWidth = w *3;
const fullHeight = h* 2;

// A
camera.setViewOffset( fullWidth, fullHeight, w *0, h* 0, w, h );
// B
camera.setViewOffset( fullWidth, fullHeight, w *1, h* 0, w, h );
// C
camera.setViewOffset( fullWidth, fullHeight, w *2, h* 0, w, h );
// D
camera.setViewOffset( fullWidth, fullHeight, w *0, h* 1, w, h );
// E
camera.setViewOffset( fullWidth, fullHeight, w *1, h* 1, w, h );
// F
camera.setViewOffset( fullWidth, fullHeight, w *2, h* 1, w, h );请注意，显示器的不必具有相同的大小，或者不必在网格中。
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
camera.lookAt(0, 10, 0);  //y轴上位置10
camera.lookAt(mesh.position);//指向mesh对应的位置
```

#### 正交相机（OrthographicCamera）

这一摄像机使用orthographic projection（正交投影）来进行投影。

在这种投影模式下，无论物体距离相机距离远或者近，在最终渲染的图片中物体的大小都保持不变。

这对于渲染2D场景或者UI元素是非常有用的。

```js
const camera = new THREE.OrthographicCamera( width / - 2, width / 2, height / 2, height / - 2, 1, 1000 );
scene.add( camera );
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

共有属性请参见其基类Camera。
请注意，在大多数属性发生改变之后，你将需要调用.updateProjectionMatrix来使得这些改变生效。

.bottom : Float
摄像机视锥体下侧面。

.far : Float
摄像机视锥体远端面，其默认值为2000。

该值必须大于near plane（摄像机视锥体近端面）的值。

.isOrthographicCamera : Boolean
Read-only flag to check if a given object is of type OrthographicCamera.

.left : Float
摄像机视锥体左侧面。

.near : Float
摄像机视锥体近端面。其默认值为0.1.

其值的有效范围介于0和far（摄像机视锥体远端面）之间。
请注意，和PerspectiveCamera不同，0对于OrthographicCamera的近端面来说是一个有效值。

.right : Float
摄像机视锥体右侧面。

.top : Float
摄像机视锥体上侧面。

.view : Object
这个值是由setViewOffset来设置的，其默认值为null。

.zoom : number
获取或者设置摄像机的缩放倍数，其默认值为1。

##### 方法

共有方法请参见其基类Camera。

.setViewOffset ( fullWidth : Float, fullHeight : Float, x : Float, y : Float, width : Float, height : Float ) : undefined
fullWidth — 多视图的全宽设置
fullHeight — 多视图的全高设置
x — 副摄像机的水平偏移
y — 副摄像机的垂直偏移
width — 副摄像机的宽度
height — 副摄像机的高度

在较大的viewing frustum（视锥体）中设置偏移量，对于多窗口或者多显示器的设置是很有用的。 对于如何使用它，请查看PerspectiveCamera中的示例。

.clearViewOffset () : undefined
清除任何由.setViewOffset设置的偏移量。

.updateProjectionMatrix () : undefined
更新摄像机投影矩阵。在任何参数被改变以后必须被调用。

.toJSON (meta : Object) : Object
meta -- 包含有元数据的对象，例如对象后代中的纹理或图像
将摄像机转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 摄像机阵列（ArrayCamera）

ArrayCamera 用于更加高效地使用一组已经预定义的摄像机来渲染一个场景。这将能够更好地提升VR场景的渲染性能。
一个 ArrayCamera 的实例中总是包含着一组子摄像机，应当为每一个子摄像机定义viewport（视口）这个属性，这一属性决定了由该子摄像机所渲染的视口区域的大小。

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

创建6个渲染到WebGLCubeRenderTarget的摄像机
构造器
CubeCamera( near : Number, far : Number, renderTarget : WebGLCubeRenderTarget )
near -- 近剪切面的距离
far -- 远剪切面的距离
renderTarget -- The destination cube render target.

构造一个包含6个PerspectiveCameras（透视摄像机）的立方摄像机， 并将其拍摄的场景渲染到一个WebGLCubeRenderTarget上。

#### 立体相机（StereoCamera）

双透视摄像机（立体相机）常被用于创建3D Anaglyph（3D立体影像） 或者Parallax Barrier（视差屏障）。

### 几何体

#### 缓冲属性BufferAttribute

这个类用于存储与BufferGeometry相关联的 attribute（例如顶点位置向量，面片索引，法向量，颜色值，UV坐标以及任何自定义 attribute ）。 利用 BufferAttribute，可以更高效的向GPU传递数据。
>在 BufferAttribute 中，数据被存储为任意长度的矢量（通过itemSize进行定义），下列函数如无特别说明， 函数参数中的index会自动乘以矢量长度进行计算。 当想要处理类似向量的数据时， 可以使用在Vector2，Vector3， Vector4以及Color这些类中的.fromBufferAttribute( attribute, index ) 方法来更为便捷地处理。

##### 构造函数

`BufferAttribute( array : TypedArray, itemSize : Integer, normalized : Boolean )`
array -- 必须是 TypedArray. 类型，用于实例化缓存。
该队列应该包含：`itemSize * numVertices`个元素，numVertices 是 BufferGeometry中的顶点数目

itemSize -- 队列中与顶点相关的数据值的大小。举例，如果 attribute 存储的是三元组（例如顶点空间坐标、法向量或颜色值）则itemSize的值应该是3。

normalized -- (可选) 指明缓存中的数据如何与GLSL代码中的数据对应。例如，如果array是 UInt16Array类型，且normalized的值是 true，则队列中的值将会从 0 - +65535 映射为 GLSL 中的 0.0f - +1.0f。 如果array是 Int16Array (有符号)，则值将会从 -32768 - +32767 映射为 -1.0f - +1.0f。若 normalized 的值为 false，则数据映射不会归一化，而会直接映射为 float 值，例如，32767 将会映射为 32767.0f.

##### 属性

- `.array` : TypedArray
在 array 中保存着缓存中的数据。

- `.count` : Integer
保存 array 除以 itemSize 之后的大小。若缓存存储三元组（例如顶点位置、法向量、颜色值），则该值应等于队列中三元组的个数。

- `.isBufferAttribute` : Boolean
用于判断对象是否为BufferAttribute类型的只读标记.

`.itemSize` : Integer
保存在 array 中矢量的长度。

- `.name` : String
该 attribute 实例的别名，默认值为空字符串。

- `.needsUpdate` : Boolean
该标志位指明当前 attribute 已经被修改过，且需要再次送入 GPU 处理。当开发者改变了该队列的值，则标志位需要设置为 true。

将标志位设为 true 同样会增加 version 的值。

- `.normalized` : Boolean
指明缓存中数据在转化为GLSL着色器代码中数据时是否需要被归一化。详见构造函数中的说明。

- `.onUploadCallback` : Function
attribute 数据传输到GPU后的回调函数。

- `.updateRange` : Object
对象包含如下成员:
offset: 默认值为 0。 指明更新的起始位置。
count: 默认值为 -1，表示不指定更新范围。

该值只可以被用于更新某些矢量数据（例如，颜色相关数据）。

- `.usage` : Usage
为输入的数据定义最优的预估使用方式。等同于在WebGLRenderingContext.bufferData() 中的usage参数。默认为StaticDrawUsage。在usage constants中查看可用值。

- `.version` : Integer
版本号，当 needsUpdate 被设置为 true 时，该值会自增。

##### 方法

- `.applyMatrix3 ( m : Matrix3 ) : this`
将矩阵m应用此BufferAttribute中的每一个Vector3元素中。

- `.applyMatrix4 ( m : Matrix4 ) : this`
将矩阵m应用到此BufferAttribute的每一个Vector3元素中

- `.applyNormalMatrix ( m : Matrix3 ) : this`
将正规矩阵m应用到此BufferAttribute的每一个Vector3元素中

- `.transformDirection ( m : Matrix4 ) : this`
将矩阵m应用到此BufferAttribute的每一个Vector3元素中，并将所有元素解释为方向向量。

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

#### 缓冲几何体BufferGeometry

是面片、线或点几何体的有效表述。包括顶点位置，面片索引、法相量、颜色值、UV 坐标和自定义缓存属性值。使用 BufferGeometry 可以有效减少向 GPU 传输上述数据所需的开销。
BufferGeometry是一个没有任何形状的空几何体，你可以通过BufferGeometry自定义任何几何形状

```js
const geometry = new THREE.BufferGeometry();
// 创建一个简单的矩形. 在这里我们左上和右下顶点被复制了两次。
// 因为在两个三角面片里，这两个顶点都需要被用到。
//通过javascript类型化数组 Float32Array创建一组xyz坐标数据用来表示几何体的顶点坐标
const vertices = new Float32Array( [
 -1.0, -1.0,  1.0,
  1.0, -1.0,  1.0,
  1.0,  1.0,  1.0,

  1.0,  1.0,  1.0,
 -1.0,  1.0,  1.0,
 -1.0, -1.0,  1.0
] );
// 创建属性缓冲区对象
// itemSize = 3 因为每个顶点都是一个三元组。
//3个为一组，表示一个顶点的xyz坐标
geometry.setAttribute( 'position', new THREE.BufferAttribute( vertices, 3 ) );//设置几何体顶点
const material = new THREE.MeshBasicMaterial( { color: 0xff0000 } );
const mesh = new THREE.Mesh( geometry, material );
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
用于判断对象是否为BufferGeometry的只读标记.

`.morphAttributes` : Object
存储 BufferAttribute 的 Hashmap，存储了几何体 morph targets 的细节信息。
注意：当这个geometry渲染之后，morph attribute 数据无法更改。你需要调用.dispose()，并重新创建一个新的BufferGeometry实例。

`.morphTargetsRelative` : Boolean
用于控制morph target的行为，如果设置为 true，morph target数据作为相对的偏移量，而非绝对的位置/法向。 默认为false。

`.name` : String
当前 bufferGeometry 实例的可选别名。默认值是空字符串。

`.userData` : Object
存储 BufferGeometry 的自定义数据的对象。为保持对象在克隆时完整，该对象不应该包括任何函数的引用。

`.uuid` : String
当前对象实例的 UUID，该值会自动被分配，且不应被修改。

##### 方法
>
>EventDispatcher 在该类上可用的所有方法。

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
计算并向此geometry中添加tangent attribute。
只支持索引化的几何体对象，并且必须拥有position(位置)，normal(法向)和 uv attributes。如果使用了切线空间法向贴图，最好使用BufferGeometryUtils.computeMikkTSpaceTangents中的MikkTSpace算法。

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
检查是否存在有指定名称的attribute，如果有返回true。

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
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );
```

##### 构造器

`BoxGeometry(width : Float, height : Float, depth : Float, widthSegments : Integer, heightSegments : Integer, depthSegments : Integer)`
width — X轴上面的宽度，默认值为1。
height — Y轴上面的高度，默认值为1。
depth — Z轴上面的深度，默认值为1。
widthSegments — （可选）宽度的分段数，默认值是1。
heightSegments — （可选）高度的分段数，默认值是1。
depthSegments — （可选）深度的分段数，默认值是1。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆形（CircleGeometry）

```js
const geometry = new THREE.CircleGeometry( 5, 32 );
const material = new THREE.MeshBasicMaterial( { color: 0xffff00 } );
const circle = new THREE.Mesh( geometry, material );
scene.add( circle );
```

##### 构造器

`CircleGeometry(radius : Float, segments : Integer, thetaStart : Float, thetaLength : Float)`
radius — 圆形的半径，默认值为1
segments — 分段（三角面）的数量，最小值为3，默认值为32。
thetaStart — 第一个分段的起始角度，默认为0。（three o'clock position）
thetaLength — 圆形扇区的中心角，通常被称为“θ”（西塔）。默认值是2*Pi，这使其成为一个完整的圆。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 平面（PlaneGeometry）

```js
const geometry = new THREE.PlaneGeometry( 1, 1 );
const material = new THREE.MeshBasicMaterial( {color: 0xffff00, side: THREE.DoubleSide} );
const plane = new THREE.Mesh( geometry, material );
scene.add( plane );
```

##### 构造器

`PlaneGeometry(width : Float, height : Float, widthSegments : Integer, heightSegments : Integer)`
width — 平面沿着X轴的宽度。默认值是1。
height — 平面沿着Y轴的高度。默认值是1。
widthSegments — （可选）平面的宽度分段数，默认值是1。
heightSegments — （可选）平面的高度分段数，默认值是1。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆柱（CylinderGeometry）

```js
const geometry = new THREE.CylinderGeometry( 5, 5, 20, 32 );
const material = new THREE.MeshBasicMaterial( {color: 0xffff00} );
const cylinder = new THREE.Mesh( geometry, material );
scene.add( cylinder );
```

##### 构造器

`CylinderGeometry(radiusTop : Float, radiusBottom : Float, height : Float, radialSegments : Integer, heightSegments : Integer, openEnded : Boolean, thetaStart : Float, thetaLength : Float)`
radiusTop — 圆柱的顶部半径，默认值是1。
radiusBottom — 圆柱的底部半径，默认值是1。
height — 圆柱的高度，默认值是1。
radialSegments — 圆柱侧面周围的分段数，默认为32。
heightSegments — 圆柱侧面沿着其高度的分段数，默认值为1。
openEnded — 一个Boolean值，指明该圆锥的底面是开放的还是封顶的。默认值为false，即其底面默认是封顶的。
thetaStart — 第一个分段的起始角度，默认为0。（three o'clock position）
thetaLength — 圆柱底面圆扇区的中心角，通常被称为“θ”（西塔）。默认值是2*Pi，这使其成为一个完整的圆柱。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 挤压（ExtrudeGeometry）

```js
const length = 12, width = 8;

const shape = new THREE.Shape();
shape.moveTo( 0,0 );
shape.lineTo( 0, width );
shape.lineTo( length, width );
shape.lineTo( length, 0 );
shape.lineTo( 0, 0 );

const extrudeSettings = {
 steps: 2,
 depth: 16,
 bevelEnabled: true,
 bevelThickness: 1,
 bevelSize: 1,
 bevelOffset: 0,
 bevelSegments: 1
};

const geometry = new THREE.ExtrudeGeometry( shape, extrudeSettings );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const mesh = new THREE.Mesh( geometry, material ) ;
scene.add( mesh );
```

##### 构造器

`ExtrudeGeometry(shapes : Array, options : Object)`
shapes — 形状或者一个包含形状的数组。
options — 一个包含有下列参数的对象：

curveSegments — int，曲线上点的数量，默认值是12。
steps — int，用于沿着挤出样条的深度细分的点的数量，默认值为1。
depth — float，挤出的形状的深度，默认值为1。
bevelEnabled — bool，对挤出的形状应用是否斜角，默认值为true。
bevelThickness — float，设置原始形状上斜角的厚度。默认值为0.2。
bevelSize — float。斜角与原始形状轮廓之间的延伸距离，默认值为bevelThickness-0.1。
bevelOffset — float. Distance from the shape outline that the bevel starts. Default is 0.
bevelSegments — int。斜角的分段层数，默认值为3。
extrudePath — THREE.Curve对象。一条沿着被挤出形状的三维样条线。Bevels not supported for path extrusion.
UVGenerator — Object。提供了UV生成器函数的对象。
该对象将一个二维形状挤出为一个三维几何体。

当使用这个几何体创建Mesh的时候，如果你希望分别对它的表面和它挤出的侧面使用单独的材质，你可以使用一个材质数组。 第一个材质将用于其表面；第二个材质则将用于其挤压出的侧面。##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 形状（ShapeGeometry）

```js
const x = 0, y = 0;

const heartShape = new THREE.Shape();

heartShape.moveTo( x + 5, y + 5 );
heartShape.bezierCurveTo( x + 5, y + 5, x + 4, y, x, y );
heartShape.bezierCurveTo( x - 6, y, x - 6, y + 7,x - 6, y + 7 );
heartShape.bezierCurveTo( x - 6, y + 11, x - 3, y + 15.4, x + 5, y + 19 );
heartShape.bezierCurveTo( x + 12, y + 15.4, x + 16, y + 11, x + 16, y + 7 );
heartShape.bezierCurveTo( x + 16, y + 7, x + 16, y, x + 10, y );
heartShape.bezierCurveTo( x + 7, y, x + 5, y + 5, x + 5, y + 5 );

const geometry = new THREE.ShapeGeometry( heartShape );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const mesh = new THREE.Mesh( geometry, material ) ;
scene.add( mesh );
```

##### 构造器

`ShapeGeometry(shapes : Array, curveSegments : Integer)`
shapes — 一个单独的shape，或者一个包含形状的Array。Default is a single triangle shape.
curveSegments - Integer - 每一个形状的分段数，默认值为12。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆环（RingGeometry）

```js
const geometry = new THREE.RingGeometry( 1, 5, 32 );
const material = new THREE.MeshBasicMaterial( { color: 0xffff00, side: THREE.DoubleSide } );
const mesh = new THREE.Mesh( geometry, material );
scene.add( mesh );
```

##### 构造器

`RingGeometry(innerRadius : Float, outerRadius : Float, thetaSegments : Integer, phiSegments : Integer, thetaStart : Float, thetaLength : Float)`
innerRadius — 内部半径，默认值为0.5。
outerRadius — 外部半径，默认值为1。
thetaSegments — 圆环的分段数。这个值越大，圆环就越圆。最小值为3，默认值为32。
phiSegments — 最小值为1，默认值为8。
thetaStart — 起始角度，默认值为0。
thetaLength — 圆心角，默认值为Math.PI * 2。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆锥（ConeGeometry）

```js
const geometry = new THREE.ConeGeometry( 5, 20, 32 );
const material = new THREE.MeshBasicMaterial( {color: 0xffff00} );
const cone = new THREE.Mesh( geometry, material );
scene.add( cone );
```

##### 构造器

`ConeGeometry(radius : Float, height : Float, radialSegments : Integer, heightSegments : Integer, openEnded : Boolean, thetaStart : Float, thetaLength : Float)`
radius — 圆锥底部的半径，默认值为1。
height — 圆锥的高度，默认值为1。
radialSegments — 圆锥侧面周围的分段数，默认为32。
heightSegments — 圆锥侧面沿着其高度的分段数，默认值为1。
openEnded — 一个Boolean值，指明该圆锥的底面是开放的还是封顶的。默认值为false，即其底面默认是封顶的。
thetaStart — 第一个分段的起始角度，默认为0。（three o'clock position）
thetaLength — 圆锥底面圆扇区的中心角，通常被称为“θ”（西塔）。默认值是2*Pi，这使其成为一个完整的圆锥。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 球（SphereGeometry）

```js
const geometry = new THREE.SphereGeometry( 15, 32, 16 );
const material = new THREE.MeshBasicMaterial( { color: 0xffff00 } );
const sphere = new THREE.Mesh( geometry, material );
scene.add( sphere );
```

##### 构造器

`SphereGeometry(radius : Float, widthSegments : Integer, heightSegments : Integer, phiStart : Float, phiLength : Float, thetaStart : Float, thetaLength : Float)`
radius — 球体半径，默认为1。
widthSegments — 水平分段数（沿着经线分段），最小值为3，默认值为32。
heightSegments — 垂直分段数（沿着纬线分段），最小值为2，默认值为16。
phiStart — 指定水平（经线）起始角度，默认值为0。。
phiLength — 指定水平（经线）扫描角度的大小，默认值为 Math.PI * 2。
thetaStart — 指定垂直（纬线）起始角度，默认值为0。
thetaLength — 指定垂直（纬线）扫描角度大小，默认值为 Math.PI。
该几何体是通过扫描并计算围绕着Y轴（水平扫描）和X轴（垂直扫描）的顶点来创建的。 因此，不完整的球体（类似球形切片）可以通过为phiStart，phiLength，thetaStart和thetaLength设置不同的值来创建， 以定义我们开始（或结束）计算这些顶点的起点（或终点）。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 圆环（TorusGeometry）

```js
const geometry = new THREE.TorusGeometry( 10, 3, 16, 100 );
const material = new THREE.MeshBasicMaterial( { color: 0xffff00 } );
const torus = new THREE.Mesh( geometry, material );
scene.add( torus );
```

##### 构造器

`TorusGeometry(radius : Float, tube : Float, radialSegments : Integer, tubularSegments : Integer, arc : Float)`
radius - 环面的半径，从环面的中心到管道横截面的中心。默认值是1。
tube — 管道的半径，默认值为0.4。
radialSegments — 管道横截面的分段数，默认值为12。
tubularSegments — 管道的分段数，默认值为48。
arc — 圆环的圆心角（单位是弧度），默认值为Math.PI * 2。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 管道（TubeGeometry）

```js
class CustomSinCurve extends THREE.Curve {

 constructor( scale = 1 ) {

  super();

  this.scale = scale;

 }

 getPoint( t, optionalTarget = new THREE.Vector3() ) {

  const tx = t * 3 - 1.5;
  const ty = Math.sin( 2 * Math.PI * t );
  const tz = 0;

  return optionalTarget.set( tx, ty, tz ).multiplyScalar( this.scale );

 }

}

const path = new CustomSinCurve( 10 );
const geometry = new THREE.TubeGeometry( path, 20, 2, 8, false );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const mesh = new THREE.Mesh( geometry, material );
scene.add( mesh );
```

##### 构造器

`TubeGeometry(path : Curve, tubularSegments : Integer, radius : Float, radialSegments : Integer, closed : Boolean)`
path — Curve - 一个由基类Curve继承而来的3D路径。 Default is a quadratic bezier curve.
tubularSegments — Integer - 组成这一管道的分段数，默认值为64。
radius — Float - 管道的半径，默认值为1。
radialSegments — Integer - 管道横截面的分段数目，默认值为8。
closed — Boolean 管道的两端是否闭合，默认值为false。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。
`.tangents` : Array
一个Vector3切线数组。

`.normals` : Array
一个Vector3法线数组。

`.binormals` : Array
一个Vector3次法线数组。

#### 多面缓冲几何体（PolyhedronGeometry）

```js
const verticesOfCube = [
    -1,-1,-1,    1,-1,-1,    1, 1,-1,    -1, 1,-1,
    -1,-1, 1,    1,-1, 1,    1, 1, 1,    -1, 1, 1,
];

const indicesOfFaces = [
    2,1,0,    0,3,2,
    0,4,7,    7,3,0,
    0,1,5,    5,4,0,
    1,2,6,    6,5,1,
    2,3,7,    7,6,2,
    4,5,6,    6,7,4
];

const geometry = new THREE.PolyhedronGeometry( verticesOfCube, indicesOfFaces, 6, 2 );
```

##### 构造器

`PolyhedronGeometry(vertices : Array, indices : Array, radius : Float, detail : Integer)`
vertices — 一个顶点Array（数组）：[1,1,1, -1,-1,-1, ... ]。
indices — 一个构成面的索引Array（数组）， [0,1,2, 2,3,0, ... ]。
radius — Float - 最终形状的半径。
detail — Integer - 将对这个几何体细分多少个级别。细节越多，形状就越平滑。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 边缘几何体（EdgesGeometry）

```js
const geometry = new THREE.BoxGeometry( 100, 100, 100 );
const edges = new THREE.EdgesGeometry( geometry );
const line = new THREE.LineSegments( edges, new THREE.LineBasicMaterial( { color: 0xffffff } ) );
scene.add( line );

```

##### 构造器

`EdgesGeometry( geometry : BufferGeometry, thresholdAngle : Integer )`
geometry — 任何一个几何体对象。
thresholdAngle — 仅当相邻面的法线之间的角度（单位为角度）超过这个值时，才会渲染边缘。默认值为1。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。
`.parameters` : Object
一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

#### 网格几何体（WireframeGeometry）

```js
const geometry = new THREE.SphereGeometry( 100, 100, 100 );

const wireframe = new THREE.WireframeGeometry( geometry );

const line = new THREE.LineSegments( wireframe );
line.material.depthTest = false;
line.material.opacity = 0.25;
line.material.transparent = true;

scene.add( line );
```

##### 构造器

`WireframeGeometry( geometry : BufferGeometry )`
geometry — 任意几何体对象。

##### 属性|方法

>共有方法请参见其基类BufferGeometry。
>共有属性请参见其基类BufferGeometry。

### 材质

#### Material

材质描述了对象objects的外观。它们的定义方式与渲染器无关， 因此，如果您决定使用不同的渲染器，不必重写材质。

##### 构造器

`Material()`该方法创建一个通用材质。

#### 属性

- `.alphaTest` : Float
设置运行alphaTest时要使用的alpha值。如果不透明度低于此值，则不会渲染材质。默认值为0。

- `.alphaToCoverage` : Boolean
启用alpha to coverage. 只能在开启了MSAA的渲染环境中使用 (当渲染器创建的时候antialias 属性要true才能使用). 默认为 false.

- `.blendDst` : Integer
混合目标。默认值为OneMinusSrcAlphaFactor。 目标因子所有可能的取值请参阅constants。 必须将材质的blending设置为CustomBlending才能生效。
- `.blendDstAlpha` : Integer
.blendDst的透明度。 默认值为 null.

- `.blendEquation` : Integer
使用混合时所采用的混合方程式。默认值为AddEquation。 混合方程式所有可能的取值请参阅constants。 必须将材质的blending设置为CustomBlending才能生效。
.blendEquationAlpha : Integer
.blendEquation 的透明度. 默认值为 null.

- `.blending` : Blending
在使用此材质显示对象时要使用何种混合。
必须将其设置为CustomBlending才能使用自定义blendSrc, blendDst 或者 [page:Constant blendEquation]。 混合模式所有可能的取值请参阅constants。默认值为NormalBlending。

- `.blendSrc` : Integer
混合源。默认值为SrcAlphaFactor。 源因子所有可能的取值请参阅constants。
必须将材质的blending设置为CustomBlending才能生效。

- `.blendSrcAlpha` : Integer
.blendSrc的透明度。 默认值为 null.

- `.clipIntersection` : Boolean
更改剪裁平面的行为，以便仅剪切其交叉点，而不是它们的并集。默认值为 false。

- `.clippingPlanes` : Array
用户定义的剪裁平面，在世界空间中指定为THREE.Plane对象。这些平面适用于所有使用此材质的对象。空间中与平面的有符号距离为负的点被剪裁（未渲染）。 这需要WebGLRenderer.localClippingEnabled为true。 示例请参阅WebGL / clipping /intersection。默认值为 null。

- `.clipShadows` : Boolean
定义是否根据此材质上指定的剪裁平面剪切阴影。默认值为 false。

- `.colorWrite` : Boolean
是否渲染材质的颜色。 这可以与网格的renderOrder属性结合使用，以创建遮挡其他对象的不可见对象。默认值为true。

- `.defines` : Object
注入shader的自定义对象。 以键值对形式的对象传递，{ MY_CUSTOM_DEFINE: '' , PI2: Math.PI * 2 }。 这些键值对在顶点和片元着色器中定义。默认值为undefined。

- `.depthFunc` : Integer
使用何种深度函数。默认为LessEqualDepth。 深度模式所有可能的取值请查阅constants。

- `.depthTest` : Boolean
是否在渲染此材质时启用深度测试。默认为 true。

- `.depthWrite` : Boolean
渲染此材质是否对深度缓冲区有任何影响。默认为true。

在绘制2D叠加时，将多个事物分层在一起而不创建z-index时，禁用深度写入会很有用。

- `.forceSinglePass` : Boolean
决定双面透明的东西是否强制使用单通道渲染，默认为false。

为了减少一些半透明物体的渲染错误，此引擎调用两次绘制来渲染渲染双面透明的东西。 但是此方案可能会导致在某些情况下使绘制调用次数翻倍，例如渲染一些平面的植物例如草精灵之类的。 在这些情况下，将forceSinglePass设置为true来使用单通道渲染来避免性能问题。

- `.isMaterial` : Boolean
检查这个对象是否为材质Material的只读标记.

- `.stencilWrite` : Boolean
是否对模板缓冲执行模板操作，如果执行写入或者与模板缓冲进行比较，这个值需要设置为true。默认为false。

- `.stencilWriteMask` : Integer
写入模板缓冲区时所用的位元遮罩，默认为0xFF。

- `.stencilFunc` : Integer
使用模板比较时所用的方法，默认为AlwaysStencilFunc。在模板函数 constants 中查看可用的值

- `.stencilRef` : Integer
在进行模板比较或者模板操作的时候所用的基准值，默认为0。

- `.stencilFuncMask` : Integer
与模板缓冲进行比较时所使用的位元遮罩，默认为0xFF

- `.stencilFail` : Integer
当比较函数没有通过的时候要执行的模板操作，默认为KeepStencilOp，在模板操作 constants 查看可用值。

- `.stencilZFail` : Integer
当比较函数通过了但是深度检测没有通过的时候要执行的模板操作， 默认为KeepStencilOp，在模板操作 constants 查看可用值。

- `.stencilZPass` : Integer
当比较函数和深度检测都通过时要执行的模板操作，默认为KeepStencilOp，在模板操作constants 中查看可用值。

- `.id` : Integer
此材质实例的唯一编号。

- `.name` : String
对象的可选名称（不必是唯一的）。默认值为空字符串。

- `.needsUpdate` : Boolean
指定需要重新编译材质。

- `.opacity` : Float
在0.0 - 1.0的范围内的浮点数，表明材质的透明度。值0.0表示完全透明，1.0表示完全不透明。
如果材质的transparent属性未设置为true，则材质将保持完全不透明，此值仅影响其颜色。 默认值为1.0。
- `.polygonOffset` : Boolean
是否使用多边形偏移。默认值为false。这对应于WebGL的GL_POLYGON_OFFSET_FILL功能。

- `.polygonOffsetFactor` : Integer
设置多边形偏移系数。默认值为0。

- `.polygonOffsetUnits` : Integer
设置多边形偏移单位。默认值为0。

- `.precision` : String
重写此材质渲染器的默认精度。可以是"highp", "mediump" 或 "lowp"。默认值为null。

- `.premultipliedAlpha` : Boolean
是否预乘alpha（透明度）值。有关差异的示例，请参阅WebGL / Materials / Physical / Transmission。 默认值为false。

- `.dithering` : Boolean
是否对颜色应用抖动以消除条带的外观。默认值为 false。

- `.shadowSide` : Integer
定义投影的面。设置时，可以是THREE.FrontSide, THREE.BackSide, 或Materials。默认值为 null。
如果为null， 则面投射阴影确定如下：
|||
|-|-|
|THREE.FrontSide| 背面|
|THREE.BackSide |前面|
|THREE.DoubleSide |双面|

- `.side` : Integer
定义将要渲染哪一面 - 正面，背面或两者。 默认为THREE.FrontSide。其他选项有THREE.BackSide 和 THREE.DoubleSide。

- `.toneMapped` : Boolean
定义这个材质是否会被渲染器的toneMapping设置所影响，默认为 true 。

- `.transparent` : Boolean
定义此材质是否透明。这对渲染有影响，因为透明对象需要特殊处理，并在非透明对象之后渲染。
设置为true时，通过设置材质的opacity属性来控制材质透明的程度。
默认值为false。

- `.type` : String
值是字符串'Material'。不应该被更改，并且可以用于在场景中查找此类型的所有对象。

- `.uuid` : String
此材质实例的UUID，会自动分配，不应该被更改。

- `.version` : Integer
开始为0，会记录 .needsUpdate : Boolean设置为true的次数。

- `.vertexColors` : Boolean
是否使用顶点着色。默认值为false。 此引擎支持RGB或者RGBA两种顶点颜色，取决于缓冲 attribute 使用的是三分量（RGB）还是四分量（RGBA）。

- `.visible` : Boolean
此材质是否可见。默认为true。

- `.userData` : Object
一个对象，可用于存储有关Material的自定义数据。它不应该包含对函数的引用，因为这些函数不会被克隆。

##### 方法
>
>EventDispatcher 方法在此类中可用。

- `.clone ( ) : Material`
返回与此材质具有相同参数的新材质。

- `.copy ( material : material ) : this`
将被传入材质中的参数复制到此材质中。

- `.dispose () : undefined`
处理材质。材质的纹理不会被处理。需要通过Texture处理。

- `.onBeforeCompile ( shader : Shader, renderer : WebGLRenderer ) : undefined`
在编译shader程序之前立即执行的可选回调。此函数使用shader源码作为参数。用于修改内置材质。

和其他属性不一样的是，这个回调在.clone()，.copy() 和 .toJSON() 中不支持。

- `.customProgramCacheKey () : String`
当用到onBeforeCompile回调的时候，这个回调函数可以用来定义在onBeforeCompile中使用的配置项，这样three.js就可以根据这个回调返回的字符串来判定使用一个缓存的编译好的着色器代码还是根据需求重新编译一段新的着色器代码。

例如一个onBeforeCompile回调函数包含了下面的条件语句:

```js
if ( black ) {
 shader.fragmentShader = shader.fragmentShader.replace('gl_FragColor = vec4(1)', 'gl_FragColor = vec4(0)')
}
```

那么 customProgramCacheKey 就可以设置为:

```js
material.customProgramCacheKey = function() {
 return black ? '1' : '0';
}
```

和其他属性不一样的是，这个回调在.clone()，.copy() 和 .toJSON() 中不支持。

- `.setValues ( values : Object ) : undefined`
values -- 具有参数的容器。 根据values设置属性。
- `.toJSON ( meta : Object ) : Object`
meta -- 包含有元数据的对象，例如该对象的纹理或图片。 将material对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 基础网格材质(MeshBasicMaterial)

##### 构造器

`MeshBasicMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

`.alphaMap` : Texture
alpha贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为null。

仅使用纹理的颜色，忽略alpha通道（如果存在）。 对于RGB和RGBA纹理，WebGL渲染器在采样此纹理时将使用绿色通道， 因为在DXT压缩和未压缩RGB 565格式中为绿色提供了额外的精度。 Luminance-only以及luminance/alpha纹理也仍然有效。

`.aoMap` : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为null。aoMap需要第二组UV。

`.aoMapIntensity` : Float
环境遮挡效果的强度。默认值为1。零是不遮挡效果。

`.color` : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

`.combine` : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity在两种颜色之间进行混合。

`.envMap` : Texture
环境贴图。默认值为null。

`.fog` : Boolean
材质是否受雾影响。默认为true。

`.lightMap` : Texture
光照贴图。默认值为null。lightMap需要第二组UV。

`.lightMapIntensity` : Float
烘焙光的强度。默认值为1。

`.map` : Texture
颜色贴图。可以选择包括一个alpha通道，通常与.transparent 或.alphaTest。默认为null。

`.reflectivity` : Float
环境贴图对表面的影响程度; 见.combine。默认值为1，有效范围介于0（无反射）和1（完全反射）之间。

`.refractionRatio` : Float
空气的折射率（IOR）（约为1）除以材质的折射率。它与环境映射模式THREE.CubeRefractionMapping 和THREE.EquirectangularRefractionMapping一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过1。默认值为0.98。

`.specularMap` : Texture
材质使用的高光贴图。默认值为null。

`.wireframe` : Boolean
将几何体渲染为线框。默认值为false（即渲染为平面多边形）。

`.wireframeLinecap` : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

`.wireframeLinejoin` : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

`.wireframeLinewidth` : Float
控制线框宽度。默认值为1。

由于OpenGL Core Profile与大多数平台上WebGL渲染器的限制， 无论如何设置该值，线宽始终为1。

#### Lambert网格材质(MeshLambertMaterial)

一种非光泽表面的材质，没有镜面高光。
由于反射率和光照模型的简单性，MeshPhongMaterial，MeshStandardMaterial或者MeshPhysicalMaterial 上使用这种材质时会以一些图形精度为代价，得到更高的性能。

##### 构造器

`MeshLambertMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

.alphaMap : Texture
alpha贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为null。

仅使用纹理的颜色，忽略alpha通道（如果存在）。 对于RGB和RGBA纹理，WebGL渲染器在采样此纹理时将使用绿色通道， 因为在DXT压缩和未压缩RGB 565格式中为绿色提供了额外的精度。 Luminance-only以及luminance/alpha纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为null。aoMap需要第二组UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是0-1。默认值为1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.combine : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity在两种颜色之间进行混合。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为1。

.envMap : Texture
环境贴图。默认值为null。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为false。

.fog : Boolean
材质是否受雾影响。默认为true。

.lightMap : Texture
光照贴图。默认值为null。lightMap需要第二组UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为1。

.map : Texture
颜色贴图。可以选择包括一个alpha通道，通常与.transparent 或.alphaTest。默认为null。

.normalMap : Texture
用于创建法线贴图的纹理。RGB值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为THREE.TangentSpaceNormalMap（默认）和THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是0-1。默认值是Vector2设置为（1,1）。

.reflectivity : Float
环境贴图对表面的影响程度; 见.combine。默认值为1，有效范围介于0（无反射）和1（完全反射）之间。

.refractionRatio : Float
空气的折射率（IOR）（约为1）除以材质的折射率。它与环境映射模式THREE.CubeRefractionMapping 和THREE.EquirectangularRefractionMapping一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过1。默认值为0.98。

.specularMap : Texture
材质使用的高光贴图。默认值为null。

.wireframe : Boolean
将几何体渲染为线框。默认值为false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为1。

由于OpenGL Core Profile与 大多数平台上WebGL渲染器的限制，无论如何设置该值，线宽始终为1。

#### Phong网格材质(MeshPhongMaterial)

一种用于具有镜面高光的光泽表面的材质。

在MeshStandardMaterial或MeshPhysicalMaterial上使用此材质时，性能通常会更高 ，但会牺牲一些图形精度。

##### 构造器

`MeshPhongMaterial( parameters : Object )`

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

.alphaMap : Texture
alpha贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为null。

仅使用纹理的颜色，忽略alpha通道（如果存在）。 对于RGB和RGBA纹理，WebGL渲染器在采样此纹理时将使用绿色通道， 因为在DXT压缩和未压缩RGB 565格式中为绿色提供了额外的精度。 Luminance-only以及luminance/alpha纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为null。aoMap需要第二组UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是0-1。默认值为1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.combine : Integer
如何将表面颜色的结果与环境贴图（如果有）结合起来。

选项为THREE.MultiplyOperation（默认值），THREE.MixOperation， THREE.AddOperation。如果选择多个，则使用.reflectivity在两种颜色之间进行混合。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为1。

.envMap : Texture
环境贴图。默认值为null。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为false。

.fog : Boolean
材质是否受雾影响。默认为true。

.lightMap : Texture
光照贴图。默认值为null。lightMap需要第二组UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为1。

.map : Texture
颜色贴图。可以选择包括一个alpha通道，通常与.transparent 或.alphaTest。默认为null。 纹理贴图颜色由漫反射颜色.color调节。

.normalMap : Texture
用于创建法线贴图的纹理。RGB值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为THREE.TangentSpaceNormalMap（默认）和THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是0-1。默认值是Vector2设置为（1,1）。

.reflectivity : Float
环境贴图对表面的影响程度; 见.combine。默认值为1，有效范围介于0（无反射）和1（完全反射）之间。

.refractionRatio : Float
空气的折射率（IOR）（约为1）除以材质的折射率。它与环境映射模式THREE.CubeRefractionMapping 和THREE.EquirectangularRefractionMapping一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过1。默认值为0.98。

.shininess : Float
.specular高亮的程度，越高的值越闪亮。默认值为 30。

.specular : Color
材质的高光颜色。默认值为0x111111（深灰色）的颜色Color。

这定义了材质的光泽度和光泽的颜色。

.specularMap : Texture
镜面反射贴图值会影响镜面高光以及环境贴图对表面的影响程度。默认值为null。

.wireframe : Boolean
将几何体渲染为线框。默认值为false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为1。

由于OpenGL Core Profile与 大多数平台上WebGL渲染器的限制，无论如何设置该值，线宽始终为1。

#### 标准网格材质(MeshStandardMaterial)

一种基于物理的标准材质，使用Metallic-Roughness工作流程。

该材质提供了比MeshLambertMaterial 或MeshPhongMaterial 更精确和逼真的结果，代价是计算成本更高

##### 构造器

`MeshStandardMaterial( parameters : Object )`

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

.alphaMap : Texture
alpha贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为null。

仅使用纹理的颜色，忽略alpha通道（如果存在）。 对于RGB和RGBA纹理，WebGL渲染器在采样此纹理时将使用绿色通道， 因为在DXT压缩和未压缩RGB 565格式中为绿色提供了额外的精度。 Luminance-only以及luminance/alpha纹理也仍然有效。

.aoMap : Texture
该纹理的红色通道用作环境遮挡贴图。默认值为null。aoMap需要第二组UV。

.aoMapIntensity : Float
环境遮挡效果的强度。默认值为1。零是不遮挡效果。

.bumpMap : Texture
用于创建凹凸贴图的纹理。黑色和白色值映射到与光照相关的感知深度。凹凸实际上不会影响对象的几何形状，只影响光照。如果定义了法线贴图，则将忽略该贴图。

.bumpScale : Float
凹凸贴图会对材质产生多大影响。典型范围是0-1。默认值为1。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。

.defines : Object
如下形式的对象:`{ 'STANDARD': '' }`;WebGLRenderer使用它来选择shaders。

.displacementMap : Texture
位移贴图会影响网格顶点的位置，与仅影响材质的光照和阴影的其他贴图不同，移位的顶点可以投射阴影，阻挡其他对象， 以及充当真实的几何体。位移纹理是指：网格的所有顶点被映射为图像中每个像素的值（白色是最高的），并且被重定位。

.displacementScale : Float
位移贴图对网格的影响程度（黑色是无位移，白色是最大位移）。如果没有设置位移贴图，则不会应用此值。默认值为1。

.displacementBias : Float
位移贴图在网格顶点上的偏移量。如果没有设置位移贴图，则不会应用此值。默认值为0。

.emissive : Color
材质的放射（光）颜色，基本上是不受其他光照影响的固有颜色。默认为黑色。

.emissiveMap : Texture
设置放射（发光）贴图。默认值为null。放射贴图颜色由放射颜色和强度所调节。 如果你有一个放射贴图，请务必将放射颜色设置为黑色以外的其他颜色。

.emissiveIntensity : Float
放射光强度。调节发光颜色。默认为1。

.envMap : Texture
环境贴图，为了能够保证物理渲染准确，您应该添加由PMREMGenerator预处理过的环境贴图，默认为null。

.envMapIntensity : Float
通过乘以环境贴图的颜色来缩放环境贴图的效果。

.flatShading : Boolean
定义材质是否使用平面着色进行渲染。默认值为false。

.fog : Boolean
材质是否受雾影响。默认为true。

.isMeshStandardMaterial : Boolean
检查当前对象是否为标准网格材质的标记。

.lightMap : Texture
光照贴图。默认值为null。lightMap需要第二组UV。

.lightMapIntensity : Float
烘焙光的强度。默认值为1。

.map : Texture
颜色贴图。可以选择包括一个alpha通道，通常与.transparent 或.alphaTest。默认为null。 纹理贴图颜色由漫反射颜色.color调节。

.metalness : Float
材质与金属的相似度。非金属材质，如木材或石材，使用0.0，金属使用1.0，通常没有中间值。 默认值为0.0。0.0到1.0之间的值可用于生锈金属的外观。如果还提供了metalnessMap，则两个值相乘。

.metalnessMap : Texture
该纹理的蓝色通道用于改变材质的金属度。

.normalMap : Texture
用于创建法线贴图的纹理。RGB值会影响每个像素片段的曲面法线，并更改颜色照亮的方式。法线贴图不会改变曲面的实际形状，只会改变光照。 In case the material has a normal map authored using the left handed convention, the y component of normalScale should be negated to compensate for the different handedness.

.normalMapType : Integer
法线贴图的类型。

选项为THREE.TangentSpaceNormalMap（默认）和THREE.ObjectSpaceNormalMap。

.normalScale : Vector2
法线贴图对材质的影响程度。典型范围是0-1。默认值是Vector2设置为（1,1）。

.refractionRatio : Float
空气的折射率（IOR）（约为1）除以材质的折射率。它与环境映射模式THREE.CubeRefractionMapping 和THREE.EquirectangularRefractionMapping一起使用。 The index of refraction (IOR) of air (approximately 1) divided by the index of refraction of the material. It is used with environment mapping mode THREE.CubeRefractionMapping. 折射率不应超过1。默认值为0.98。

.roughness : Float
材质的粗糙程度。0.0表示平滑的镜面反射，1.0表示完全漫反射。默认值为1.0。如果还提供roughnessMap，则两个值相乘。

.roughnessMap : Texture
该纹理的绿色通道用于改变材质的粗糙度。

.wireframe : Boolean
将几何体渲染为线框。默认值为false（即渲染为平面多边形）。

.wireframeLinecap : String
定义线两端的外观。可选值为 'butt'，'round' 和 'square'。默认为'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinejoin : String
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

.wireframeLinewidth : Float
控制线框宽度。默认值为1。
由于OpenGL Core Profile与 大多数平台上WebGL渲染器的限制，无论如何设置该值，线宽始终为1。

#### 物理网格材质(MeshPhysicalMaterial)

提供了更高级的基于物理的渲染属性
物理网格材质使用了更复杂的着色器功能，所以在每个像素的渲染都要比three.js中的其他材质更费性能，大部分的特性是默认关闭的，需要手动开启，每开启一项功能在开启的时候才会更耗性能。请注意，为获得最佳效果，您在使用此材质时应始终指定environment map。

##### 构造器

`MeshPhysicalMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从Material和MeshStandardMaterial继承的任何属性)

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

#### 基础线条材质（LineBasicMaterial）

```js
const material = new THREE.LineBasicMaterial( {
 color: 0xffffff,
 linewidth: 1,
 linecap: 'round', //ignored by WebGLRenderer
 linejoin:  'round' //ignored by WebGLRenderer
} );
```

##### 构造器

`LineBasicMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

- `.color : Color`
材质的颜色(Color)，默认值为白色 (0xffffff)。

- `.fog : Boolean`
材质是否受雾影响。默认为true。

- `.linewidth : Float`
控制线宽。默认值为 1。

由于OpenGL Core Profile与 大多数平台上WebGL渲染器的限制，无论如何设置该值，线宽始终为1。

- `.linecap : String`
定义线两端的样式。可选值为 'butt', 'round' 和 'square'。默认值为 'round'。

该属性对应2D Canvas lineCap属性， 并且会被WebGL渲染器忽略。

- `.linejoin : String`
定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应2D Canvas lineJoin属性， 并且会被WebGL渲染器忽略。

- `.map : Texture`
Sets the color of the lines using data from a Texture.

#### 虚线材质(LineDashedMaterial)

```js
const material = new THREE.LineDashedMaterial( {
 color: 0xffffff,
 linewidth: 1,
 scale: 1,
 dashSize: 3,
 gapSize: 1,
} );
```

##### 构造器

`LineDashedMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从LineBasicMaterial继承的任何属性)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。

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
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从LineBasicMaterial继承的任何属性)。

#### 阴影材质(ShadowMaterial)

此材质可以接收阴影，但在其他方面完全透明。

```js
const geometry = new THREE.PlaneGeometry( 2000, 2000 );
geometry.rotateX( - Math.PI / 2 );

const material = new THREE.ShadowMaterial();
material.opacity = 0.2;

const plane = new THREE.Mesh( geometry, material );
plane.position.y = -200;
plane.receiveShadow = true;
scene.add( plane );
```

##### 构造器

`ShadowMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从LineBasicMaterial继承的任何属性)。

##### 属性|方法

>共有方法请参见其基类Material。
>共有属性请参见其基类Material。
.color : Color
Color of the material, by default set to black (0x000000).

.fog : Boolean
材质是否受雾影响。默认为true。

.transparent : Boolean
定义此材质是否透明。默认值为 true。

#### 点材质(PointsMaterial)

##### 构造器

`PointsMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从LineBasicMaterial继承的任何属性)。
属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色），内部调用Color.set(color)。

#### 点精灵材质(SpriteMaterial)

```js
const map = new THREE.TextureLoader().load( 'textures/sprite.png' );
const material = new THREE.SpriteMaterial( { map: map, color: 0xffffff } );

const sprite = new THREE.Sprite( material );
sprite.scale.set(200, 200, 1)
scene.add( sprite );

```

##### 构造器

`SpriteMaterial( parameters : Object )`
parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。 材质的任何属性都可以从此处传入(包括从Material 和 ShaderMaterial继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 0xffffff（白色）， 内部调用Color.set(color)。 SpriteMaterials不会被Material.clippingPlanes裁剪。

##### 属性|方法

.alphaMap : Texture
alpha贴图是一张灰度纹理，用于控制整个表面的不透明度。（黑色：完全透明；白色：完全不透明）。 默认值为null。

仅使用纹理的颜色，忽略alpha通道（如果存在）。 对于RGB和RGBA纹理，WebGL渲染器在采样此纹理时将使用绿色通道， 因为在DXT压缩和未压缩RGB 565格式中为绿色提供了额外的精度。 Luminance-only以及luminance/alpha纹理也仍然有效。

.color : Color
材质的颜色(Color)，默认值为白色 (0xffffff)。 .map会和 color 相乘。

.fog : Boolean
材质是否受雾影响。默认为true。

.isSpriteMaterial : Boolean
Read-only flag to check if a given object is of type SpriteMaterial.

.map : Texture
颜色贴图。可以选择包括一个alpha通道，通常与.transparent 或.alphaTest。默认为null。

.rotation : Radians
sprite的转动，以弧度为单位。默认值为0。

.sizeAttenuation : Boolean
精灵的大小是否会被相机深度衰减。（仅限透视摄像头。）默认为true。

.transparent : Boolean
定义此材质是否透明。默认值为 true。

### 物体

#### 三维物体（Object3D）

这是Three.js中大部分对象的基类，提供了一系列的属性和方法来对三维空间中的物体进行操纵。

请注意，可以通过.add( object )方法来将对象进行组合，该方法将对象添加为子对象，但为此最好使用Group（来作为父对象）

##### 构造器

`Object3D()`
构造器中不带有参数。

##### 属性

- `.animations` : AnimationClip
三维物体所属的动画剪辑数组.

- `.castShadow` : Boolean
对象是否被渲染到阴影贴图中。默认值为false。

- `.children` : Array
含有对象的子级的数组。请参阅Group来了解将手动对象进行分组的相关信息。

- `.customDepthMaterial` : Material
在渲染到深度图的时候所用的自定义深度材质。 只能在网格中使用。 当使用DirectionalLight（平行光）或者SpotLight（聚光灯光）生成影子的时候, 如果你调整过顶点着色器中的顶点位置，就需要定义一个自定义深度材质来生成正确的影子。默认为undefined.

- `.customDistanceMaterial` : Material
与customDepthMaterial相同，但与PointLight(点光源）一起使用。默认值为undefined。

- `.frustumCulled` : Boolean
当这个设置了的时候，每一帧渲染前都会检测这个物体是不是在相机的视椎体范围内。 如果设置为false 物体不管是不是在相机的视椎体范围内都会渲染。默认为true。

- `.id` : Integer
只读 —— 表示该对象实例ID的唯一数字。

- `.isObject3D` : Boolean
查看所给对象是不是Object3D类型的只读标记.

- `.layers` : Layers
物体的层级关系。 物体只有和一个正在使用的Camera至少在同一个层时才可见。当使用Raycaster进行射线检测的时候此项属性可以用于过滤不参与检测的物体.

- `.matrix` : Matrix4
局部变换矩阵。

- `.matrixAutoUpdate` : Boolean
当这个属性设置了之后，它将计算每一帧的位移、旋转（四元变换）和缩放矩阵，并重新计算matrixWorld属性。默认值是Object3D.DEFAULT_MATRIX_AUTO_UPDATE (true)。

- `.matrixWorld` : Matrix4
物体的世界变换。若这个Object3D没有父级，则它将和local transform .matrix（局部变换矩阵）相同。

- `.matrixWorldAutoUpdate` : Boolean
默认为 true. 当设置的时候，渲染器在每一帧都会检查物体自身以及它的自带是否需要更新世界变换矩阵。 如果不需要的话它自身以及它的子代的所有世界变换矩阵都需要你来维护。

- `.matrixWorldNeedsUpdate` : Boolean
当这个属性设置了之后，它将计算在那一帧中的matrixWorld，并将这个值重置为false。默认值为false。

- `.modelViewMatrix` : Matrix4
这个值传递给着色器，用于计算物体的位置。

- `.name` : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

- `.normalMatrix` : Matrix3
这个值传递给着色器，用于计算物体的光照。 它是物体的modelViewMatrix矩阵中，左上角3x3子矩阵的逆的转置矩阵。

使用这个特殊矩阵的原因，是只需使用modelViewMatrix就可以得出一个法线（缩放时）的非单位长度或者非垂直的方向（不规则缩放时）。

另一方面，modelViewMatrix矩阵中的位移部分和法线的计算无关，因此Matrix3就已经足够了。

- `.onAfterRender` : Function
一个可选的回调函数，在Object3D渲染之后直接执行。 使用以下参数来调用此函数：renderer，scene，camera，geometry，material，group。

注意此回调函数只会在*可渲染*的3D物体上执行。可渲染的3D物体指的是那种拥有视觉表现的、定义了几何体与材质的物体，例如像是Mesh、Line、Points 或者Sprite。 Object3D、 Group 或者 Bone 这些是不可渲染的物体，因此此回调函数不会在这样的物体上执行。

- `.onBeforeRender` : Function
一个可选的回调函数，在Object3D渲染之前直接执行。 使用以下参数来调用此函数：renderer，scene，camera，geometry，material，group。

注意此回调函数只会在*可渲染*的3D物体上执行。可渲染的3D物体指的是那种拥有视觉表现的、定义了几何体与材质的物体，例如像是Mesh、Line、Points 或者Sprite。 Object3D、 Group 或者 Bone 这些是不可渲染的物体，因此此回调函数不会在这样的物体上执行。

- `.parent` : Object3D
在scene graph（场景图）中，一个对象的父级对象。 一个对象最多仅能有一个父级对象。

- `.position` : Vector3
表示对象局部位置的Vector3。默认值为(0, 0, 0)。

- `.quaternion` : Quaternion
表示对象局部旋转的Quaternion（四元数）。

- `.receiveShadow` : Boolean
材质是否接收阴影。默认值为false。

- `.renderOrder` : Number
这个值将使得scene graph（场景图）中默认的的渲染顺序被覆盖， 即使不透明对象和透明对象保持独立顺序。 渲染顺序是由低到高来排序的，默认值为0。

- `.rotation` : Euler
物体的局部旋转，以弧度来表示。（请参阅Euler angles-欧拉角）

- `.scale` : Vector3
物体的局部缩放。默认值是Vector3( 1, 1, 1 )。

- `.up` : Vector3
这个属性由lookAt方法所使用，例如，来决定结果的朝向。 默认值是Object3D.DEFAULT_UP，即( 0, 1, 0 )。

- `.userData` : Object
一个用于存储Object3D自定义数据的对象。 它不应当包含对函数的引用，因为这些函数将不会被克隆。

- `.uuid` : String
该对象实例的UUID。 这是一个自动生成的值，不应当对其进行修改。

- `.visible` : Boolean
可见性。这个值为true时，物体将被渲染。默认值为true。

静态属性
静态属性和方法由每个类所定义，并非由每个类的实例所定义。 也就是说，改变Object3D.DEFAULT_UP或Object3D.DEFAULT_MATRIX_AUTO_UPDATE的值， 将改变每个在此之后由Object3D类（或派生类）创建的实例中的up和matrixAutoUpdate的值。（已经创建好的Object3D不会受到影响）。

- `.DEFAULT_UP` : Vector3
默认的物体的up方向，同时也作为DirectionalLight、HemisphereLight和Spotlight（自顶向下创建的灯光）的默认方向。 默认设为( 0, 1, 0 )。

- `.DEFAULT_MATRIX_AUTO_UPDATE` : Boolean
matrixAutoUpdate的默认设置，用于新创建的Object3D。
.DEFAULT_MATRIX_WORLD_AUTO_UPDATE : Boolean
matrixWorldAutoUpdate的默认设置，用于新创建的Object3D。

##### 方法

EventDispatcher 在该类上可用的所有方法。

- `.add ( object : Object3D, ... ) : this`
添加对象到这个对象的子级，可以添加任意数量的对象。 当前传入的对象中的父级将在这里被移除，因为一个对象仅能有一个父级。

请参阅Group来查看手动编组对象的相关信息。

- `.applyMatrix4 ( matrix : Matrix4 ) : undefined`
对当前物体应用这个变换矩阵，并更新物体的位置、旋转和缩放。

- `.applyQuaternion ( quaternion : Quaternion ) : this`
对当前物体应用由四元数所表示的变换。

- `.attach ( object : Object3D ) : this`
将object作为子级来添加到该对象中，同时保持该object的世界变换。

- `.clone ( recursive : Boolean ) : Object3D`
recursive —— 如果值为true，则该物体的后代也会被克隆。默认值为true。

返回对象前物体的克隆（以及可选的所有后代）。

- `.copy ( object : Object3D, recursive : Boolean ) : this`
recursive —— 如果值为true，则该物体的后代也会被复制。默认值为true。

复制给定的对象到这个对象中。 请注意，事件监听器和用户定义的回调函数（.onAfterRender 和 .onBeforeRender）不会被复制。

- `.getObjectById ( id : Integer ) : Object3D`
id —— 标识该对象实例的唯一数字。

从该对象开始，搜索一个对象及其子级，返回第一个带有匹配id的子对象。
请注意，id是按照时间顺序来分配的：1、2、3、……，每增加一个新的对象就自增1。

- `.getObjectByName ( name : String ) : Object3D`
name —— 用于来匹配子物体中Object3D.name属性的字符串。

从该对象开始，搜索一个对象及其子级，返回第一个带有匹配name的子对象。
请注意，大多数的对象中name默认是一个空字符串，要使用这个方法，你将需要手动地设置name属性。

- `.getObjectByProperty ( name : String, value : Any ) : Object3D`
name —— 将要用于查找的属性的名称。
value —— 给定的属性的值。

从该对象开始，搜索一个对象及其子级，返回第一个给定的属性中包含有匹配的值的子对象。

- `.getObjectsByProperty ( name : String, value : Any ) : Object3D`
name —— 将要用于查找的属性的名称。
value —— 给定的属性的值。

从此对象开始，搜索一个对象及其子对象，返回包含给定属性的匹配值的所有子对象。

- `.getWorldPosition ( target : Vector3 ) : Vector3`
target — 结果将被复制到这个Vector3中。

返回一个表示该物体在世界空间中位置的矢量。

- `.getWorldQuaternion ( target : Quaternion ) : Quaternion`
target — 结果将被复制到这个Quaternion中。

返回一个表示该物体在世界空间中旋转的四元数。

- `.getWorldScale ( target : Vector3 ) : Vector3`
target — 结果将被复制到这个Vector3中。

返回一个包含着该物体在世界空间中各个轴向上所应用的缩放因数的矢量。

- `.getWorldDirection ( target : Vector3 ) : Vector3`
target — 结果将被复制到这个Vector3中。

返回一个表示该物体在世界空间中Z轴正方向的矢量。

- `.localToWorld ( vector : Vector3 ) : Vector3`
vector - 一个表示在该物体局部空间中位置的向量。

将该向量从物体的局部空间转换到世界空间。

- `.lookAt ( vector : Vector3 ) : undefined`
.lookAt ( x : Float, y : Float, z : Float ) : undefined
vector - 一个表示世界空间中位置的向量。

也可以使用世界空间中x、y和z的位置分量。

旋转物体使其在世界空间中面朝一个点。

这一方法不支持其父级被旋转过或者被位移过的物体。

- `.raycast ( raycaster : Raycaster, intersects : Array ) : undefined`
抽象（空方法），在一条被投射出的射线与这个物体之间获得交点。 在一些子类，例如Mesh, Line, and Points实现了这个方法，以用于光线投射。

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

绕局部空间的X轴旋转这个物体。

- `.rotateY ( rad : Float ) : this`
rad - 将要旋转的角度（以弧度来表示）。

绕局部空间的Y轴旋转这个物体。

- `.rotateZ ( rad : Float ) : this`
rad - 将要旋转的角度（以弧度来表示）。

绕局部空间的Z轴旋转这个物体。

- `.setRotationFromAxisAngle ( axis : Vector3, angle : Float ) : undefined`
axis -- 一个在局部空间中的标准化向量。
angle -- 角度（以弧度来表示）。

调用.quaternion中的setFromAxisAngle( axis, angle )。

- `.setRotationFromEuler ( euler : Euler ) : undefined`
euler -- 指定了旋转量的欧拉角。
调用.quaternion中的setRotationFromEuler( euler)。

- `.setRotationFromMatrix ( m : Matrix4 ) : undefined`
m -- 通过该矩阵中的旋转分量来旋转四元数。
调用.quaternion中的setFromRotationMatrix( m)。

请注意，这里假设m上的3x3矩阵是一个纯旋转矩阵（即未缩放的矩阵）。

- `.setRotationFromQuaternion ( q : Quaternion ) : undefined`
q -- 标准化的四元数。

将所给的四元数复制到.quaternion中。

- `.toJSON ( meta : Object ) : Object`
meta -- 包含有元数据的对象，例如该对象的材质、纹理或图片。 将对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

- `.translateOnAxis ( axis : Vector3, distance : Float ) : this`
axis -- 一个在局部空间中的标准化向量。
distance -- 将要平移的距离。

在局部空间中沿着一条轴来平移物体，假设轴已被标准化。

- `.translateX ( distance : Float ) : this`
沿着X轴将平移distance个单位。

- `.translateY ( distance : Float ) : this`
沿着Y轴将平移distance个单位。

- `.translateZ ( distance : Float ) : this`
沿着Z轴将平移distance个单位。

- `.traverse ( callback : Function ) : undefined`
callback - 以一个object3D对象作为第一个参数的函数。

在对象以及后代中执行的回调函数。

- `.traverseVisible ( callback : Function ) : undefined`
callback - 以一个object3D对象作为第一个参数的函数。

类似traverse函数，但在这里，回调函数仅对可见的对象执行，不可见对象的后代将不遍历。

- `.traverseAncestors ( callback : Function ) : undefined`
callback - 以一个object3D对象作为第一个参数的函数。

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

表示基于以三角形为polygon mesh（多边形网格）的物体的类。 同时也作为其他类的基类
网格模型Mesh其实就一个一个三角形(面)拼接构成。使用使用网格模型Mesh渲染几何体geometry，就是几何体所有顶点坐标三个为一组，构成一个三角形，多组顶点构成多个三角形，就可以用来模拟表示物体的表面。
空间中一个三角形有正反两面，相机对着三角形的一个面，如果三个顶点的顺序是逆时针方向，该面视为正面，如果三个顶点的顺序是顺时针方向，该面视为反面。

```js
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0xffff00 } );
const mesh = new THREE.Mesh( geometry, material );
scene.add( mesh );
```

网格模型Mesh对应的几何体BufferGeometry，拆分为多个三角后，很多三角形重合的顶点位置坐标是相同的，这时候如果你想减少顶点坐标数据量，可以借助几何体顶点索引geometry.index来实现。

```js
//每个三角形3个顶点坐标，矩形平面可以拆分为两个三角形，也就是6个顶点坐标。
const vertices = new Float32Array([
    0, 0, 0, //顶点1坐标
    80, 0, 0, //顶点2坐标
    80, 80, 0, //顶点3坐标
    0, 0, 0, //顶点4坐标   和顶点1位置相同
    80, 80, 0, //顶点5坐标  和顶点3位置相同
    0, 80, 0, //顶点6坐标
]);
//如果几何体有顶点索引geometry.index，那么你可以吧三角形重复的顶点位置坐标删除。
const vertices = new Float32Array([
    0, 0, 0, //顶点1坐标
    80, 0, 0, //顶点2坐标
    80, 80, 0, //顶点3坐标
    0, 80, 0, //顶点4坐标
]);
```

BufferAttribute定义顶点索引.index数据
通过javascript类型化数组Uint16Array创建顶点索引.index数据。

```js
// Uint16Array类型数组创建顶点索引数据
const indexes = new Uint16Array([
    // 下面索引值对应顶点位置数据中的顶点坐标
    0, 1, 2, 0, 2, 3,
])
```

通过threejs的属性缓冲区对象BufferAttribute表示几何体顶点索引.index数据。

```js
// 索引数据赋值给几何体的index属性
geometry.index = new THREE.BufferAttribute(indexes, 1); //1个为一组
```

#### 构造器

`Mesh( geometry : BufferGeometry, material : Material )`
geometry —— （可选）BufferGeometry的实例，默认值是一个新的BufferGeometry。
material —— （可选）一个Material，或是一个包含有Material的数组，默认是一个新的MeshBasicMa`terial。

##### 属性

共有属性请参见其基类Object3D。

.geometry : BufferGeometry
BufferGeometry 的实例或者派生类，定义了物体的结构。

.isMesh : Boolean
Read-only flag to check if a given object is of type Mesh.

.material : Material
由Material基类或者一个包含材质的数组派生而来的材质实例，定义了物体的外观。默认值是一个MeshBasicMaterial。

.morphTargetInfluences : Array
一个包含有权重（值一般在0-1范围内）的数组，指定应用了多少变形。 默认情况下是未定义的，但是会被updateMorphTargets重置为一个空数组。

.morphTargetDictionary : Object
基于morphTarget.name属性的morphTargets字典。 默认情况下是未定义的，但是会被updateMorphTargets重建。

##### 方法

共有方法请参见其基类Object3D。

.clone () : Mesh
返回这个Mesh对象及其子级的克隆。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的Ray（射线）和这个网格之间产生交互。 Raycaster.intersectObject将会调用这个方法。

.updateMorphTargets () : undefined
更新morphTargets，使其不对对象产生影响，重置morphTargetInfluences and morphTargetDictionary属性。

#### 点（Points）

一个用于显示点的类。 由WebGLRenderer渲染的点使用 gl.POINTS。

##### 构造器

`Points( geometry : BufferGeometry, material : Material )`
geometry —— （可选）是一个BufferGeometry的实例，默认值是一个新的BufferGeometry。几何体geometry作为点模型Points参数，会把几何体渲染为点
material —— （可选） 是一个对象，默认值是一个PointsMaterial。

##### 属性

共有属性请参见其基类Object3D。

.geometry : BufferGeometry
一个BufferGeometry的实例（或者派生类），定义了物体的结构。

.isPoints : Boolean
Read-only flag to check if a given object is of type Points.

.material : Material
Material的实例。定义了物体的外观。默认值是一个的PointsMaterial。

.morphTargetInfluences : Array
一个包含有权重（值一般在0-1范围内）的数组，指定应用了多少变形。 默认情况下是未定义的，但是会被updateMorphTargets重置为一个空数组。

.morphTargetDictionary : Object
基于morphTarget.name属性的morphTargets字典。 默认情况下是未定义的，但是会被updateMorphTargets重建。

##### 方法

共有方法请参见其基类Object3D。

.clone () : Mesh
返回这个Mesh对象及其子级的克隆。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的Ray（射线）和这个网格之间产生交互。 Raycaster.intersectObject将会调用这个方法。

.updateMorphTargets () : undefined
更新morphTargets，使其不对对象产生影响，重置morphTargetInfluences and morphTargetDictionary属性。

#### 精灵（Sprite）

精灵是一个总是面朝着摄像机的平面，通常含有使用一个半透明的纹理。精灵不会投射任何阴影

```js
const map = new THREE.TextureLoader().load( "sprite.png" );
const material = new THREE.SpriteMaterial( { map: map } );

const sprite = new THREE.Sprite( material );
scene.add( sprite );
```

##### 构造器

Sprite( material : Material )
material - （可选值）是SpriteMaterial的一个实例。 默认值是一个白色的SpriteMaterial。

创建一个新的Sprite。

##### 属性

共有属性请参见其基类Object3D。

.isSprite : Boolean
Read-only flag to check if a given object is of type Sprite.

.material : SpriteMaterial
SpriteMaterial的一个实例，定义了这个对象的外观。默认值是一个白色的SpriteMaterial。

.center : Vector2
这个精灵的锚点，也就是精灵旋转时，围绕着旋转的点。当值为(0.5,0.5)时，对应着这个精灵的中心点；当值为(0,0)时，对应着这个精灵左下角的点。
其默认值是(0.5,0.5)。

##### 方法

共有方法请参见其基类Object3D。

.clone () : Sprite
返回当前Sprite对象的一个克隆及其任何后代。

.copy ( sprite : Sprite ) : this
将前一个Sprite对象的属性复制给当前的这个对象。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在投射的光线和精灵之前产生交互。Raycaster.intersectObject将会调用这个方法。 在对sprite进行射线投射之前，射线投射必须通过调用Raycaster.setFromCamera()来初始化。

#### 线（Line）

它几乎和LineSegments是一样的，唯一的区别是它在渲染时使用的是gl.LINE_STRIP， 而不是gl.LINES。

```js
const material = new THREE.LineBasicMaterial({
 color: 0x0000ff
});

const points = [];
points.push( new THREE.Vector3( - 10, 0, 0 ) );
points.push( new THREE.Vector3( 0, 10, 0 ) );
points.push( new THREE.Vector3( 10, 0, 0 ) );

const geometry = new THREE.BufferGeometry().setFromPoints( points );

const line = new THREE.Line( geometry, material );
scene.add( line );
```

##### 构造器

`Line( geometry : BufferGeometry, material : Material )`
geometry —— 表示线段的顶点，默认值是一个新的BufferGeometry。几何体作为线模型Line (opens new window)的参数，你会发现渲染效果是从第一个点开始到最后一个点，依次连成线。
material —— 线的材质，默认值是一个新的具有随机颜色的LineBasicMaterial。

##### 属性

共有属性请参见其基类Object3D。

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

共有方法请参见其基类Object3D。

.computeLineDistances () : this
计算LineDashedMaterial所需的距离的值的数组。 对于几何体中的每一个顶点，这个方法计算出了当前点到线的起始点的累积长度。

.raycast ( raycaster : Raycaster, intersects : Array ) : undefined
在一条投射出去的Ray（射线）和这条线之间产生交互。 Raycaster.intersectObject将会调用这个方法。

.clone () : Line
返回这条线及其子集的一个克隆对象。

.updateMorphTargets () : undefined
Updates the morphTargets to have no influence on the object. Resets the .morphTargetInfluences and .morphTargetDictionary properties.

#### 组（Group）

它几乎和Object3D是相同的，其目的是使得组中对象在语法上的结构更加清晰。

```js
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );

const cubeA = new THREE.Mesh( geometry, material );
cubeA.position.set( 100, 100, 0 );

const cubeB = new THREE.Mesh( geometry, material );
cubeB.position.set( -100, -100, 0 );

//create a group and add the two cubes
//These cubes can now be rotated / scaled etc as a group
const group = new THREE.Group();
group.add( cubeA );
group.add( cubeB );

scene.add( group );
```

##### 构造器

`Group( )`

##### 属性|方法

>共有属性请参见其基类Object3D。
>共有方法请参见其基类Object3D。

.isGroup : Boolean
Read-only flag to check if a given object is of type Group.

.type : String
一个字符串：“Group”。这个属性不应当被改变。

#### 实例化网格（InstancedMesh）

一种具有实例化渲染支持的特殊版本的Mesh。你可以使用 InstancedMesh 来渲染大量具有相同几何体与材质、但具有不同世界变换的物体。 使用 InstancedMesh 将帮助你减少 draw call 的数量，从而提升你应用程序的整体渲染性能。

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

`canvas` - 一个供渲染器绘制其输出的canvas 它和下面的domElement属性对应。 如果没有传这个参数，会创建一个新canvas
`context` - 可用于将渲染器附加到已有的渲染环境(RenderingContext)中。默认值是null
`precision` - 着色器精度. 可以是 "highp", "mediump" 或者 "lowp". 如果设备支持，默认为"highp" .
`alpha` - controls the default clear alpha value. When set to true, the value is 0. Otherwise it's 1. Default is false.
`premultipliedAlpha` - renderer是否假设颜色有 premultiplied alpha. 默认为true
`antialias` - 是否执行抗锯齿。默认为false.
`stencil` - 绘图缓存是否有一个至少8位的模板缓存(stencil buffer)。默认为true
`preserveDrawingBuffer` -是否保留缓直到手动清除或被覆盖。 默认false.
`powerPreference` - 提示用户代理怎样的配置更适用于当前WebGL环境。 可能是"high-performance", "low-power" 或 "default"。默认是"default". 详见WebGL spec
`failIfMajorPerformanceCaveat` - 检测渲染器是否会因性能过差而创建失败。默认为false。详见 WebGL spec for details.
`depth` - 绘图缓存是否有一个至少6位的深度缓存(depth buffer )。 默认是true.
`logarithmicDepthBuffer` - 是否使用对数深度缓存。如果要在单个场景中处理巨大的比例差异，就有必要使用。 Note that this setting uses gl_FragDepth if available which disables the Early Fragment Test optimization and can cause a decrease in performance. 默认是false。 示例：camera / logarithmicdepthbuffer

##### 属性

- `.autoClear` : Boolean
定义渲染器是否在渲染每一帧之前自动清除其输出。

- `.autoClearColor` : Boolean
如果autoClear为true, 定义renderer是否清除颜色缓存。 默认是true

- `.autoClearDepth` : Boolean
如果autoClear是true, 定义renderer是否清除深度缓存。 默认是true

- `.autoClearStencil` : Boolean
如果autoClear是true, 定义renderer是否清除模板缓存. 默认是true

- `.debug` : Object

  - checkShaderErrors: 如果为true，定义是否检查材质着色器程序 编译和链接过程中的错误。 禁用此检查生产以获得性能增益可能很有用。 强烈建议在开发期间保持启用这些检查。 如果着色器没有编译和链接 - 它将无法工作，并且相关材料将不会呈现。 默认是true
  - onShaderError( gl, program, glVertexShader, glFragmentShader ): A callback function that can be used for custom error reporting. The callback receives the WebGL context, an instance of WebGLProgram as well two instances of WebGLShader representing the vertex and fragment shader. Assigning a custom function disables the default error reporting. Default is null.

- `.capabilities` : Object
一个包含当前渲染环境(RenderingContext)的功能细节的对象。

  - floatFragmentTextures: 环境是否支持OES_texture_float扩展
  - floatVertexTextures: 如果floatFragmentTextures和vertexTextures都是true， 则此值为true
  - getMaxAnisotropy(): 返回最大可用各向异性。
  - getMaxPrecision(): 返回顶点着色器和片元着色器的最大可用精度。
  - isWebGL2: true if the context in use is a WebGL2RenderingContext object.
  - logarithmicDepthBuffer: 如果logarithmicDepthBuffer在构造器中被设为true且 环境支持EXT_frag_depth扩展，则此值为true
  - maxAttributes: gl.MAX_VERTEX_ATTRIBS的值
  - maxCubemapSize: gl.MAX_CUBE_MAP_TEXTURE_SIZE 的值，着色器可使用的立方体贴图纹理的最大宽度*高度
  - maxFragmentUniforms: gl.MAX_FRAGMENT_UNIFORM_VECTORS的值，片元着色器可使用的全局变量(uniforms)数量
  - maxSamples: The value of gl.MAX_SAMPLES. Maximum number of samples in context of Multisample anti-aliasing (MSAA).
  - maxTextureSize: gl.MAX_TEXTURE_SIZE的值，着色器可使用纹理的最大宽度*高度
  - maxTextures: *gl.MAX_TEXTURE_IMAGE_UNITS的值，着色器可使用的纹理数量
  - maxVaryings: gl.MAX_VARYING_VECTORS的值，着色器可使用矢量的数量
  - maxVertexTextures: gl.MAX_VERTEX_TEXTURE_IMAGE_UNITS的值，顶点着色器可使用的纹理数量。
  - maxVertexUniforms: gl.MAX_VERTEX_UNIFORM_VECTORS的值，顶点着色器可使用的全局变量(uniforms)数量
  - precision: 渲染器当前使用的着色器的精度
  - vertexTextures: 如果 .maxVertexTextures : Integer大于0，此值为true (即可以使用顶点纹理)
- `.clippingPlanes` : Array
用户自定义的剪裁平面，在世界空间中被指定为THREE.Plane对象。 这些平面全局使用。空间中与该平面点积为负的点将被切掉。 默认值是[]

- `.domElement` : DOMElement
一个canvas，渲染器在其上绘制输出。
渲染器的构造函数会自动创建(如果没有传入canvas参数);你需要做的仅仅是像下面这样将它加页面里去:`document.body.appendChild( renderer.domElement );`
- `.extensions` : Object

  - get( extensionName : String ): 用于检查是否支持各种扩展，并返回一个对象，其中包含扩展的详细信息。 该方法检查以下扩展：
WEBGL_depth_texture
EXT_texture_filter_anisotropic
WEBGL_compressed_texture_s3tc
WEBGL_compressed_texture_pvrtc
WEBGL_compressed_texture_etc1
.outputColorSpace : string
定义渲染器的输出编码。默认为THREE.SRGBColorSpace

如果渲染目标已经使用 .setRenderTarget、之后将直接使用renderTarget.texture.colorSpace

查看texture constants页面以获取其他格式细节

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
定义渲染器是否考虑对象级剪切平面。 默认为false.

- `.useLegacyLights` : Boolean
Whether to use the legacy lighting mode or not. 默认是true。

- `.properties` : Object
渲染器内部使用，以跟踪各种子对象属性。

- `.renderLists` : WebGLRenderLists
在内部用于处理场景渲染对象的排序。

- `.shadowMap` : WebGLShadowMap
如果使用，它包含阴影贴图的引用。

  - enabled: 如果设置开启，允许在场景中使用阴影贴图。默认是 false。
  - autoUpdate: 启用场景中的阴影自动更新。默认是true
如果不需要动态光照/阴影, 则可以在实例化渲染器时将之设为false
  - needsUpdate: 当被设为true, 场景中的阴影贴图会在下次render调用时刷新。默认是false
如果你已经禁用了阴影贴图的自动更新(shadowMap.autoUpdate = false), 那么想要在下一次渲染时更新阴影的话就需要将此值设为true
  - type: 定义阴影贴图类型 (未过滤, 关闭部分过滤, 关闭部分双线性过滤), 可选值有:

THREE.BasicShadowMap
THREE.PCFShadowMap (默认)
THREE.PCFSoftShadowMap
THREE.VSMShadowMap
详见Renderer constants

- `.sortObjects` : Boolean
定义渲染器是否应对对象进行排序。默认是true.

说明: 排序用于尝试正确渲染出具有一定透明度的对象。根据定义，排序可能不总是有用。根据应用的需求，可能需要关闭排序并使其他方法来处理透明度的渲染，例如， 手动确定每个对象的渲染顺序。

- `.state` : Object
包含设置WebGLRenderer.context状态的各种属性的函数。

- `.toneMapping` : Constant
默认是NoToneMapping。查看Renderer constants以获取其它备选项

- `.toneMappingExposure` : Number
色调映射的曝光级别。默认是1

-`.xr` : WebXRManager
Provides access to the WebXR related interface of the renderer.

##### 方法

- `.render ( scene : Object3D, camera : Camera ) : undefined`
用相机(camera)渲染一个场景(scene)或是其它类型的object。
渲染一般是在canvas上完成的，或者是renderTarget(如果有指定)
如果forceClear值是true，那么颜色、深度及模板缓存将会在渲染之前清除，即使渲染器的autoClear属性值是false
即便forceClear设为true, 也可以通过将autoClearColor、autoClearStencil或autoClearDepth属性的值设为false来阻止对应缓存被清除。

- `.setSize ( width : Integer, height : Integer, updateStyle : Boolean ) : undefined`
将输出canvas的大小调整为(width, height)并考虑设备像素比，且将视口从(0, 0)开始调整到适合大小 将updateStyle设置为false以阻止对canvas的样式做任何改变。

- `.setViewport ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined`
将视口大小设置为(x, y)到 (x + width, y + height).

- `.clear ( color : Boolean, depth : Boolean, stencil : Boolean ) : undefined`
告诉渲染器清除颜色、深度或模板缓存. 此方法将颜色缓存初始化为当前颜色。参数们默认都是true

- `.clearColor ( ) : undefined`
清除颜色缓存。 相当于调用.clear( true, false, false )

- `.clearDepth ( ) : undefined`
清除深度缓存。相当于调用.clear( false, true, false )

- `.clearStencil ( ) : undefined`
清除模板缓存。相当于调用.clear( false, false, true )

- `.compile ( scene : Object3D, camera : Camera ) : undefined`
使用相机编译场景中的所有材质。这对于在首次渲染之前预编译着色器很有用。

- `.copyFramebufferToTexture ( position : Vector2, texture : FramebufferTexture, level : Number ) : undefined`
将当前WebGLFramebuffer中的像素复制到2D纹理中。可访问WebGLRenderingContext.copyTexImage2D.

- `.copyTextureToTexture ( position : Vector2, srcTexture : Texture, dstTexture : Texture, level : Number ) : undefined`
将纹理的所有像素复制到一个已有的从给定位置开始的纹理中。可访问WebGLRenderingContext.texSubImage2D.

- `.dispose ( ) : undefined`
处理当前的渲染环境

- `.forceContextLoss () : undefined`
模拟WebGL环境的丢失。需要支持 WEBGL_lose_context 扩展才能用。

- `.forceContextRestore ( ) : undefined`
模拟WebGL环境的恢复。需要支持 WEBGL_lose_context 扩展才能用。

- `.getClearAlpha () : Float`
返回一个表示当前alpha值的float，范围0到1

- `.getClearColor ( target : Color ) : Color`
返回一个表示当前颜色值的THREE.Color实例

- `.getContext () : WebGL2RenderingContext`
返回当前WebGL环境

- `.getContextAttributes () : WebGLContextAttributes`
返回一个对象，这个对象中存有在WebGL环境在创建的时候所设置的属性

- `.getActiveCubeFace () : Integer`
Returns the current active cube face.

- `.getActiveMipmapLevel () : Integer`
Returns the current active mipmap level.

- `.getRenderTarget () : RenderTarget`
如果当前存在RenderTarget，则返回该值；否则返回null。

- `.getCurrentViewport () : RenderTarget`
返回当前视口

- `.getDrawingBufferSize () : Object`
返回一个包含渲染器绘图缓存宽度和高度(单位像素)的对象。

- `.getPixelRatio () : number`
返回当前使用设备像素比

- `.getSize ( target : Vector2 ) : Vector2`
返回包含渲染器输出canvas的宽度和高度(单位像素)的对象。

- `.initTexture ( texture : Texture ) : undefined`
初始化给定的纹理。用于预加载纹理而不是等到第一次渲染（可能会由于解码和 GPU 上传的开销而导致明显的延迟）.

- `.resetGLState ( ) : undefined`
将GL状态重置为默认值。WebGL环境丢失时会内部调用

- `.readRenderTargetPixels ( renderTarget : WebGLRenderTarget, x : Float, y : Float, width : Float, height : Float, buffer : TypedArray, activeCubeFaceIndex : Integer ) : undefined`
buffer - Uint8Array is the only destination type supported in all cases, other types are renderTarget and platform dependent. See WebGL spec for details.

将renderTarget中的像素数据读取到传入的缓冲区中。这是WebGLRenderingContext.readPixels()的包装器
示例：interactive / cubes / gpu

For reading out a WebGLCubeRenderTarget use the optional parameter activeCubeFaceIndex to determine which face should be read.

- `.resetState () : undefined`
可用于重置内部 WebGL 状态。此方法主要与跨多个 WebGL 库共享单个 WebGL 上下文的应用程序相关。

- `.setAnimationLoop ( callback : Function ) : undefined`
callback — 每个可用帧都会调用的函数。 如果传入‘null’,所有正在进行的动画都会停止。

可用来代替requestAnimationFrame的内置函数. 对于WebXR项目，必须使用此函数。

- `.setClearAlpha ( alpha : Float ) : undefined`
设置alpha。合法参数是一个0.0到 1.0之间的浮点数

- `.setClearColor ( color : Color, alpha : Float ) : undefined`
设置颜色及其透明度

- `.setPixelRatio ( value : number ) : undefined`
设置设备像素比。通常用于避免HiDPI设备上绘图模糊

- `.setRenderTarget ( renderTarget : WebGLRenderTarget, activeCubeFace : Integer, activeMipmapLevel : Integer ) : undefined`
renderTarget -- 需要被激活的renderTarget(可选)。若此参数为空，则将canvas设置成活跃render target。
activeCubeFace -- Specifies the active cube side (PX 0, NX 1, PY 2, NY 3, PZ 4, NZ 5) of WebGLCubeRenderTarget. When passing a WebGLArrayRenderTarget or WebGL3DRenderTarget this indicates the z layer to render in to (optional).
activeMipmapLevel -- Specifies the active mipmap level (optional).

该方法设置活跃rendertarget。

- `.setScissor ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined`
将剪裁区域设为(x, y)到(x + width, y + height) Sets the scissor area from

- `.setScissorTest ( boolean : Boolean ) : undefined`
启用或禁用剪裁检测. 若启用，则只有在所定义的裁剪区域内的像素才会受之后的渲染器影响。

#### WebGL1Renderer

自r118起，WebGLRenderer会自动使用 WebGL 2 渲染上下文。当升级一个已存在的项目到 => r118 ， 应用程序可能会因为下列两种原因而损坏：

自定义着色器代码必须符合GLSL 3.0。
WebGL 1扩展检查到被更改
如果你不能够花时间来升级你的代码，但仍然想要使用最新版本，那你就可以使用WebGL1Renderer。 这一版本的渲染器会强制使用 WebGL 1 渲染上下文。
构造函数
WebGL1Renderer( parameters : Object )
Creates a new WebGL1Renderer.

#### SVG渲染器（SVGRenderer）

SVGRenderer被用于使用SVG来渲染几何数据，所产生的矢量图形在以下几个方面十分有用：

动画标志（logo）或者图标（icon）
可交互的2D或3D图表或图形
交互式地图
复杂的或包含动画的用户界面
SVGRenderer具有很多优势。它产生清晰并且锐利的图像输出，它和实际视口分辨率无关。
SVG元素可以通过CSS来控制样式；并且由于它可以添加诸如标题或者描述文字之类的元数据（对于搜索引擎或者屏幕阅读器十分有用），因此它具有十分良好的可访问性。

然而，SVG也有一些十分重要的限制：

没有高级的着色器
不支持纹理
不支持阴影

SVGRenderer 是一个附加组件，必须显式导入。 See Installation / Addons.

`import { SVGRenderer } from 'three/addons/renderers/SVGRenderer.js';`

#### CSS 3D渲染器（CSS3DRenderer）

CSS3DRenderer用于通过CSS3的transform属性， 将层级的3D变换应用到DOM元素上。 如果你希望不借助基于canvas的渲染来在你的网站上应用3D变换，那么这一渲染器十分有趣。 同时，它也可以将DOM元素与WebGL的内容相结合。

然而，这一渲染器也有一些十分重要的限制：

它不可能使用three.js中的材质系统。
同时也不可能使用几何体。
CSS3DRenderer 仅支持100%的浏览器和显示缩放。
因此，CSS3DRenderer仅仅关注普通的DOM元素，这些元素被包含到了特殊的对象中（CSS3DObject或者CSS3DSprite），然后被加入到场景图中。
进口
CSS3DRenderer 是一个附加组件，必须显式导入。 See Installation / Addons.

`import { CSS3DRenderer } from 'three/addons/renderers/CSS3DRenderer.js';`

### 场景

#### Scene

场景能够让你在什么地方、摆放什么东西来交给three.js来渲染，这是你放置物体、灯光和摄像机的地方。

##### 构造器

Scene()
创建一个新的场景对象。

##### 属性

.background : Object
若不为空，在渲染场景的时候将设置背景，且背景总是首先被渲染的。 可以设置一个用于的“clear”的Color（颜色）、一个覆盖canvas的Texture（纹理）， 或是 a cubemap as a CubeTexture or an equirectangular as a Texture。默认值为null。

.backgroundBlurriness : Float
Sets the blurriness of the background. Only influences environment maps assigned to Scene.background. Valid input is a float between 0 and 1. Default is 0.

.environment : Texture
若该值不为null，则该纹理贴图将会被设为场景中所有物理材质的环境贴图。 然而，该属性不能够覆盖已存在的、已分配给 MeshStandardMaterial.envMap 的贴图。默认为null。

.fog : Fog
一个fog实例定义了影响场景中的每个物体的雾的类型。默认值为null。

.isScene : Boolean

.overrideMaterial : Material
如果不为空，它将强制场景中的每个物体使用这里的材质来渲染。默认值为null。

##### 方法

.toJSON : Object
meta -- 包含有元数据的对象，例如场景中的的纹理或图片。 将scene对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 雾（Fog）

这个类中的参数定义了线性雾。也就是说，雾的密度是随着距离线性增大的。

##### 构造器

Fog( color : Integer, near : Float, far : Float )
颜色参数传入Color构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是CSS风格的字符串。

##### 属性

.isFog : Boolean
Read-only flag to check if a given object is of type Fog.

.name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

.color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

.near : Float
开始应用雾的最小距离。距离小于活动摄像机“near”个单位的物体将不会被雾所影响。

默认值是1。

.far : Float
结束计算、应用雾的最大距离，距离大于活动摄像机“far”个单位的物体将不会被雾所影响。

默认值是1000。

##### 方法

.clone () : Fog
返回一个具有和当前雾参数相同的新的Fog实例。

.toJSON () : Object
以JSON格式返回Fog的数据。

#### 雾-指数（FogExp2）

该类所包含的参数定义了指数雾，它可以在相机附近提供清晰的视野，且距离相机越远，雾的浓度随着指数增长越快。

##### 构造器

FogExp2( color : Integer, density : Float )
颜色参数传入Color构造函数中，来设置颜色属性。颜色可以是一个十六进制的整型数，或者是CSS风格的字符串。

##### 属性

.isFogExp2 : Boolean
Read-only flag to check if a given object is of type FogExp2.

.name : String
对象的名称，可选、不必唯一。默认值是一个空字符串。

.color : Color
雾的颜色。比如说，如果将其设置为黑色，远处的物体将被渲染成黑色。

.density : Float
定义雾的密度将会增长多块。

默认值是0.00025.

##### 方法

.clone () : FogExp2
返回一个具有和当前雾参数相同的新的FogExp2实例。

.toJSON () : Object
以JSON格式返回FogExp2的数据。

### 光源

#### Light

##### 构造器（Constructor）

`Light( color : Integer, intensity : Float )`
color - (可选参数) 16进制表示光的颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。

创造一个新的光源。注意，这并不是直接调用的（而是使用派生类之一）。

##### 属性（Properties）

公共属性请查看基类Object3D。

.color : Color
光源的颜色。如果构造的时候没有传递，默认会创建一个新的 Color 并设置为白色。

.intensity : Float
光照的强度，或者说能量。 在 physically correct 模式下, color 和强度 的乘积被解析为以坎德拉（candela）为单位的发光强度。 默认值 - 1.0
.isLight : Boolean
Read-only flag to check if a given object is of type Light.

##### Methods

公共方法请查看基类 Object3D。

.copy ( source : Light ) : this
从source复制 color, intensity 的值到当前光源对象中。

.toJSON ( meta : Object ) : Object
以JSON格式返回光数据。

meta -- 包含有元数据的对象，例如该对象的材质、纹理或图片。 将该light对象转换为 three.js JSON Object/Scene format（three.js JSON 物体/场景格式）。

#### 环境光AmbientLight

环境光会均匀的照亮场景中的所有物体。环境光不能用来投射阴影，因为它没有方向。

```js
const light = new THREE.AmbientLight( 0x404040 ); // soft white light
scene.add( light );
```

构造函数
`AmbientLight( color : Integer, intensity : Float )`
color - （参数可选）颜色的rgb数值。缺省值为 0xffffff。
intensity - (参数可选)光照的强度。缺省值为 1。

创建一个环境光对象。

##### 属性|方法

>公共属性请查看基类 Light。
>公共方法请查看基类Light。

.castShadow : Boolean
这个参数在对象构造的时候就被设置成了 undefined 。因为环境光不能投射阴影。

.isAmbientLight : Boolean
Read-only flag to check if a given object is of type AmbientLight.

#### 平行光（DirectionalLight）

平行光是沿着特定方向发射的光。这种光的表现像是无限远,从它发出的光线都是平行的。常常用平行光来模拟太阳光 的效果;

```js
const directionalLight = new THREE.DirectionalLight( 0xffffff, 0.5 );
scene.add( directionalLight );
```

##### 构造函数

`DirectionalLight( color : Integer, intensity : Float )`
color - (可选参数) 16进制表示光的颜色。 缺省值为 0xffffff (白色)。
intensity - (可选参数) 光照的强度。缺省值为1。

##### 属性|方法

>公共属性请查看基类 Light。
>公共方法请查看基类Light。

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
这使得属性target中的 matrixWorld 会每帧自动更新。

它也可以设置target为场景中的其他对象(任意拥有 position 属性的对象), 示例如下:

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
const light = new THREE.PointLight( 0xff0000, 1, 100 );
light.position.set( 50, 50, 50 );
scene.add( light );
```

##### 构造函数

`PointLight( color : Integer, intensity : Float, distance : Number, decay : Float )`
color - (可选参数)) 十六进制光照颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。
distance - 这个距离表示从光源到光照强度为0的位置。 当设置为0时，光永远不会消失(距离无穷大)。缺省值 0.
decay - 沿着光照距离的衰退量。缺省值 2。

##### 属性|方法

>公共属性请查看基类 Light。
>公共方法请查看基类Light。

.castShadow : Boolean
如果设置为 true 该平行光会产生动态阴影。 警告: 这样做的代价比较高而且需要一直调整到阴影看起来正确. 查看 DirectionalLightShadow 了解详细信息。该属性默认为 false。
.distance : Float
如果非零，那么光强度将会从最大值当前灯光位置处按照距离线性衰减到0。 缺省值为 0.0。

.power : Float
光功率
在 physically correct 模式中, 表示以"流明（光通量单位）"为单位的光功率。 缺省值 - 4Math.PI。

该值与 intensity 直接关联
`power = intensity * 4π`修改该值也会导致光强度的改变。

.shadow : PointLightShadow
PointLightShadow用与计算此光照的阴影。

此对象的摄像机被设置为 fov 为90度，aspect为1， 近裁剪面 near 为0，远裁剪面far 为500的透视摄像机 PerspectiveCamera。

.decay : Float
The amount the light dims along the distance of the light. Default is 2.
In context of physically-correct rendering the default value should not be changed.

##### 方法

`.copy ( source : PointLight ) : this`
将所有属性的值从源 source 复制到此点光源对象。

#### 聚光灯（SpotLight）

光线从一个点沿一个方向射出，随着光线照射的变远，光线圆锥体的尺寸也逐渐增大。该光源可以投射阴影

```js
const spotLight = new THREE.SpotLight( 0xffffff );
spotLight.position.set( 100, 1000, 100 );
spotLight.map = new THREE.TextureLoader().load( url );

spotLight.castShadow = true;

spotLight.shadow.mapSize.width = 1024;
spotLight.shadow.mapSize.height = 1024;

spotLight.shadow.camera.near = 500;
spotLight.shadow.camera.far = 4000;
spotLight.shadow.camera.fov = 30;

scene.add( spotLight );

```

##### 构造函数

`SpotLight( color : Integer, intensity : Float, distance : Float, angle : Radians, penumbra : Float, decay : Float )`
color - (可选参数) 十六进制光照颜色。 缺省值 0xffffff (白色)。
intensity - (可选参数) 光照强度。 缺省值 1。
distance - 从光源发出光的最大距离，其强度根据光源的距离线性衰减。
angle - 光线散射角度，最大为Math.PI/2。
penumbra - 聚光锥的半影衰减百分比。在0和1之间的值。默认为0。
decay - 沿着光照距离的衰减量。

##### 属性|方法

>公共属性请查看基类 Light。
>公共方法请查看基类Light。

.angle : Float
从聚光灯的位置以弧度表示聚光灯的最大范围。应该不超过 Math.PI/2。默认值为 Math.PI/3。

.castShadow : Boolean
此属性设置为 true 聚光灯将投射阴影。警告: 这样做的代价比较高而且需要一直调整到阴影看起来正确。 查看 SpotLightShadow 了解详细信息。 默认值为 false

.decay : Float
The amount the light dims along the distance of the light. Default is 2.
In context of physically-correct rendering the default value should not be changed.

.distance : Float
如果非零，那么光强度将会从最大值当前灯光位置处按照距离线性衰减到0。 缺省值为 0.0。

.isSpotLight : Boolean
Read-only flag to check if a given object is of type SpotLight.

.penumbra : Float
聚光锥的半影衰减百分比。在0和1之间的值。 默认值 — 0.0。

.position : Vector3
假如这个值设置等于 Object3D.DEFAULT_UP (0, 1, 0)，那么光线将会从上往下照射。

.power : Float
光功率
在 physically correct 模式中， 表示以"流明（光通量单位）"为单位的光功率。 缺省值 - 4Math.PI。

该值与 intensity 直接关联
power = intensity * 4π修改该值也会导致光强度的改变。

.shadow : SpotLightShadow
SpotLightShadow用与计算此光照的阴影。

.target : Object3D
聚光灯的方向是从它的位置到目标位置.默认的目标位置为原点 (0,0,0)。
注意: 对于目标的位置，要将其更改为除缺省值之外的任何位置，它必须被添加到 scene 场景中去。
scene.add( light.target );这使得属性target中的 matrixWorld 会每帧自动更新。

它也可以设置target为场景中的其他对象(任意拥有 position 属性的对象), 示例如下:
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
你必须在你的场景中加入 RectAreaLightUniformsLib ，并调用init()。

### 三维坐标系

AxesHelper的xyz轴
three.js坐标轴颜色红R、绿G、蓝B分别对应坐标系的x、y、z轴，对于three.js的3D坐标系默认y轴朝上。

```js
// AxesHelper：辅助观察的坐标系
const axesHelper = new THREE.AxesHelper(150);
scene.add(axesHelper);
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
