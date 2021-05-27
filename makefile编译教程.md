### 1. 简单粗暴版
	
	假设有2个.cc文件需要编译，分别为main.cc和function.cc，则新建makefile文件：
	\# vim makefile
	
	修改makefile内容为：
  
```c++
  main:main.o function.o
    g++ -g -o main main.o function.o -std=c++11 `pkg-config --libs --cflags opencv4`

  function.o:function.cc
    g++ -g -c function.cc -std=c++11 \`pkg-config --libs --cflags opencv4\`

  main.o:main.cc
    g++ -g -c main.cc -std=c++11 \`pkg-config --libs --cflags opencv4\`

  .PHONY:clean
  clean:
    -rm -f main.o function.o
```
	
  其中，`pkg-config --libs --cflags opencv4`为编译opencv库的命令；
	
	需要注意编译顺序，执行make时按照从下往下的顺序执行，即：
	
	# g++ -g -c main.cc -std=c++11 \`pkg-config --libs --cflags opencv4\`
	
	# g++ -g -c function.cc -std=c++11 \`pkg-config --libs --cflags opencv4\`
	
	# g++ -g -o main main.o function.o -std=c++11 \`pkg-config --libs --cflags opencv4\`
	
	输入下列命令执行makefile:
	
	# make
	
	编译完成后会生成main, main.o和function.o文件，执行下列命令删除.o文件：
	
	# make clean
