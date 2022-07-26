## C++中各种符号的重载

首先我们应该先了解一下重载的概念：

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

**就我自己对运算符的重载的理解，我认为可以简单解释为对一个C++中已经存在的符号重新对它进行定义，以适应新出现的功能需求，而新定义的符号只在它的作用域中发挥作用，而原有的符号依旧发挥作用，系统依照情况选择合适的定义。**

### 1、重载示例符号

+=，[]，*，==，！= **

 ### 2、演示代码

``` c ++
/**
 * @author lwj
 * 修改日期 2022.3.21
 * 异常简单例子
 * */

//对+，+=，[]，*，==，！=  进行了重载

#include <iostream>
#include <string.h>
#include <assert.h>

using namespace std;

class String
{
private:
    char *data;

public:
    String(const char *str = "")
    {
        if (str == NULL)
        {
            data = new char[1];
            data[0] = 0;
        }
        else
        {
            data = new char[strlen(str) + 1];
            strcpy(data, str);
        }
    }
    String(const String &s)
    {
        data = new char[strlen(s.data) + 1];
        strcpy(data, s.data);
    }
    ~String()
    {
        delete[] data;
    }

public:
    //+重载
    String operator+(const String &s1)
    {
        char *tmp = new char[strlen(s1.data) + 1]; //先创建存放两个相加的数的大小的内存
        strcpy(tmp, data);                         //先把一个数先放入tmp中
        strcat(tmp, s1.data);                      //然后再将另一个数拼接起来
        return tmp;
    }

    //=重载
    String operator=(const String &s2)
    {
        strcpy(data, s2.data); //先把一个数先放入tmp中
        return String(data);
    }

    //+=重载
    void operator+=(const String &s3)
    {
        char *tmp = new char[strlen(s3.data) + strlen(data) + 1];
        strcpy(tmp, s3.data);
        strcat(tmp, data);
        delete[] data;
        data = tmp;
    }

    //[]的重载
    char operator[](int pos)
    {
        assert(pos >= 0 && pos <= strlen(data));
        return data[pos];
    }

    //==重载
    bool operator==(const String &s5)
    {
        if (data == s5.data)
        {
            return true;
        }
        else
        {
            return false;
        }
    }

    //*重载
    char operator*(int i)
    {
        return data[i];
    }
    //输出数据
    void printStr()
    {
        int count = strlen(data);
        for (size_t i = 0; i < count; i++)
        {
            cout << data[i];
        }
        cout << endl;
    }
};

int main()
{
    String a1 = "asdfna";
    String a2 = "sfasdfasf";

    String a3(a1); //测试拷贝构造
    // cout<<"a3.data = ";
    // a3.printStr();

    // a3 = a1;//=号重载,防止重复释放空间
    // a3 = a2 + a1;//测试=重载
    // cout<<"a3.data = ";

    // a3+= a2; //+=重载
    // cout<<"a3.data = ";
    // a3.printStr();

    // cout<<a3[0]<<endl;//测试[]重载

    // if(a1==a2)//测试==重载
    // {
    //     cout<<"相等"<<endl;
    // }else
    // {
    //     cout<<"不相等"<<endl;
    // }
}
```

### 3、示例代码讲解

上文中的代码是一个简单String类的测试，String类是对于C++中string 类型部分功能进行仿写，我要通过这个示例代码，来讲解运算符重载的实现。

#### 3.1 String类 +重载

**String类中 重载+号的功能是把两个字符串合并**

以下为实现代码：

``` c ++
    String operator+(const String &s1)
    {
        char *tmp = new char[strlen(s1.data) + 1]; //先创建存放两个相加的数的大小的内存
        strcpy(tmp, data);                         //先把一个数先放入tmp中
        strcat(tmp, s1.data);                      //然后再将另一个数拼接起来
        return tmp;
    }
```

以下为测试代码：

``` c ++
    // a3 = a1;//=号重载,防止重复释放空间
    // a3 = a2 + a1;//测试+重载
    // cout<<"a3.data = ";
```

#### 3.2 String类 =重载

**String类中 重载=号的功能是把一个字符串赋值给另一个字符串**

以下为实现代码：

``` c ++
    //=重载
    String operator=(const String &s2)
    {
        strcpy(data, s2.data); //先把一个数先放入tmp中
        return String(data);
    }
```

以下为测试代码：

```c ++
    a3 = a1;//=号重载,防止重复释放空间
    a3 = a2 + a1;//测试=重载
    cout<<"a3.data = ";
```



#### 3.3 String类 +=重载

**String类中 重载+=号的功能是把两个字符串合并然后赋值给当前对象**

以下为实现代码：

``` c ++
    //+=重载
    void operator+=(const String &s3)
    {
        char *tmp = new char[strlen(s3.data) + strlen(data) + 1];//开辟空间
        strcpy(tmp, s3.data);
        strcat(tmp, data);
        delete[] data;//删除旧空间
        data = tmp;//data指向新空间的首地址
    }
```

以下为测试代码：

```c ++
    a3+= a2; //+=重载
    cout<<"a3.data = ";
    a3.printStr();
```



#### 3.4 String类 []重载

**String类中 重载[]号的功能是返回[]号里下标的元素**

以下为实现代码：

``` c ++
    //[]的重载
    char operator[](int pos)
    {
        assert(pos >= 0 && pos <= strlen(data));
        return data[pos];
    }
```

以下为测试代码：

```c ++
cout<<a3[0]<<endl;//测试[]重载
```



#### 3.5 String类 ==重载

**String类中 重载==号的功能是判断两个字符串是否相等**

以下为实现代码：

``` c ++
    //==重载
    bool operator==(const String &s5)
    {
        if (data == s5.data)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
```

以下为测试代码：

```c ++
     if(a1==a2)//测试==重载
     {
         cout<<"相等"<<endl;
     }else
     {
         cout<<"不相等"<<endl;
     }
```

### 4、总结

想要学好重载的基本使用，首先需要知道在没重载之前的这个符号是怎么用，而你重载后需要它能干什么，它能干什么取决于你在重载函数中赋予它怎样的能力。



