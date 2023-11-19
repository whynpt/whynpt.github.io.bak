# 字符串
  
## 初始化 
c里使用char类型的数组表示字符串，需要包含头文件`string.h`；c++中使用std::string表示字符串，在头文件`string`中。
  
char类型的数组以数组或字符串常量的形式初始化，std::string是一种类，有多种初始化方法，取决于构造函数和重载后的运算符‘=’。如下所示：
  
`char cArr[] = {'c', 'd', '\0'};`  // 编译器不会自动添加'\0'，如果不自己添加，打印输出会有意外情况  
`char cArr1[] = "cd";`  
`std::string s1;`  
`std::string s2(s1);`  
`std::string s3("cd");`  
`std::string s4 = "cd";`  
`std::string s5(n, 'c');`  
  
如果以printf的输出为标准，c中char型指针也算字符串，可以用一个字符串常量给一个char型指针赋值：  
`char *cPtr = "aaa";`  
注意使用字符串常量是不需要头文件的。
  
## 内存  
利用上面提到的构造方式，定义局部变量。  
`long long head = 0x10203040;`  
`std::string str("fffffff");`  
`std::string str1 = "eeeeeeeee";  // 9 'e'`  
`char* cPtr = "ddddddd";`
`char cArr[64] = {'c', 'c', 'c', 'c', 'c', 'c', 'c', 'c'};  // 8 'c'`  
`char cArr1[64] = "bbbbbbb";`
`const char* cCPtr = "aaaaaaa";`
`std::string longStr(16, 'g');`
`long long tail = 0x50607080;`  

局部变量的起始地址如下图：![字符串内存](pic/string_memory.JPG)
  
对于char型指针cPtr和cCPtr，栈内存中只存放指针的值，占8字节。对于char型数组，整个数组都存放在栈内存中。对于`std::string`类型的变量，如果字符串的长度小于16，则变量在栈内存中占32字节。从低到高分别是字符串的地址、字符串长度、字符串内容。最后8个字节的内容随字符串长度变化，如果字符串长度小于8，则最后一个字节为0x10；如果大于8小于16，则最后16个字节全是字符串内容。虽然在gcc的环境中`sizeof(std::string)`的输出是32，但如果字符串的长度大于某个值，`std::string`类型的变量会在内存中占40个字节，具体原因要看底层源代码。
  
## 转换
  
这里介绍`const char*`，`std::string`以及`char []`三者的互相转换方法。
- `std::string`很接近python的字符串，可以有用'[]'索引得到字符串，也可以用`sunstr`函数实现切片，也重载了`+`可以实现字符与字符串以及字符串与字符串的拼接。直接调用`c_str()`，返回值可以赋给`const char*`，反过来`const char*`类型的变量可以直接赋给`std::string`
- `char []`是数组，至多在初始化的时候可以使用字符串常量，其他时候赋值必须是可修改的左值，所以不能用其他两种类型赋值，必须以修改内存的方式进行复制，例如调用`strncpy, strcpy, memset`函数
  
`strncpy`和`strcpy`都是c的库函数，包含在`string.h`中，函数入参都包括源、目的字符串，但`strncpy`多一个size参数，表示要赋值的字符个数。`strcpy`复制操作遇到`\0`结束，但`strncpy`是严格复制规定的字符个数，如果个数大于源字符串长度，就会填充`\0`。两个函数都必须是传入的源字符串必须是可以修改的，即为`char*`，不能是`const char*`，否则会有段错误。
## 接口
  
与字符串操作相关的有`insert, erase, assgin, append, find, replace`等，数值转字符串函数`to_string, stoi, atoi, stof, atof`，切片`substr`，