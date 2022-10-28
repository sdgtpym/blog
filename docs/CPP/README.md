# C++ 技术积累

## C++中一个空类有哪些内容

```cpp
class Demo {}
```

```cpp
class Demo {
    // 构造函数
    Demo();
    
    // 拷贝构造函数
    Demo(Demo& demo);

    // 析构函数
    ~Demo();

    // 赋值运算符
    Demo& operator=(const Demo& demo);

    // 取地址运算符
    Demo* operator&();

    // 静态版本取地址运算符
    const Demo* operator&();
}
```

## explicit 关键字的作用

explicit 中文意思为明确的，该关键字在C++中修饰一个函数的时候，表示明确的，防止隐式类型转换的作用。

该关键字仅对只有一个参数的类构造函数有效，也就是说，仅在构造函数中只有一个参数的时候可以修饰该构造函数；如果有多个参数，则无效，也没有必要使用该关键字，因为多个参数的构造函数不存在隐式类型转换的问题。

## 什么是隐式类型转换

隐式类型转换分两种：算数类型和类类型。

算数类型隐式转换的设计原则就是尽量避免精度损失。

可以参考一下几项规则：

* 整型提升：将小整数类型转换成较大的整数类型。
* 有符号类型转换为无符号类型：int -> unsigned int。
* 条件判断中，非布尔类型转换为布尔类型: 0 -> false, !0 -> true。

## 一个String类的实现

```CPP
class String 
{
public:
    String():
        mStr(nullptr), 
        mLength(0) 
    {}

    String(const char* str) {
        if(nullptr == str) {
            mLength = 0;
            mStr = nullptr;
            return;
        }

        mLength = strlen(str);
        mStr = new char(mLength + 1);
        memset(mStr, 0, mLength + 1);
        memcpy(mStr, str, mLength);
    }

    ~String() {
        if(nullptr != mStr) 
        {
            delete mStr;
            mStr = nullptr;
        }
    }

    String& operator=(const String& src)
    {
        mLength = src.mLength;
        if(nullptr != mStr) {
            delete mStr;
            mStr = nullptr;
        }

        if(nullptr == src.mStr) {
            mStr = src.mStr;
        }

        mStr = new char(mLength);
        memset(mStr, 0, mLength);
        memcpy(mStr, src.mStr, mLength - 1);
    }

    int length()
    {
        return mLength;
    }

private:
    char* mStr;
    int mLength;
}
```