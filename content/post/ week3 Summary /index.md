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
- string和char数组有什么区别？什么时候应该使用哪个？
	- String is a data type provided by C++ standard library for representing text data. It is dynamic in size, and support operation like concatenation, searching, and replacing. Strings are high level abstractions, it is easier to use and maintain. 
	- Char array is a basic data type used to store characters. It has fixed size, and its length needs to be specified during declaration. (need more)
- C++标准库中的算法有哪些？它们可以用于哪些容器？ 
	- C++ standard library provides sorting, searching, and contianer manipulation. `sort` `find` `binary_search` `copy` and `transform`. These algorithms can be used on vectors, lists, maps, and arrays.
- C++标准库中的智能指针有哪些？它们的作用是什么？
	- There are mainly 3 types of smart pointers 
	- `unique_ptr`exclusively owns the object, and automatically release the pointed object
	- `shared_ptr`allow mulitple pointers to share ownership of the same object and uses reference counting to manage object's lifetime.
	- `weak_ptr` addresses the issue of circular references in shared_ptr by observing without increasing the reference count. 
- C++标准库中的异常处理机制是什么？它们是如何工作的？
	- The exception handling mechanism in the C++ std library uses try-catch blocks to catch and handle exception
	- The `try` block attempts to execute the code that may throw an exception, and then catches and handles the exception in catch block
	- Exceptions can be thrown by errors or exceptional conditions in the program, such as `division by zero` or `memory allocation failure`
- C++标准库中的文件流有哪些？如何读取和写入文件？
	- File stream in c++ includes ifstream, ofstream, and fstream.
	- `ifstream` refers to reading data from files, `ofstream` refers to writing data to files, `fstream` allows both reading and writing operations.
	- Reading from files can be done by using `>>`, while writing to fiels done using output operator `<<`
- C++11 标准库：什么是 C++11 标准库？C++11 标准库中有哪些新增的容器？C++11 标准库中有哪些新增的算法？C++11 标准库中有哪些新增的线程和同步原语？
	- Inlcudes new containers and algorihtms like `unordered_map`, `unordered_set`, `move`, `emplace`, `lambda expressions`
	- Threading and synchronization: `std::mutex`, `std::thread`, `std::condition_variable` make concurrent programming more convenient and safe.
- 并发编程：什么是并发编程？为什么需要并发编程？如何使用 C++11 中的 std::thread、std::mutex、std::condition_variable 实现并发编程？
	- Concurrent programming refers to executing mulitple independent tasks simultaneously, on multi-core processors. 
	- Improve program performance and resource allocation. 

- I/O
	- Reading files in C++
	  - Typically use the `ifstream` object from the `<fstream>` library
	   1. Need to open the file using `open()`method of iftream, specifying the name and the mode. 
	   2. Then, use input operations (such as `>>` `getline()`) on the ifstream object to read data from the file.
	   3. After reading, close the file using `close()` of ifstream object. 
	   - 
		```c++
			#include <iostream>
			#include <fstream>

			int main(){
				std::ifstream file("example.txt"); //open the file 
				if (file.is_open()) {
					std::string line;
					while (std::getline(file, line)) {
						std::cout << line << std::endl; 
					}
					file.close();
				}else{
					std::cerr << "Unable to open file" << std::endl;
				}
				return 0;
				
			}
		```
	- Writing to file in C++ 
	  1. To write to a file in c++, you would use ofstream object from <fstream> library.
	  2. Open the file using open(), specify the name and the mode. 
	  3. Then use output operations (such as `<<`) on the ofstream object to write datat to the file. 
	  4. After writing, close the file `close()` of ofstream object. 
	  ```c++
			#include <fstream>
			int main(){
				ofstream file("/Users/chuang24/CLionProjects/cpp_practice/example.txt");
				if (file.is_open()) {
					file << "Hello, world! " << endl;
					file.close();
				} else {
					cerr << "Unable to open the file. " << endl;
				}
				return 0;
			}
	  ```
	- Buffering in C++
		- Buffering in a mechanism used to improve the efficiency of input/output operations.
		- The data is often first stored in a buffer before transferred to the actual file or device when you read or write data to a file or a console.
		- Buffers reduce the number of I/O operations, making file operations more efficient. 
		- C++ std library handles buffering automatically for more operations.  

    - Ridirecting C++ standard output to a file 

- Container
	- Vector vs Array vs List 
		- Vector
			- It is a dynamic array that can resize to accomodate a varying numer of elements 
			- Can dynamic resizing, insertion, and deletion of elements
			- Elements are stored in contiguous memory locations
			- Good for frequent insertion, deletion, or resizing of elements are requried. 

		- Array 
			- It is a fixed size collection of elements of same type. 
			- Elements are stored in contiguous memory locations
			- Elements accessing are done using indices 
			- Good for scenarios where a number of elements is known at compile time and does not change frequently.

		- List
			- It is a double linked list where each element is stored in a separate node 
			- Lists do not have contiguous memory allocation for elements 
			- List supports fast insertion and deletion operations at both ends and at any position within the list. 
			- Does not support random accessing using indices, traversal is done through iterators.
			- Slower access times compared to arrays and vectors because it needs to traverse node sequentially 
			- Good for frequent insertion and deletion, especially in the middle of the container. 
	- Deque (short for double ended queue) is a sequential container similar to vector but can efficiently insert and delete elements at both beginning and end of the queue. 
		- Double ended: supports efficient insertion and deletion operation at both end of the sequence. 
		- Dynamic resizing: Like vectors, allow growing dynamically as elements are added 
		- Random access: Support indices, can access any element in deque in constant tyime.
		- Iterator support: Allow traversal of elements in sequential order, can be used to insert, remove, and modify elements within the deque.
		- Overall, it offer a balance between vector and list, making it suitable for scenarios where elements need to be manipulated at both ends of the container efficiently. 
	- Fast Insertion and Deletion in Vector and Deque:
	  - Vector: Use `push_back()` for fast insertion, `pop_back()` for fast deletion at the end. 
	  - Deque: `push_front()` for `push_back()` for fast insertion at both ends, `pop_front()` `pop_back()` for fast deletion from both ends.
	- Difference between List and Vector 
		- Data structure: Vector has a dynamic arrays internally, lists use doubly linked lists.
		- Insertion and Deletion: Could be slow for large vector container, especially in the middle due to element shifting. Lists offer constant time insertion and deletion. 
		- Random access: Vector support, while list does not support
		- Memory overhead: Lists have higher memory overhead than vectors due to the pointer used. 
	- Automatic Resizing in Vector:
	 	- Vectors automatically resize themselves when they run out of space, this is done by allocating a new and larger array, copying existing elements to it, and deallocating the old array. 
	- Removing Duplicates in Vector: 
		- You can first sort the vector using std::sort(), then use std::unique() with erase() to remove duplicates. 
		- show code plz 
	- Difference between vector and list iterators: 
	    - Vector iterators are implemented as pointers, allowing random access and arithmetic operation. 
		- List iterators are pointers to nodes, supporting only forward traversal and limited arithmetic. 
	- Implement Stack and queue using vector: 
		- For stack: Simply use `push_back()` to push elements onto the stack, and `pop_back()`to remove element from the top.
		- For queue: use `push_back` to enqueue elements and `pop_front()` to dequeue elements. 

- Generic Algorithm
	- To sort a container using generic algorithms in C++ 
	```c++
		std::vector<int> vec = {3, 1, 4, 1, 5 ,10};

		std::sort(vec.begin(), vec.end());
	```
	- When is iterator becoming invalidated?
		- Not done yet 
	- Find the max or min elemnet in a container using generic algorithms, use `std::max_element` or `std::min_element` from <algorithm>
	```c++
		auto max_element = std::max_element(vec.begin(), vec.end());
    	auto min_element = std::min_element(vec.begin(), vec.end());
		// returns the iterator pointing to the max and min elements. 
	```

- Associative Containers 
	- This type of containers that store elements in a sorted order based on keys. 
	- `std::set`: A sorted set of unique keys. 
	- `std::map`: A sorted map of key-value pairs.
	- `std::multiset`: A sorted multiset that allow duplicate keys.
	- `std::multimap`: A sorted multimap that allows duplicate keys with associate values.

	- Difference between `std::map` and `std::unordered_map`:
		- Ordered vs Unordered: `std_map` is an ordered container that maintains elements in sorted order based on keys. `unordered_map` is unordered associative container that does not maintain any order. 
		- Performance: `std:unordered_map` typically have constant time complexity on average O(1). Insertion, deletion, and search operation on `std::map` has log time complexity Olog(n)
		- Use `map` when you need the elements to be sorted by keys when require specific ordering based on keys. Use `unordered_map` for fast retrieval, and do not need elements to be ordered. 
	- Elements in C++ associative containers are typically sorted based on the keys. For std::map and std::set, the sorting is based on a comparison function provided by the std::less functor by default. For std::unordered_map and std::unordered_set, the elements are not sorted as they use a hash table-based implementation.

	- The difference between associative container iterator and pointer? 
		- Iterators in associative containers behave similarly to iterators in other containers, allowing traversal through the elements. 
		- However there are some differences.
		1. Ordering: In ordered associative containers like `map` and `set`, iterators traverse in sorted order. In unordered container like `unordered_map` and `unordered_set`, iterators traverse in unspecified order.
		2. Stability: Iterators in associative containers remain valid unless the corresponding element is erased. However, inserting new elements may invalidate iterators in certain scenarios. 

	- How to use self defined comparison function in associative container? 
		- To use a custom comparison function for sorting elements in C++ associative containers like `map`, you can provide a custom comparison functor as a template argument. 
		- ```c++
			#include <map>
			//Custom comparison function
			struct CustomCompare{
				bool operator()(const int& a, const int& b) const{
					return a > b; //sort in a descending order
				}
			};
			int main(){
				std::vector<int> vec = {3, 1, 4, 1, 5 ,10};
				std::map<int, string, CustomCompare> custom_map;
				custom_map[1] = "One";
				custom_map[2] = "Two";
				custom_map[3] = "Three";
				custom_map[5] = "Crazy";
				for (const auto& pair : custom_map){
					cout << pair.first << ": " << pair.second << endl;
				}


				return 0;
			}
		```
- Smart pointers 
	- What are smart pointers? How do they work? 
		- Smart pointers are objects that mimic the behavior of pointers but provide additional features such as automatic memory managemnet. They work by encapsulating a raw pointer and managing the memory allocated ot it. `unique_ptr` `shared_ptr` and `weak_ptr` are differnet smart pointers each with its own ownership semantics, and memory management strategies. 
	- What is the difference between arrays and pointers? How to dynamically allocate arrays? 
		- Arrays are collections of elements of the same data type stored in contiguous memory locations, where pointers are variables that store memory addresses. To dynamically allocate arrays in C++, you can use the `new` keyword to allocate memory and initialize an array of a specific size. 
	- What is a virtual destructor? Why is it needed? 
		- A virtual destructor is a destructor in a base class that is declared as virtual. It is needed when a class is expected to be used as a base class where objects of derived classes may be deleted through a pointer to the base class. Without a virtual destructor, deleting an object through a base class pointer may result in underfined behaviour and resource leaks. 
	- How to handle exceptions, such as memory allocation failure in C++
		- It is handled using `try` `catch` and `throw` keywords. When an exceptional situation occurs, such as a memory allocation failure, the code within the `try` blcok is executed, if an excpetion is thrown, it will be caught by the `catch` block, where appropriate error handling will be performed. 

	- What is dynamic memory allocation? How to perform dynamic memory allocation? 
		- Dynamic memory allocation refers to the process of allocating memory for variables at runtime. It can be performed using `new` and `delete` operators. 

	- What are the differences between `new` and `malloc` in C++ 
		- `new` is an operator in C++ used for dynamic memory allocation that constructs and initialize objects, it  returns a pointer to the type declared.
		- `malloc` is a function in C used for memory allocation without initializing objects, it retuns a void pointer that needs to be casted to the appropriate type. 

	- What is memory leak? How to avoid memory leaks? 
		- A memory leak occurs when memory is not deallocated when no longer needed. Either make sure to `delete` for each `new` memory allocated, or use smart pointers. 
	- What is dynamic polymorhpism? How to achieve dynamic polymorehpism in C++?
		- Dynamic polymorephism refers to the ability of a function or method to behave differently based on the obejct it is called on. It is achieved using virtual functions and inhereitance. When a function is declared as virtual in a base class and overriden in derived class, the appropriate version of the function is called based on the type of the object at runtime.
 
 - What are move constructors and move assignent operators? 
	- Move constructors and move assignment operators are speical member functions introduced in C++11 to transfer ownership of resources from one object to another. They are used to move contents of a temporary object or an object about to be destroyed into another object, typically avoiding unncessary copying of data. Move constructors are invoked when initializing an object from an rvalue, while move assignment operators are called when assigning to an already initialized object from an rvalue. 
 - What is a copy constructor 
	- A copy constructor is a special construcotr that initializes a new object as a copy of existing object. It takes a reference to another object of the same type as a parameter and creates a new object by copying the contents of the parameter object. Copy constructors are invoked when objects are passed by value as function arguments, returned by value from functions. 
 - What is an assignment operator? 
	- Assignment operator is a special member function in C++ that assigns the contents of one object to another object of the same type. It is typically overloaded to perform member wise assignmetn of data members from the right hand side object to the left hand side object. 
 - What is a default constructor? 
	- It is a constructor that can be called without any arguments, it is used ot initialize objects when no explicit initialization values are provided. If a class does not define any constructors, the compiler automatically generates a default constructor for it. 
 - When is the copy constructor called? 

 - When is the assignment operator called? 


 - How to implement deep copy and showllow copy? 
	- Shallow copy: Copies the values of all data members from one object to another, if the data member contain pointers, only the memory addresses are copied., not the dynamically allocated memory pointed to by the pointers. 
	- Deep Copy: Copies not only the value but also the allocated memory. This involves allocating new memory and copy from the source to destination object. 

 - What is RAII technique, and how is it used to manage resources 
	- Lifetime of a resource is tied to the lifetime of an object. RAII ensure that resources are properly managed and automatically cleaned up when the object goes out of scope. 
 - How to prevent objects of a class from being copied? 
	- To prevent objects of a class from being copied, declare the copy constructor and assignmetn operator as private and provide no implementation for them. This prevents the compiler from generating these functions and makes it a compile-time error to attempt to copy objects of the class. 