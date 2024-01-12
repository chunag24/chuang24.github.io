---
title: Week 1 Summary on C Programming
subtitle:  

# Summary for listings and search engines
summary:  
# Link this post with a project
projects: []

# Date published
date: '2024-01-09T00:00:00Z'

# Date updated
lastmod: '2024-01-09T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.infoworld.com%2Farticle%2F3353419%2Fwhats-new-in-c-plus-plus-20-modules-concepts-and-coroutines.html&psig=AOvVaw2prJcUqHmpVgKux1hWeqzl&ust=1705000798871000&source=images&cd=vfe&opi=89978449&ved=0CBMQjRxqFwoTCOjWvozF04MDFQAAAAAdAAAAABAI)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
 
tags:
  - Academic
 

  
---
 
## Week 2 Question 
1. \n 代表什么
- `\n` represents an escape character representing a newline.
2. / 符号针对浮点数和整形类型的区别是什么
- For int, `/` is used to perform itneger division. The result will be truncated to an integer, the fractional part will be discarded. 
For Float, / will keep the fractional part.
3. 是否了解取整 取余数
- **Rouding:** Refers to converting a float to the nearest integer. Can be achieved using floor(), round(), ceil()
**Remainder:** `%` will be used to find the remainder of diving two integers.
4. 如果变量不初始化的话，数值是多少？
- The value would be undefined, it could contain any value the memory address contians. 
5. '0'的ascii码是多少，知道'0'的ascii如何计算'1'到'9'的ascii码
- The ASCII code for 0 is 48.
Simply add the corresponding number to get number from 1 to 9.
## Week 3 Question 
1. ++i 和 i++ 的区别是什么
`++i` would increment the value first, and then return i.
`i++` would return i first, and then increment i. 
2. 什么是整形变量溢出
Integer overflow occurs when an integer variable store a value that exceeds the max value it can represent. A `signed int` can only sotre a max value of 2^31 - 1. IF it tries to store a larger value, the result will be undefined. The value may jumps to minimum value.
3. 什么时候用if， 什么时候用switch
Use switch when you have a variable and would like to execute different blocks of code depending on the value of the variable. 

## Week 4 Question
1. 什么时候用while循环， 什么时候用for

2. break 和 continue的作用

## Week 5 question
1. 整型有哪些种？他们分别占用多少字节？
- `char` 1 byte, representing a single char or small integer
`int`  4 byte, used for general purpose integer value 
`short` 2 bytes, used for smaller integer 
`long`  4 on 32 bit, 8 on 64 bit, used for larger value 
`long long` 8 bytes, used for very large value.
2. 是否了解浮点数和整型之间的转换？
- Conversion between float and int types, can be done explicitly by (type casting). 
- When converting from float to int, the factional part is truncated. 
- When converting from int to float, the integer is represented as floating point value. 

3. 浮点数有哪些类型？分别占用多少字节
- `float` typically 4 bytes, 
- `double` 8 bytes, double the precision of float 
- `long double` 8 to 16 bytes, provide more precision than double 

4. int a [5]; int* p = a; 所以sizeof(a)和sizeof(p)分别等于多少？
- sizeof(a) is 5 x 4 bytes = 20 bytes 
- sizeof(p) is 4 bytes for most OS, the size of a pointer.

5. 是否了解二进制、八进制、十六进制、十进制之间的互相转换规则？
- Binary to Decimal: 1101 -> 1 x 2^3 + 1 x 2^2 + 0 x 2^1 + 1 x 2^0 = 8 + 4 + 0 + 1 = 13
- hexa to decimal : on a base of 16
- octal to decimal: on a base of 8


6. 各种类型的整型，他们的大小范围是什么？
- `signed char` : -128 to 127
- `unsigned char` 0 to 255 
- `signed short` -32768 to 32767
- `unsigned short` 0 65535
- `signed int` -2,147,483,648 to 2,147,483,647 
- `unsigned int`0 to 4,294,967,295


7. 'a'的ascii码是多少，知道'a'的ascii如何计算'b'到'z'的ascii码
- The ASCII code of a is 97
- add all the way up to 97 + 25 = 122 

## Week 6 question 
1. 是否了解条件短路？
- Short circuiting happens when there is operations like `&&` and `||` 
- With `&&`, the second operand will not be evaluated if the first operand is false 
- With `||`, the second operand will not be evaluated if the first one is true. 
2. 什么是函数声明？什么是函数定义？
- Function declaration tells the compiler about the function name, return type, parameters. 
- Function definition, provides the actual body of the function. 
3. 函数传值和传指针的区别是什么？
- pass by value: The function receives a coopy of the acutal data. Modifications made to the parameter wont affect the parameter outside of the function
- pass by pointer: The function receives the address of the data, the value can be modified directly within this function. 
4. 是否了解递归(递归要多练习题目)
5. 一个程序可以有两个main函数吗？
- No, a C program can only have one main function, as it serves as the starting point of a program.

## Week 7
1. 如果p是指针，*p和p分别代表什么
- *p represents dereferencing the pointer to access the value.
- p just represents the memory address of whatever it is point to. 
2. 如果a数组size为6，访问a[6]会发生什么？
- C is zero-indexed. An array of size 6 has valid indices from 0 to 5. accessing a[6] could lead to accessing memory beyond the allocated space for this array. 
3. 是否了解二维数组的内存排布？
This means the elements of the array are stored row by row. If you have an array int arr[3][4], the elements are stored in memory as arr[0][0], arr[0][1], arr[0][2], arr[0][3], arr[1][0], ..., arr[2][3]. The memory is contiguous, so knowing the base address, the row size, and the column size, you can calculate the address of any element.
4. 字符串和字符数组的区别是什么？
- A string in C is a character array with null character \0 at the end. 

5. 函数参数传入数组会变成什么？
- When an array is passed to a function, it is passed as a pointer to the first element of the array. Meaning that changes made inside the function will affect the original array. The size of array is lost, so we would need to pass the size of the array as seperate parameter.
6. 指针常量和常量指针的区别是什么？
- A pointer constant `const int* p` is a pointer that points to a constant value. Meaning that you cannot change the value pointed to by p, but you could change p itself to point to another address. 
- A constant pointer `int* const p` indicates that the pointer p cannot change its address, but the value at the address can be changed. 

## 字符串
1. 字符串和字符数组的区别是什么？
- A char array is an array of characters, and it doesnt have to be a string. It may or may not end with a null character. 
2. 实现strlen\strcmp\strcpy\strcat\strchr\strstr
```c
size_t strlen(const char *str){
    const char* s;
    for (s = str; *s; ++s){}
    return (s-str)
}
```
3. malloc出来的变量和局部变量的区别？
- THe variable allocated using malloc has global scope, and can be accessed as long as the address is known. It is allocated on the heap.
- The local variable is allocated on the stack, and the scope is limited to the block in which it is defined. Once the block execution is over, the memory of the local variable will be reclaimed. 

## 结构类型
1. 什么是枚举？
- Enum is a data type that allows developer to define a set of named integer constants.
2. 如何计算结构体占用内存大小
- Calculation of Memory size of a structure requires alignment or padding. 
3. 什么是联合？
- Union allows different datat types to be stored in the same memory location. The size of a union is as large as the size of its largest member. 
## 程序结构

1. include头文件时，<>和""的区别是什么？
- The preprocessor seraches for hearder file in standard system directories. This is typically used for standard library headers such as #include <stdio.h> 
- When using "", the preprocessor searches for the header file in the current directory of the source file. If not found, it then searches the std. 
2. 什么是宏？宏和const的区别是什么？
- Macros are defined using #deifne. It is handled by preprocessor, it is essentially a piece of code that is replaced by the value of the macro. 
- const is keyword to make variable's value unmodifiable. `const` have specific type, and they are type-safe. 
3. 局部变量、全局变量、静态变量他们的生命周期、作用域是什么？
- Local Variables:
Lifetime: Exist only within the block/function they are defined in. They are created when the block is entered and destroyed when it is exited.
Scope: Only accessible within the block/function they are declared in.
- Global Variables:
Lifetime: Exist for the lifetime of the program. They are created when the program starts and destroyed when the program ends.
Scope: Accessible from any part of the program.
- Static Variables:
Lifetime: Exist for the lifetime of the program, similar to global variables. However, they are only initialized once.
Scope: If declared within a function, they are only accessible within that function (like a local variable), but they retain their value between function calls. If declared outside any function, they have the same scope as global variables but are only accessible in the file where they are declared (file scope).


## 什么是重定向？
Output Redirection: For instance, program > output.txt redirects the output of program to the file output.txt.
Input Redirection: As an example, program < input.txt reads input from the file input.txt instead of the standard input (usually the keyboard).
## r+和a模式的区别是什么？
When opening files in C using the fopen() function, r+ and a are two different file modes:

- r+ Mode: This is a read/write mode. If the file exists, it is opened for both reading and writing. The file pointer is placed at the beginning of the file, which means any write operations will overwrite existing content starting from the beginning of the file. If the specified file does not exist, fopen() will return NULL.

- a Mode: This is an append mode. If the file exists, writing operations append data at the end of the file without overwriting existing content. If the file does not exist, a new file is created. In a mode, the file is always opened in an appending way, meaning the file pointer is automatically placed at the end of the file.
 

# 操作系统

1. Basic Components of an operating system 
- Process management (process scheduling, creation, termination)
- Memory managemnet  (Memory allocation for process )
- File System Management (Create files, deletion, access control)
- I/O System management  (I/O devices, and input and output operations.)

2. Process communication
- Pipes: Allow processes to communicate in a producer-consumer manner. 
- Message Queues: Enable processes to send and receive messages 
- Shared Memory: Processes share a common memory area 
- Sockets: Used for communication over a network
- Signals: used for simple notifications to processess 

3. What is a deadlock? How to prevent it from happenening?
- Deadlock is a situation where two or more processes are unable to proceed since each is waiting for the other to release resources. 
- Conditions: Mutual Exclusion, Hold and wait, no preemption, circular wait. 
- Prevention: Avoid one or more of the necessary conditions. 

4. Process State Transition Diagram
- New -> Ready -> Running -> Waiting -> Terminated

5. Types of Resource competition among Processess: 
- CPU cycles: Competing for processor time 
- Memory space: Competing for physical and virtual memory
- I/O devices: Competing for input/oupput devices 
- Files: Competing for file access 




