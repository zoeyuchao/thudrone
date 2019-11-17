## OpenCV

### Read img

```python
import cv2
np.array img=cv2.imread('address','way')
# way==1 means BGR
# way==0 means Grey
```

### Convert

```python
imgDst=cv2.cvtColor(imgSrc,cv2.COLOR_BGR2GRAY)
# BGR to Gray
```

### Show Image

```python
imshow(WindowName,Img)
cv2.waitkey(2000)
# wait for 2000 ms
```

### Gauss Blur

```python
# define
def GaussianBlur(imgSrc, ksize, sigmaX, dst=None, sigmaY=None, borderType=None)
# exp
cv2.GaussianBlur(imgDst,(0,0),cv2.BORDER_DEFAULT)
```

### Erode

```python
# define kernel
kernel=getStructuringElement(int shape, Size esize, Point anchor = Point(-1, -1));# shape:矩形：MORPH_RECT;交叉形:MORPH_CROSS;椭圆形：MORPH_ELLIPSE;
## define kernel simple
kernel=np.ones((5,5), np.uint8)
# erode
dst = cv2.erode(src,kernel,iterations)
## iterations表示迭代次数
```

### threshold

函数中的参数Gray表示灰度图，参数127表示对像素值进行分类的阈值，参数255表示像素值高于阈值时应该被赋予的新像素值，最后一个参数对应不同的阈值处理方法。

> https://blog.csdn.net/Eastmount/article/details/83548652

```python
# define
retval,dst = cv2.threshold(src,threshold, maxval, type)
# exp
retval,imgDst=cv2.threshold(imgDst,127,255,cv2.THRESH_BINARY)
```

### Find Circle

之前的降噪和灰度化都是为了这一步的检测

检测圆形方法如下：

```python
cv2.HoughCircles(gray,cv2.HOUGH_GRADIENT,1,50,param1=80,param2=30,minRadius=15,maxRadius=20)
'''
参数1 image：传递图像
参数2 method：默认，不用理解
参数3 dp：默认，不用理解
参数4 minDist：不同圆心的最小距离，单位为像素
参数5 涉及到Canny算法，这里的80为canny算法的上限，这里的canny算法下限自动设置为为上限一半，马上介绍canny算法
参数6 需要理解上面的参考文章，可以认为是需要达到的累加数量
参数7，8 为最小半径和最大半径，避免识别白色的圆圈
'''
```

但是更常见的是检测椭圆

```python
RotatedRect fitEllipse(InputArray points)
'''
输入需要处理的图像的轮廓（contours），返回一个旋转的矩形的参数，为（（中心x，y），（长轴，短轴），旋转角度）
'''
# example：其中S2为椭圆面积1/4（因为不是半长轴/半短轴）

find = 0
for cnt in contours:
  if len(cnt)>50:
    S1=cv2.contourArea(cnt)
    ell=cv2.fitEllipse(cnt)
    S2 =3.14*ell[1][0]*ell[1][1]
    	if (S1/S2)>0.2: #面积比例，可以更改
        if(S1/S2>Smax):
          Smax=S1/S2
          cntMax=cnt
          find=1
if(find==1):
  ell=cv2.fitEllipse(cntMax)
  return (ell[0][0],ell[0][1],3.14*ell[1][0]*ell[1][1]/4)
if(find==0):
  return (-1,-1,-1)
```

### find contours

cv2.findContours()有三个参数。**第一个**是输入图像，**第二个**是轮廓检索模式，**第三个**是轮廓近似方法.

会返回一个元组。OpenCV2.x版本**第一个元素**是轮廓

```python
# define
findContours(image, mode, method)

# exp
contours,hierarchy = cv2.findContours(img,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)


```

mode:

* RETR_LIST 从解释的角度来看，这中应是最简单的。它只是提取所有的轮廓，而不去创建任何父子关系。
* RETR_EXTERNAL 如果你选择这种模式的话，只会返回最外边的的轮廓，所有的子轮廓都会被忽略掉。
* RETR_CCOMP 在这种模式下会返回所有的轮廓并将轮廓分为两级组织结构。
* RETR_TREE 这种模式下会返回所有轮廓，并且创建一个完整的组织结构列表。它甚至会告诉你谁是爷爷，爸爸，儿子，孙子等。

method:

* cv2.CHAIN_APPROX_NONE，表示边界所有点都会被储存；
* cv2.CHAIN_APPROX_SIMPLE 会压缩轮廓，将轮廓上冗余点去掉，比如说四边形就会只储存四个角点。

image:

* original image

contour:

* cv2.findContours()函数首先返回一个list，list中每个元素都是图像中的一个轮廓，用numpy中的ndarray表示。

hierarchy:

* 返回一个可选的hiararchy结果，这是一个ndarray，其中的元素个数和轮廓个数相同，每个轮廓contours[i]对应4个hierarchy元素hierarchy[i][0] ~hierarchy[i][3]，分别表示后一个轮廓、前一个轮廓、父轮廓、内嵌轮廓的索引编号，如果没有对应项，则该值为负数。

```python
#define
drawContours(image, contours, contourIdx, color)

```

> 函数cv2.drawContours()被用来绘制轮廓。第一个参数是一张图片，可以是原图或者其他。第二个参数是轮廓，也可以说是cv2.findContours()找出来的点集，一个列表。第三个参数是对轮廓（第二个参数）的索引，当需要绘制独立轮廓时很有用，若要全部绘制可设为-1。接下来的参数是轮廓的颜色和厚度。

