### 1. 首先在matlab中将.mat格式文件存为.txt文件，代码如下:
```matlab
clc;
clear;

load('F:\学术资料\计算光刻\源代码\code of Computational Lithography\RevisedAlgorithm\matrix\pz_t.mat');
fop = fopen('matrix_txt\pz_t.txt','w');
M=50; % 行数 
N=50; % 列数

for m = 1:M
  for n = 1:N
    fprintf(fop, '%d ', pz_t(m,n)); % %d表示整数类型，%f表示浮点数
  end
  fprintf(fop, '\n');
end
fclose(fop);
```
### 2. 在c++源代码中读取.txt文件，代码如下：
```c++
#include <iostream>
#include <opencv2/opencv.hpp>
#include <cmath>
#include <fstream>
using namespace cv;
using namespace std;
int main()
{
int i ,j;
Mat pz_t = Mat::zeros(Size(50,50),CV_32F);
ifstream infile;
infile.open("pz_t.txt");
for(i=0;i!=50;i++){
  for(j=0;j!=50;j++){
    infile >> pz_t.at<float>(i,j);
  }
}

infile.close();
// show pz_t
Mat pz_t_converted;
pz_t.convertTo(pz_t_converted,CV_32F,256);
namedWindow("pz_t", 0);
resizeWindow("pz_t", 640, 640);
imshow("pz_t", pz_t_converted);
waitKey();

return 0;
}
```
