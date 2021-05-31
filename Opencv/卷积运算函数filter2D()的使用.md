### 博客原文：https://blog.csdn.net/xiachong27/article/details/80520154
filter2D()函数的定义如下：
```c++
	CV_EXPORTS_W void filter2D( InputArray src, OutputArray dst, int ddepth,
                            InputArray kernel, Point anchor=Point(-1,-1),
                            double delta=0, int borderType=BORDER_DEFAULT );
```

该函数利用内核实现对图像的卷积运算。参数含义如下：

InputArray src: 输入图像

OutputArray dst: 输出图像，和输入图像具有相同的尺寸和通道数量

int ddepth: 目标图像深度，如果没写将生成与原图像深度相同的图像。原图像和目标图像支持的图像深度如下：
```c++
    src.depth() = CV_8U, ddepth = -1/CV_16S/CV_32F/CV_64F
    src.depth() = CV_16U/CV_16S, ddepth = -1/CV_32F/CV_64F
    src.depth() = CV_32F, ddepth = -1/CV_32F/CV_64F
    src.depth() = CV_64F, ddepth = -1/CV_64F
```
当ddepth输入值为-1时，目标图像和原图像深度保持一致。

InputArray kernel: 卷积核（或者是相关核）,一个单通道浮点型矩阵。如果想在图像不同的通道使用不同的kernel，可以先使用split()函数将图像通道事先分开。

Point anchor: 内核的基准点(anchor)，其默认值为(-1,-1)说明位于kernel的中心位置。基准点即kernel中与进行处理的像素点重合的点。

double delta: 在储存目标图像前可选的添加到像素的值，默认值为0

int borderType: 像素向外逼近的方法，默认值是BORDER_DEFAULT,即对全部边界进行计算。

该函数使用于任意线性滤波器的图像，支持就地操作。当其中心移动到图像外，函数可以根据指定的边界模式进行插值运算。

函数实质上是计算kernel与图像的相关性而不是卷积： 

也就是说kernel并不是中心点的镜像，如果需要一个真正的卷积，使用函数flip()并将中心点设置为(kernel.cols - anchor.x - 1, kernel.rows - anchor.y -1). 

该函数在大核(11x11或更大)的情况下使用基于DFT的算法，而在小核情况下使用直接算法(使用createLinearFilter()检索得到). 

示例程序如下：
```c++
	#include <iostream>
	#include <opencv2/core.hpp>
	#include <opencv2/highgui.hpp>
	#include <opencv2/imgproc.hpp>
	 
	using namespace std;
	using namespace cv;
	 
	int main()
	{
		Mat srcImage = imread("lena.jpg");
	 
		//判断图像是否加载成功
		if(srcImage.data)
			cout << "图像加载成功!" << endl << endl;
		else
		{
			cout << "图像加载失败!" << endl << endl;
			return -1;
		}
		namedWindow("srcImage", WINDOW_AUTOSIZE);
		imshow("srcImage", srcImage);
	 
		Mat kern = (Mat_<char>(3,3) << 0, -1 ,0, -1, 5, -1, 0, -1, 0);
		Mat dstImage;
		filter2D(srcImage,dstImage,srcImage.depth(),kern);
		namedWindow("dstImage",WINDOW_AUTOSIZE);
		imshow("dstImage",dstImage);
	 
	 
		waitKey(0);
	 
		return 0;
	}
```
