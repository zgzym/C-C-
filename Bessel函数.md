### cmath库中的bessel函数需要支持c++17才能使用，对于c++11而言，包含bessel函数的头文件为<tr1/cmath>，

### 命名空间位于std::tr1.

### 一类bessel函数为：
```c++
	double cyl_bessel_j(double nu, double x);
	float cyl_bessel_jf(double nu, double x);
	long double cyl_bessel_jl(double nu, double x);
```

使用时需加上命名空间，如std::tr1::cyl_bessel_jf(1.0, x)

而regular modified bessel function函数名为cyl_bessel_i,cyl_bessel_if,cyl_bessel_il;

iregular modified bessel function函数名为cyl_bessel_k,cyl_bessel_kf,cyl_bessel_kl;

### 二类bessel函数又称为Neumann function(诺依曼函数)，其函数名为cyl_neumann，cyl_neumannf,cyl_neumannl.
