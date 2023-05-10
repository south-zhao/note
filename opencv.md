[TOC]

# opencv

## opencv中的Gui特性

### 1.读取图像

`cv2.imread()`内部有两个参数

- cv2.IMREAD_COLOR：读入一副彩色图像。图像的透明度会被忽略， 这是默认参数。
- cv2.IMREAD_GRAYSCALE：以灰度模式读入图像，颜色为1，灰度为0，不变为-1

### 2.显示图像

`cv2.imshow()`窗口会自动调整为图像大小。第一 个参数是窗口的名字，其次才是我们的图像,但是图像会一闪而过，需要在进行处理，加上`cv2.waitKey(0);cv2.destroyAllWindows()`

### 3.保存图像

`cv2.imwrite()`首先需要一个文件名，之后才 是你要保存的图像











