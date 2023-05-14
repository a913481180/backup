---
title: MATLAB 图像的空间变换及相邻区域和块的处理
date: 2019-02-23 20:54:12
categories:
- experiments
---


# MATLAB ****图像的空间变换及相邻区域和块的处理**

## 一、目的

1. 掌握MATLAB的基本应用方法
2. 熟悉MATLAB的4个内置函数：clc, clear, close, cputime
3. 掌握MATLAB空间变换--图像插值、缩放、旋转
4. 掌握MATLAB相邻区域和块的处理-滑动邻域和图像块操作

## 二、仪器设备、软件

仪器设备： 计算机

操作系统：Windows10

编程软件：MATLAB R2019a

截图软件：截图工具(SnippingTool.exe)

## 三、内容及步骤(参考MATLAB 图像处理工具箱的帮助完成相应功能)

**1.**  **了解图像插值的方法和应用**

在图像缩放、旋转中MATLAB图像处理工具箱提供了以下三种插值方法：

**&#39;nearest&#39;**

nearest neighbor interpolation （最邻近插值）

对于最邻近插值来说，输出像素被赋值为当前点落入的像素点的值。

使用方法：

x=(-3.5:3.5)&#39;;

[x convergent(x) nearest(x) round(x)]

**&#39;bilinear&#39;**

bilinear interpolation（双线性插值）

对于双线性插值来说，输出像素被赋值为2×2矩阵所包含的有效点的加权平均值。

使用方法：

% Design a 6-th order Elliptic analog low pass filter and transform

% it to a Discrete-time representation.

Fs =0.5; % Sampling Frequency

[z,p,k]=ellipap(6,5,90); % Lowpass filter prototype

[num,den]=zp2tf(z,p,k); % Convert to transfer function form

[numd,dend]=bilinear(num,den,Fs); % Analog to Digital conversion

fvtool(numd,dend) % Visualize the filter

**&#39;bicubic&#39;**

bicubic interpolation（双立次插值）

对于双立次插值来说，输出像素被赋值为4×4矩阵所包含的有效点的加权平均值。

使用方法：

%双三次插值具体实现

 clc,clear; fff=imread(&#39;E:\Documents\BUPT\DIP\图片\lena.bmp&#39;);

 ff = rgb2gray(fff);%转化为灰度图像

 [mm,nn]=size(ff);               %将图像隔行隔列抽取元素，得到缩小的图像f m=mm/2;n=nn/2;

f = zeros(m,n);

 for i=1:m

 for j=1:n

 f(i,j)=ff(2\*i,2\*j);

 end

end

k=5;                       %设置放大倍数

 bijiao1 = imresize(f,k,&#39;bilinear&#39;);%双线性插值结果比较

bijiao = uint8(bijiao1);

 a=f(1,:);c=f(m,:);             %将待插值图像矩阵前后各扩展两行两列,共扩展四行四列

b=[f(1,1),f(1,1),f(:,1)&#39;,f(m,1),f(m,1)];

d=[f(1,n),f(1,n),f(:,n)&#39;,f(m,n),f(m,n)];

 a1=[a;a;f;c;c];

 b1=[b;b;a1&#39;;d;d];

ffff=b1&#39;;

f1=double(ffff);

 g1 = zeros(k\*m,k\*n);

 for i=1:k\*m                 %利用双三次插值公式对新图象所有像素赋值

   u=rem(i,k)/k; i1=floor(i/k)+2;

A=[sw(1+u) sw(u) sw(1-u) sw(2-u)];

      for j=1:k\*n

    v=rem(j,k)/k;

j1=floor(j/k)+2;

      C=[sw(1+v);sw(v);sw(1-v);sw(2-v)];

      B=[f1(i1-1,j1-1) f1(i1-1,j1) f1(i1-1,j1+1) f1(i1-1,j1+2)

     f1(i1,j1-1)   f1(i1,j1)   f1(i1,j1+1)   f1(i1,j1+2)

    f1(i1+1,j1-1)  f1(i1+1,j1) f1(i1+1,j1+1) f1(i1+1,j1+2)

   f1(i1+2,j1-1) f1(i1+2,j1) f1(i1+2,j1+1) f1(i1+2,j1+2)];

      g1(i,j)=(A\*B\*C);

end

end

g=uint8(g1);

 imshow(uint8(f));

 title(&#39;缩小的图像&#39;);    %显示缩小的图像

Figure

,imshow(ff);

title(&#39;原图&#39;);               %显示原图像

figure,

imshow(g);

title(&#39;双三次插值放大的图像&#39;);     %显示插值后的图像

figure,imshow(bijiao);

title(&#39;双线性插值放大结果&#39;);     %显示插值后的图像

mse=0;

ff=double(ff);

g=double(g);

 ff2=fftshift(fft2(ff));                     %计算原图像和插值图像的傅立叶幅度谱

 g2=fftshift(fft2(g));

figure,subplot(1,2,1),imshow(log(abs(ff2)),[8,10]);

title(&#39;原图像的傅立叶幅度谱&#39;);

subplot(1,2,2),imshow(log(abs(g2)),[8,10]);

title(&#39;双三次插值图像的傅立叶幅度谱&#39;);

function A=sw(w1) w=abs(w1);

if w\&lt;1&amp;&amp;w\&gt;=0

   A=1-2\*w^2+w^3;

 elseif w\&gt;=1&amp;&amp;w\&lt;2

A=4-8\*w+5\*w^2-w^3;

 Else

A=0;

 end

**2.**  **了解图像相邻区域和块的处理**** - ****滑动窗和图像块操作**

**3.**** 熟悉 ****MATLAB**** 的 ****4**** 个内置函数： ****clc, clear, close, cputime**

1) clc - Clear Command Window（清除命令行窗口）

2) clear - Remove items from workspace, freeing up system memory

（删除工作区中的内容，释放系统存储器）

3) close - Close figure(关闭图形窗口)

close ALL - closes all the open figure windows(关闭所有打开的图形窗口)

4) cputime - CPU time in seconds(以秒为单位的CPU时间)

cputime returns the CPU time in seconds that has been used

by the MATLAB process since MATLAB started.

cputime返回自从MATLAB打开MATLAB进程使用的以秒为单位的CPU时间

For example:

t=cputime; your\_operation; cputime-t

returns the cpu time used to run your\_operation.

读取原始图像后，图像处理前调用一次cputime赋值给变量t，图像处理后再调用一次cputime减去图像处理前CPU时间t，即为运行图像处理操作所花费的时间。以此可以评测图像处理算法的时间复杂度。

**4.**  **放大和缩小一幅图像**** (imresize)**

B=imresize(A, SCALE, METHOD)

B=imresize(A, [NUMROWS NUMCOLS], METHOD)

放大和缩小后图像的分辨率不一样，不要用同一个图形窗口内的subplot （子图）显示原图像、放大和缩小后的图像。显示每一幅图像前，加figure (创建新的图形窗口)。

**使用方法：**

B = imresize(A,scale)

[B](https://ww2.mathworks.cn/help/matlab/ref/imresize.html?searchHighlight=imresize&amp;s_tid=doc_srchtitle#d117e606348) = imresize([A](https://ww2.mathworks.cn/help/matlab/ref/imresize.html?searchHighlight=imresize&amp;s_tid=doc_srchtitle#d117e605577),[scale](https://ww2.mathworks.cn/help/matlab/ref/imresize.html?searchHighlight=imresize&amp;s_tid=doc_srchtitle#d117e605629)) 返回图像 B，它是将 A 的长宽大小缩放 scale 倍之后的图像。输入图像 A 可以是灰度、RGB 或二值图像。如果 A 有两个以上维度，则 imresize 只调整前两个维度的大小。如果 scale 在 [0, 1] 范围内，则 B 比 A 小。如果 scale 大于 1，则 B 比 A 大。默认情况下，imresize 使用双三次插值。

举例：

I = imread(&#39;ngc6543a.jpg&#39;);
 %将图像的长宽缩小二分之一。
 J = imresize(I, 0.5);
 %显示原始图像和调整大小后的图像。
 figure(2); imshow(I);
 figure(3); imshow(J);

综上所述，在MATLAB Editor（编辑器），新建一个文件，编写代码如下：

clc,clear;close all;

A=imread(&#39;baby.jpg&#39;);

%将图像的长宽缩小十分之一。

B =imresize(A,0.1);

%将图像的长宽放大二分之一。

C =imresize(A,2);

%显示图像

figure

subplot(111);

imshow(A);

title(&#39;原图&#39;);

figure

subplot(111);

imshow(B);

title(&#39;缩小&#39;);

figure

subplot(111);

imshow(C);

title(&#39;放大&#39;);

点击MATLAB Editor（编辑器）的运行按钮，可看到工作区内容以及运行结果

1. **旋转一幅图像**** (imrotate)**

B = imrotate(A,ANGLE,METHOD)

B = imrotate(A,ANGLE,METHOD,BBOX)

**使用方法：**

B = imrotate(A,angle)
将图像A（图像的数据矩阵）绕图像的中心点旋转angle度， 正数表示逆时针旋转， 负数表示顺时针旋转。返回旋转后的图像矩阵。

B = imrotate(A,angle,method)
使用method参数可以改变插值算法，method参数可以为下面这三个值：需要引号引出，默认值用大括号{}括起来。
 &#39;nearest&#39;：最邻近线性插值（Nearest-neighbor interpolation）
 &#39;bilinear&#39;： 双线性插值（Bilinear interpolation）
 &#39;bicubic&#39;： 双三次插值（或叫做双立方插值）（Bicubic interpolation）

B = imrotate(A,angle,method,bbox)
 bbox参数用于指定输出图像属性：
 &#39;crop&#39;： 通过对旋转后的图像B进行裁剪， 保持旋转后输出图像B的尺寸和输入图像A的尺寸一样。
 &#39;loose&#39;： 使输出图像足够大， 以保证源图像旋转后超出图像尺寸范围的像素值没有丢失。 一般这种格式产生的图像的尺寸都要大于源图像的尺寸。

综上所述，在MATLAB Editor（编辑器），新建一个文件，编写代码如下：

clc,clear;close all;

A=imread(&#39;baby.jpg&#39;);

%顺时针旋转60度

B = imrotate(A,60);

%顺时针旋转30度,双线性插值法

C = imrotate(A,30,&#39;bilinear&#39;);

%顺时针旋转15度,最邻近线性插值法

D = imrotate(A,15,&#39;nearest&#39;);

%逆时针旋转60度,双三次插值法

E = imrotate(A,-60,&#39;bicubic&#39;);

%指定输出图像属性

%通过对旋转后的图像B进行裁剪，

%逆时针旋转30度,旋转后输出图像B的尺寸和输入图像A的尺寸一样

F = imrotate(A,-30,&#39;bilinear&#39;,&#39;crop&#39;);

%逆时针旋转15度,使输出图像足够大，

G = imrotate(A,-15,&#39;bilinear&#39;,&#39;crop&#39;);

% %显示图像

figure

subplot(111);

imshow(A);

title(&#39;原图&#39;);

figure

subplot(111);

imshow(B);

title(&#39;顺时针旋转60度&#39;);

figure

subplot(111);

imshow(C);

title(&#39;顺时针旋转30度,双线性插值法&#39;);

figure

subplot(111);

imshow(D);

title(&#39;顺时针旋转15度,最邻近线性插值法&#39;);

figure

subplot(111);

imshow(E);

title(&#39;逆时针旋转60度,双三次插值法&#39;);

figure

subplot(111);

imshow(F);

title(&#39;逆时针旋转30度,旋转后输出图像B的尺寸和输入图像A的尺寸一样&#39;);

imshow(G);

title(&#39;逆时针旋转30度,旋转后使输出图像足够大&#39;);

点击MATLAB Editor（编辑器）的运行按钮，可看到工作区内容以及运行结果

**6.**  **滑动邻域操作**** (nlfilter)**

B = nlfilter(A,[M N],FUN)

FUN必须为一个函数句柄

B = nlfilter(A,[m n],fun)

使用对A使用滤波函数，滤波函数的形式为fun

tips:nlfilter在较大的图像中操作时间较长，可以使用colfilt函数代替该函数

综上所述，在MATLAB Editor（编辑器），新建一个文件，编写代码如下：

clc,clear;close all;

A=imread(&#39;coins.png&#39;);

%滤波函数

fun = @(x) median(x(:));

%对A使用滤波函数，滤波函数的形式为fun

B = nlfilter(A,[3 3],fun);

%显示图像

suplot(111),

imshow(A),

title(&#39;原图&#39;);

figure,

suplot(111),

imshow(B),

title(&#39;处理后图像&#39;);

点击MATLAB Editor（编辑器）的运行按钮，可看到工作区内容以及运行结果

**7.**  **图像块操作** **(blockproc)**

使用8-by-8 blocks DCT 变换对图像进行压缩与解压缩，利用dctmtx, blockproc等函数，更改mask 矩阵的值观察解压缩重构图像的变化（区域编码mask矩阵值只能为0或1，1表示选取，0表示舍弃）。

B = blockproc(A,[m n],fun) 通过对大小为 [m n] 的每个非重叠图像块应用 fun 函数并将结果串联到输出图像 B 中，来处理图像 A。

B = blockproc(src\_filename,[m n],fun) 处理文件名为 src\_filename 的图像，一次读取和处理一个图像块。此语法对于处理大型图像很有用。

B = blockproc(adapter,[m n],fun) 处理由 adapter（一个 ImageAdapter 对象）指定的源图像。如果您需要自定义 API 来读写特定图像文件格式，请使用此语法。

blockproc(\_\_\_,Name,Value) 使用名称-值对组参数来控制图像块行为的各个方面。

通 ![](RackMultipart20210826-4-1uatq9n_html_830d1655bcbf2565.png)过改变模板矩阵中非零元素的个数，得到不同缩编码图像，编写程序计算原图像和dct变换后得到的图像之间的均方误差。用到的matlab函数为im2double,dctmtx,blkproc，对比还是容易发现，第二个代码的运行结果图片损失程度还是比较大的，因程序二模板的0个数占比更多，使更多的高频像素被压缩，而该压缩是不可逆的，所以图二损失程度更大。

MATLAB图像处理工具箱实现离散余弦变换有两种方法：dct2和dctmtx（1）使用函数dct2，该函数用一个基于FFT的算法来提高当输入较大的方阵时的计算速度。

（2）使用由dctmtx函数返回的DCT变换矩阵，这种方法较适合于较小的输入方阵（例如8×8或16×16）。

①函数：dct2

实现图像的二维离散余弦变换。调用格式为：

B = dct2(A)

B = dct2(A,[M N])

B = dct2(A,M,N)

式中A表示要变换的图像，M和N是可选参数，表示填充后的图像矩阵大小，B表示变换后得到的图像矩阵。

②函数：dctmtx

D = dctmtx(N)

式中D是返回N×N的DCT变换矩阵，如果矩阵A是N×N方阵，则A的DCT变换可用D×A×D&#39;来计算。这在有时比dct2计算快，特别是对于A很大的情况。

Blkproc

功能：对图像进行分块处理

函数调用形式：B = blkproc(A,[m n],fun, parameter1, parameter2, ...)

B = blkproc(A,[m n],[mborder nborder],fun,...)

B = blkproc(A,&#39;indexed&#39;,...)

参数解释：[m n] ：图像以m\*n为分块单位，对图像进行处理（如8像素\*8像素）

Fun： 应用此函数对分别对每个m\*n分块的像素进行处理

parameter1, parameter2： 要传给fun函数的参数

mborder nborder：对每个m\*n块上下进行mborder个单位的扩充，左右进行nborder个单位的扩充，扩充的像素值为0，fun函数对整个扩充后的分块进行处理。

Mse等价于sum(a.^2)/lenght(a);

MSE和RMSE都是网络的性能函数。MSE是（神经）网络的均方误差，叫&quot;Mean Square Error&quot;。比如有n对输入输出数据，每对为[Pi,Ti],i=1,2,...,n.网络通过训练后有网络输出,记为Yi。那MSE＝(求和(Ti-Yi)^2(i=1,2,..n))/n，即每一组数的误差平方和再除以数据的对数。RMSE叫&quot;Root Mean Square Error&quot;，即在MSE基础上要开根号，中文译为&quot;均方根误差&quot;，MSE=MSE开根号。亦即RMSE是MSE的平方根。

综上所述，在MATLAB Editor（编辑器），新建一个文件，编写代码如下：

clc,clear;close all;

I=imread(&#39;cameraman.tif&#39;);

I=im2double(I);

T=dctmtx(8);%得到一个8\*8的离散余弦变化矩阵

B=blkproc(I,[8 8],&#39;P1\*x\*P2&#39;,T,T&#39;);% x就是每一个分成的8\*8大小的块，P1\*x\*P2相当于像素块的处理函数，p1=T p2=T&#39;,也就是fun=p1\*x\*p2&#39;=T\*x\*T&#39;的功能是进行离散余弦变换

m=[1 1 1 0 0 0 0 0

1 1 0 0 0 0 0 0

1 0 0 0 0 0 0 0

0 0 0 0 0 0 0 0

0 0 0 0 0 0 0 0

0 0 0 0 0 0 0 0

0 0 0 0 0 0 0 0

0 0 0 0 0 0 0 0 ];

B2=blkproc(B,[8 8],&#39;P1.\*x&#39;,m);%舍弃每个块中的高频系数，达到图像压缩的目的

I2=blkproc(B2,[8 8],&#39;P1\*x\*P2&#39;,T&#39;,T);%进行反余弦变换，得到压缩后的图象

cha=abs(I-I2);

junfang=mse(cha);

figure,imshow(I),title(&#39;原始图像&#39;);

figure,imshow(I2),title(&#39;压缩（解压缩）图像&#39;),

xlabel({&#39;均方误差: &#39;;junfang});

点击MATLAB Editor（编辑器）的运行按钮，可看到工作区内容以及运行结果

