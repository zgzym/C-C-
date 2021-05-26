## 1. 头文件

	使用opencv需要在代码的头文件中包含<opencv2/opencv.hpp>，头文件opencv.hpp包含opencv的所有头文件。

## 2. 命名空间

	opencv的命名空间名称为cv，可以在源代码文件中声明using namespace cv来使用opencv的命名空间。注意不要
	
	在头文件(.h)中采用using namespace cv来声明命名空间，以免在源代码文件中造成命名冲突。
	
## 3. 矩阵的初始化

### 3.1 Mat类的定义
		
		在opencv的C++接口中，一般采用Mat类作为矩阵操作类（CvMat为C语言版本的矩阵类）。
		
		Mat类定义于core.hpp中，主要包含有两部分数据：
		
		一部分是矩阵头（matrix header），这部分的大小是固定的，包含矩阵的大小，存储的方式，矩阵存储的地址等等；
		
		另一个部分是一个指向矩阵包含像素值的指针（data）。Mat类的定义如下：
  ```c++
		class CV_EXPORTS Mat
		{
			public:
				// ... a lot of methods ...
				...
			 
				/*! includes several bit-fields:
					 - the magic signature
					 - continuity flag
					 - depth
					 - number of channels
			    */
				int flags;
				//! the array dimensionality, >= 2
				int dims;
				//! the number of rows and columns or (-1, -1) when the array has more than 2 dimensions
				int rows, cols;
				//! pointer to the data
				uchar* data;
			 
				//! pointer to the reference counter;
				// when array points to user-allocated data, the pointer is NULL
				int* refcount;
			 
				// other members
				...
		};
``` 
### 3.2 Mat类的数据类型
	
  
		Mat的存储是逐行存储的，矩阵中的数据类型包括：Mat_<uchar>对应的是CV_8U，Mat_<uchar>对应的是CV_8U，
		
		Mat_<char>对应的是CV_8S，Mat_<int>对应的是CV_32S，Mat_<float>对应的是CV_32F，Mat_<double>对应的是CV_64F，
		
		预定义类型的结构如下所示:
		
		CV_<bit_depth>(S|U|F)C<number_of_channels>
		
		1–bit_depth—比特数—代表8bite,16bites,32bites,64bites—
		
		2–S|U|F–S--代表—signed int—有符号整形
		
		U–代表–unsigned int–无符号整形
		
		F–代表–float---------单精度浮点型
		
		3–C<number_of_channels>----代表—一张图片的通道数,比如:
		
		1–灰度图片–grayImg—是–单通道图像
		
		2–RGB彩色图像---------是–3通道图像
		
		3–带Alph通道的RGB图像–是--4通道图像
		
		对应的数据深度如下：

		• CV_8U - 8-bit unsigned integers ( 0..255 )

		• CV_8S - 8-bit signed integers ( -128..127 )

		• CV_16U - 16-bit unsigned integers ( 0..65535 )

		• CV_16S - 16-bit signed integers ( -32768..32767 )

		• CV_32S - 32-bit signed integers ( -2147483648..2147483647 )

		• CV_32F - 32-bit ﬂoating-point numbers ( -FLT_MAX..FLT_MAX, INF, NAN )

		• CV_64F - 64-bit ﬂoating-point numbers ( -DBL_MAX..DBL_MAX, INF, NAN )
	
### 3.3 定义Mat类矩阵

		3.3.1 使用构造函数
			
			Mat::Mat();	//default
			
			Mat::Mat(int rows, int cols, int type);
			
			Mat::Mat(Size size, int type);
			
			Mat::Mat(int rows, int cols, int type, const Scalar& s);
			
			Mat::Mat(Size size, int type, const Scalar& s);
			
			Mat::Mat(const Mat& m);
			
			//参数说明：
			
			//int rows：高
			
			//int cols：宽
			
			//int type：参见"Mat类型定义"
			
			//Size size：矩阵尺寸，注意宽和高的顺序：Size(cols, rows)
			
			//const Scalar& s：用于初始化矩阵元素的数值
			
			//const Mat& m：拷贝m的矩阵头给新的Mat对象，但是不复制数据！相当于创建了m的一个引用对象
			 
			//例子1：创建100*90的矩阵，矩阵元素为3通道32位浮点型
			
			cv::Mat M(100, 90, CV_32FC3);
			
			//例子2：使用一维或多维数组来初始化矩阵，
			
			double m[3][3] = {{a, b, c}, {d, e, f}, {g, h, i}};
			
			cv::Mat M = cv::Mat(3, 3, CV_64F, m);
		
		3.3.2 使用create函数

			Mat a = create(10, 9, CV_16U);	//创建10*9的矩阵，矩阵元素为16位无符号整型
			
			//create的一个特殊用法：如果初始化的时候没有传入size的参数，或者后面需要改变size的参数，可以使用create来调整
			
			// make 7x7 complex matrix filled with 1+3j.
			
			cv::Mat M(7,7,CV_32FC2,Scalar(1,3));
			
			// and now turn M to 100x60 15-channel 8-bit matrix.
			
			// The old content will be deallocated：隐式使用release()释放
			
			M.create(100,60,CV_8UC(15));		
	
### 3.4 合并矩阵	
	
		//假设现有需要将n个CvMat类型的矩阵（每个都是1*m的行向量）合并到一个n*m（n行m列）的矩阵中，即：矩阵合并问题。
		
		CvMat* vector; //行向量：1*m
		
		Mat dst;	//目标矩阵：n*m
		
		Mat vectorMat = Mat(vector, true);	//将CvMat转为Mat
		
		Mat tmpMat = M.row(i);	//注意：浅拷贝（参见"复制矩阵"）
		
		vectorMat.copyTo(tmpMat);	//注意：深拷贝（此时tmpMat已改变为vectorMat，即：目标矩阵的第i+1行被赋值为vectorMat）
		
		//以上为改变目标矩阵的i+1行的值，以此类推即可将n个行向量合并到目标矩阵中。
			
### 3.5 访问矩阵元素

		3.5.1 at函数
```c++
			M.at<double>(i,j) += 1.f;	//set (i,j)-th element
		
			int a = M.at<int>(i,j);		//get (i,j)-th element
```
		3.5.2 矩阵元素指针（ptr）
```c++
			for(int i = 0; i < M.rows; i++)
			{
				const double* pData = M.ptr<double>(i);	//第i+1行的所有元素
				for(int j = 0; j < M.cols; j++)
					cout<<pData[j]<<endl;
			}	
```
## 4. 矩阵的保存与读取

	c++中opencv的Mat类矩阵的保存采用FileStorage类，示例代码如下：	
```c++
    FileStorage fs("example.xml", FileStorage::WRITE); //表示以“WRITE"模式打开文件example.xml，若不存在则新建
		
    fs << "example" << mat_example; // 将矩阵mat_example存入文件example.xml
		
    fs.release(); //释放fs的内存空间
```
	矩阵文件的读取示例代码如下：
```c++
    FileStorage fs("example.xml", FileStorage::READ); //表示以“READ"模式打开文件example.xml，若不存在则报错
		
    Mat mat_example; // 定义矩阵mat_example用于保存example.xml中的矩阵
		
    fs["example"] >> mat_example; // 将文件example.xml中的矩阵数据写入矩阵mat_example
		
    fs.release(); //释放fs的内存空间
```
	
	在存储数据时，fs<<"vocabulary"<<mat将mat矩阵保存在了声明fs对象时制定的xml文件的vocabulary标签下，也可换成其它标签。
	
	可以多个<<符号连续使用，程序将自动将引号内容理解为标签名，不带引号的理解为数据变量或者常量。
	
	在读取数据时，[ ]中的内容为指定的标签，并将数据读入>>的变量中。

	来源：https://blog.csdn.net/mmjwung/article/details/6913540		
			
