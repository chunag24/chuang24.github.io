---
title: Week 3 Summary on C++ Programming
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
1. 变量和基本类型 
- C++ 中的基本数据类型有哪些？它们的存储大小是多少？
	- common data types in c++ includes:
		- int: 4 bytes 
		- float: 4 bytes 
		- double: 8 bytes 
		- char: 1 byte 
		- bool: 1 byte, but compiler dependent 
		
- C++ 中的 sizeof 运算符有什么作用？如何使用它？
	- The `sizeof` operator is used to determine the size of a type or variable in bytes 
	- `int size = sizeof(int)`

- C++ 中的逻辑运算符有哪些？它们的优先级是怎样的？
	- Logic operators include &&, || and !
	- Their precedence is as follows: 
		!, &&, ||
- 什么是 const 关键字？它在 C++ 中有哪些应用？
	- THe const keyword is used to declare constants, it ensures the value of the variable cnanot be modified after initialization
	- It can be used to declare constants, specify function parameters that should not be modified, prevent modification of member functions in classes declared as const.
- 什么是结构体？C++ 中如何声明和使用结构体？
	- A structure is a user defined data type that allows the grouping of variables of different data types under a single name. 
- 什么是枚举类型？C++ 中如何声明和使用枚举类型？
	- An enum typeis a user defined data type used to assign names to integral constants, making the code more readable and maintainable. 
- 指针和引用有什么区别？
	- Pointers: Pointer are variables that store memory addresses. Pointers need to be dereferenced to access the value they point to. Can be reassigned to point to different objects.
	- References: References are aliases to existing variables, and must be initialized upon declaration, cannot be reassigned to refer to a differetn object

2. 字符串、向量和数组
	- 字符串
		- 如何在一个字符串中查找一个子串？
			- use find() to serach for a substring 
			```c++
			std::string str = "hello world";
			std::size_t found = str.find("world");
			if (found != std::string::npos) {
				std::cout << "Substring found at position: " << found << std::endl;
			} else {
    			std::cout << "Substring not found." << std::endl;
			}
			```
		- 如何删除字符串中的空格？
		```c++
			std::string str = "hello world";
			str.erase(std::remove_if(str.begin(), str.end(), ::isspace), str.end());
		```
		- 如何将一个字符串转换为整数？
		```c++
			std::string str = "12345";	
			int num = std::stoi(str);
		```
	- 数组
		- 如何实现数组的二分查找算法？
		```c++
			int binarySearch(int arr[], int size, int target){
				int left = 0, right = size - 1;
				while(left <= right){
					int mid = left + (right - left) / 2;
					int (arr[mid] == target) return mid;
					if (arr[mid] < target) left = mid + 1;
					else right = mid - 1;
				}
				return -1; // for not finding anything
			}
		```
		- 如何对数组进行排序？
		```c++
			int arr[] = {5,2,3,7,1};
			int size = sizeof(arr) / sizeof(arr[0]);
			std::sort(arr, arr+size);
		```
	- 向量
		- 数组和vector有什么区别？
			- Arrays have a fixed size determined at compile time, while vectors are dynamic arrays that can grow or shrink in size at run time.
			- Vectors provide additional functionality such as bound-checking, automatic resizeing, and functions like push_back() and pop_back().
		- 数组和vector各项操作时间复杂度是多少？
			- For arrays:
				- Accessing elements by index :O(1)
				- Linear searching O(n)
				- Sorting: O(nlogn)
			- For vectors:
				- Accessing elements by index :O(1)
				- Linear searching O(n)
				- Sorting: O(nlogn)

- 如何在C++中使用switch语句，它与if语句有什么区别？
	- ```c++
		int choice = 2;
		switch(choice){
			case 1:
				break;
			case 2:
				break;
			default:
				break;
		}
	  ```
	- Main difference between if statement is that switch is used when you have single expression to compare against multiple possible values. If statement can be used for more complex conditions. 
- 请解释一下C++中的goto语句，它的用法和语法。 
	- ```c++
		int i = 0;
		loop:
			if (i < 5) {
				// Do something
				i++;
				goto loop;
			}
	  ```

- 请解释一下C++中的continue和break语句，在什么情况下使用它们？
	- Continue is used to skip the rest of the current iteration of a loop and proceed to the next iteration.
	- Break is used to exit from the loop or switch statement. It is used to terminate the loop prematurely.
- 什么是函数重载？如何实现函数重载？
	- Function overloading allows multiple functions with the same name but different parameters lists to be defined within the same scope.
	- ```c++
		void print(int num) {
			std::cout << "Integer: " << num << std::endl;
		}

		void print(double num) {
			std::cout << "Double: " << num << std::endl;
		}

	``` 
- 什么是默认参数？如何使用默认参数？
	- Default parameters are parameters in a function declaration that have default values assigned to them 
	- If a caller doesn't provide a value for a parameter with a default value, the defautl value will be used. 

- 什么是引用参数？为什么要使用引用参数？
	- Reference parameters allow you to pass variables by references to a function, meaning that changes made to the parameter inside the function affect the original variable. 
	- using pass by reference avoid the overhead of copying large objects when passing them to functions. 

- 么是指针参数？如何使用指针参数？
	- Pointer parameters allow you to pass addresss of a variable to a function, allowing the function to indirectly access and modify the varaible.
- 什么是内联函数？如何使用内联函数？
	- Inline functions are small functions that are expanded at the point of call, rather than being executed through a function call mechanism.
	- Typically used for small, frequently called functions to reduce the overhead of function calls. 
	- ```c++
		inline int square(int num){
			return num * num;
		}
	```

- 什么是函数指针？如何使用函数指针？
	- Function pointers are pointers that point to functions instead of data 
	- ```c++
		void func(unt num){
			cout << "Value: " << num << endl;
		}
		int main(){
			void (*ptr)(int) = &func; // delcare a function pointer 
			(*ptr)(5); // call the function indrectly
		}
	```
- 什么是Lambda表达式？如何使用Lambda表达式？
	- Lambda expressions are annonymouns functions that can be defined inline. 
	```c++
	auto sum = [](int a, int b) {return a + b;};
	```
- 什么是构造函数和析构函数？它们的作用是什么？
	- Constructors are special member functions in a class that are called when an object is created. They initialize the object's data members.
Destructors are special member functions in a class that are called when an object goes out of scope or is explicitly deleted. They release resources held by the object. Constructors have the same name as the class and no return type, while destructors have the same name as the class preceded by a tilde (~).
- 什么是拷贝构造函数？为什么需要它？
 - A copy constructor is a special constructor that creates a copy of an object when it is instantiated from another object of the same class. It creates a new object by copying the member variables of another object.
The need for a copy constructor arises to perform a deep copy of objects during object creation, ensuring the independence of objects in memory and avoiding issues related to data sharing and synchronization.
- 什么是成员函数和成员变量？
  - Member variables are data items within a class or structure used to store the state and attributes of objects. They are also referred to as data members.
Member functions are functions defined within a class or structure used to operate on and access the member variables of a class, as well as perform the functionality of the class. Member functions can access the member variables of an object and call other member functions.
- 什么是虚函数？它的作用是什么？
	- A virtual function is a function declared as virtual in a base class. It can be overridden in derived classes, allowing a base class pointer or reference to dynamically invoke the version of the function specific to the derived class. The purpose of a virtual function is to achieve polymorphism, enabling the invocation of the appropriate function version at runtime based on the actual type of the object, rather than the static type of the pointer or reference.
- 什么是纯虚函数？它的作用是什么？
	- A pure virtual function is a virtual function declared as pure in a base class, meaning it has no implementation and is followed by = 0. It signifies that the base class expects derived classes to provide their own implementations. The purpose of a pure virtual function is to define an interface, requiring derived classes to implement the function so that polymorphism can be achieved through base class pointers or references.
- 什么是const成员函数？为什么需要它？
	- A const member function is a member function declared as const within a class. It promises not to modify the state of the object. In the function declaration and definition, the const keyword follows the parameter list. The purpose of a const member function is to allow calling the function on constant objects and provide additional compile-time safety checks to ensure that the object's state is not unintentionally modified.
- 什么是友元函数和友元类？
	- Friend function is a function declared outside the class but declared as a friend inside the class, allowing it to access the private members of the class.
Friend class is a class declared as a friend inside another class, enabling all member functions of the friend class to access the private members of the declaring class. The purpose of friend functions and friend classes is to grant specific functions or classes access to the private members of a class, providing flexibility while compromising encapsulation.

- 什么是静态成员变量和静态成员函数？
	- Static member variables are variables that belong to the class itself, rather than to individual objects of the class. They have only one copy in memory and exist throughout the program's execution.
Static member functions are functions associated with a class rather than with individual objects of the class. They can access static member variables and other static member functions but cannot directly access non-static member variables or member functions.
The purpose of static member variables and static member functions is to provide data and functionality related to the class itself, which can be shared among all objects of the class.

- 其他
string和char数组有什么区别？什么时候应该使用哪个？
C++标准库中的算法有哪些？它们可以用于哪些容器？
C++标准库中的智能指针有哪些？它们的作用是什么？
C++标准库中的异常处理机制是什么？它们是如何工作的？
C++标准库中的文件流有哪些？如何读取和写入文件？
C++11 标准库：什么是 C++11 标准库？C++11 标准库中有哪些新增的容器？C++11 标准库中有哪些新增的算法？C++11 标准库中有哪些新增的线程和同步原语？
并发编程：什么是并发编程？为什么需要并发编程？如何使用 C++11 中的 std::thread、std::mutex、std::condition_variable 实现并发编程？