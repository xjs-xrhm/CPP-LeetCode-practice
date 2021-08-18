# CPP并发与多线程1

## 1.线程启动、结束和创建

### （1）函数创建

自己定义一个函数

```cpp
void myPrint() {
	cout << "my subthread is starting" << endl;
	for (int i = 0; i < 5; i++) {
		cout << i << endl;
	}
	cout << "my subthread over" << endl;

	return;
}
```

之后在主程序中创建线程对象，将函数名传入

```c++
thread myThread(myPrint)
```

#### join()

阻塞主线程并等待myprint执行完，当myPrint执行完，join()也执行完
主线程继续往下执行
join代表子线程和主线程汇合

```c++
myThread.join();
```

程序效果如下：主线程会等待子线程结束之后再结束

![image-20210817165801270](https://raw.githubusercontent.com/xjs-xrhm/Images/main/202108171658355.png)

#### detach()

传统多线程程序中，主线程要等待子线程执行完毕，才继续向下执行
detach：分离，主线程不再与子线程汇合，不再等待子线程
使用detach后，子线程和主线程失去联系，也就是变成守护线程，由运行时库管理

```c++
myThread.detach();	//一旦调用detach(),就不能再用join()，否则系统会报错
```

效果如下：主线程结束后，程序终止，不管子线程是否执行完毕

![image-20210817170038025](https://raw.githubusercontent.com/xjs-xrhm/Images/main/202108171700071.png)

#### joinable()

joinable()判断是否可以成功使用join()或detch()
如果返回true,证明可以调用join()或者detach()
如果返回false,证明不可以调用join()或者detach()

```c++
if (myThread.joinable()) {
	cout << "可以调用join()或者detach()" << endl;
}
else {
cout << "不可以调用join()或者detach()" << endl;
}
```

![image-20210817170129824](https://raw.githubusercontent.com/xjs-xrhm/Images/main/202108171701862.png)

## (2)类对象创建

创建一个类

```c++
class test {
public:
	void operator()() {
		cout << "class subthread starting" << endl;

		cout << "class subthread over" << endl;
	}
};
```

在主函数中，创建对象并传入线程

```c++
	test t1;
	thread myThread2(t1);
```

![image-20210817170440663](https://raw.githubusercontent.com/xjs-xrhm/Images/main/202108171704697.png)

## （3）lambda表达式创建

在主程序中创建表达式并传入线程对象那

```c++
	auto lambdaThread = [] {
		cout << "lambda subthread starting" << endl;

		cout << "lambda subthread over" << endl;
	};
//将lambda表达式传入线程对象
	thread myThread3(lambdaThread);
```

![image-20210817170540656](https://raw.githubusercontent.com/xjs-xrhm/Images/main/202108171705688.png)

