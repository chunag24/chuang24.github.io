---
title: Week 2 Summary on C++ Programming
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
 

1. C++和C的区别是什么？
- C++ is an object oriented programming language, with support of encapsulation, inhereitance, and polymorphism, where C is a procedural programming language(meanging it focuses on the procedure or steps in solving a problem)
- C++ has the Standard Template Library which provides a set of common classes and interfaces. Such as vectors, sets, maps, stacks, queues. 
- Natively supports inline functions, which can be used to optimize performance of small, frequently used functions.
	 
2. #define等宏替换的处理发生在编译还是预编译阶段？
- Macro replacement occurs during the preprocessing stage, not during the compilation. During preprocessing, the preprocessor handles "#" and file inclusions, and conditional compilation(#ifdef).
3. 编译和预编译的区别是什么？
- Preprocessing
  - FIrst phase of the compilation process. 
  - Handles macro definition, file inclusion, and conditional compilation commands.
  - Results in a modified version of the source code with all the preprocessing directives resolved, ready for actual compilation
- Compiling 
  - Compiler translate the preprocessed source code into machine code or an intermediate form 
  - It involves checking syntax, and semantics, optimizing the code.
  - Output is an object file, which then requires linking before it becomes an executable. 
4. 面向对象三大特性是什么？
  - Encapsulation
    - It is the process of hiding internal state and implementation details of an object, exposing only the interface for the object's behaviour. 
    - This reduces complexity of the system, reusability of the code, as well as improving security by guarding against unauthorized access. 
  - Inheritance
    - This is a feature where a child class can inherit features from another class
    - It allows for code reuse, establishing a hierarchical relationship between classess. 
  - Polymorphism 
    - The ability of different classes to be treated as instances of the same class through a common interface. "Many shapes", "Many forms"
    - Can be achieved through method overriding, where the child class provides a specific implementation of a method defined in parent class. 
    - The base class pointer could use the virtual function declared in both the base and derived class depnding on what type of object the poitner is point to.  
5. include头文件时<>和""区别是什么？
  - <> would search for the files in 
	<>先在当前文件所在的文件目录进行查找对应的文件
	“”在系统文件目录中进行查找
6. 成员变量的作用域和声明周期，和本地变量（局部变量）有什么区别？
- The scope of member variable is the lifespan of the object. The scope of the local variable ends when the block is finished. 
7. extern有什么作用？
- When a variable is decalred in one source file and defined in another, using extern for the declaration can avoid redefining the same variable in multiple files. 
- Using extern for the declaration can make the code more modular, easier to maintaine and extend. 
8. 是否可以区分 默认构造函数、拷贝构造函数、赋值函数、析构函数？
	默认构造函数：A() Constructor 
	默认析构函数：~A()，Destructor only releases the space occupied by the class's regular data, but does not release the heap memory allocated by new or malloc. Therefore, sometimes a custom destructor is needed to release those memory. 
	默认拷贝构造函数：A(const A&)；
	默认拷贝是浅拷贝，只赋值变量的值，不赋值动态内存；自定义拷贝构造是深拷贝，赋值指向的动态内存，会为新对象分配新的内存
	默认赋值函数：A& operator = (const A&)
9. new & delete和malloc & free区别？
-  new/delete are C++ keyword that require complier support, where as malloc/free are library functions that requrie header file support. 
- new can automatically calculate the required memory space, while malloc needs to explicitly specify the size of the memory required. 
- When memory allocation with new is successful, it returns a pointer to the object type, requiring no type conversion, hence new is a type-safe operator, whereas malloc returns void* on successful memory lalocation, necessitating a type cast. 
- When the memory allocation failed on new, it would throw a bad_alloc exception; malloc would return NULL on failure. However, when using std::nothrow version of new, it wont throw execption but will also return a null pointer. 
- new first calls the operator new function to allocate enough memory, then calls the type's constructor. delete first calls the destructor, then the operator delete to release memoyr. malloc/free are library functions that can only dynamically allocated and release memory, can't forced to do custom type object constructio nand destruction work. 
- new/delete operators can be overloaded to customize object memory allocation and release. When using placement-new, it can provide provide a specified memory address for the object, allowing you to call the constructor on an existing memory block to complete object's initialization, malloc/free do not allow overloading. 
10. malloc和calloc区别？
- The memory block allocated by calloc will be initialized to 0. calloc function requries the number of required elements and the number of bytes per element.
	calloc分配的内存块中的每一位都被初始化为0，传入calloc的参数包括所需的元素的数量和每个元素的字节数
11. public private protected讲解下作用
	- public：Can be visited and used by any class. 
	- private：Can only be used by the same class. 
	- protected：Can only be visited or used by the same and derived class. 
12. 初始化列表内初始化，和在构造函数函数体内初始化，有什么区别？
	- What is the difference between initialization in an initializer list and initialization in a constructor's body? 
	- It is more efficient, as it directly constructs the member with the given value. Rather than first default constructing and assigning a value as it happens in the constructor body. 
	- 
	```c++
	class Example{
		int a; 
		std:: string str;
	public:
		Example(int x, const std::string& s) : a(x), str(s) {}
	};
	```
	- Initialization in the constructor body happens after the member has been default constructed. It is a more familiar syntax for those coming ferom languages without initializer lists.
	```c++
	class Exmaple{
		int a; 
		std::string str;
	public: 
		Example(int x, const std::string& s) {
			a = x;
			str = s; 
		}
	}
	```
	 
13. public继承和private继承的区别？
```c++

// Private inheritance, the public and protected memebers of the base class becomes private members of the derived class. 
// Private inheritance implies an "implement-in-terms-of" relationship. It is more about implementation reuse rather interface reuse. 
// For example, if you own a cake business. Imagine theres a recipe for a cake that is well known and used by many. But you have a secret family recepi that enhances it. You use the original recipe(private inheritance), and your secret ingredients, and the cake you make looks completely diffferent and may even taste better, but no one knows you started with the well known recipe.
// Public inheritance, think of you inherit a restaurant and its famous pizza recipe, but you also introudce some new dishes to the menu. Your customer know they can still get the classic family pizza, but now they have more options. 
```
14. overload和override区别？
```c++
// For overloading, it occurs when two or more functiosn in the same scope have the same name but different parammeters. 
void print(int i) {
	cout << "Printing int: " << i << endl; 
}
void print(double i) {
	cout << "Printing float: " << f  << endl; 
}

// For overriding, it happens in object-oriented programming when a subclass or derived class provide a specific implementation for a method already defined in its base class. This is a feature of runtime polymorhpism. 

class Base {
	public:
	 virutal void show() {
		cout << "Base class now" << endl;  
	}
};

class derived : public Base{
	public: 
		void show() override {
			cout << "Derived Class show " << endl; 
		}
};
```
15. C语言是否支持函数重载？为什么？
- C programming does not support function overloading. In C, the compiler handles function based on their names during compilation. Each function name is assigned a unique address by the compiler. In C, function identification relies solely on the name. 
- In C++, function overlaoding is achieved through name mangling, where the compiler generates a unique name for each function based on its name, parameter types, and other information. 
	 
16. 内联函数有什么好处？什么坏处？
- Inline functions can lead to faster execution time. The compiler attempts to expand the code of the function in place of the function call. This saves the overhead of a function call and return, which can be significant in small frequently called functions. 
- Disadvantages: It could lead to ***code bloat*** due to an increase in the binary size of the program. 
17. 什么时候适合用内联函数？
- When the function is small, and few lines long. When the function is frequently called. Complex recursive functions are generally not good candidates for inlining. 
18. const和宏替换的区别？
	const是C++中用于定义常量的关键字，在编译时进行类型检查
	宏替换是在预处理阶段进行的文本替换
19. 常量指针和指针常量区别？
- const pointer: `int* const p` refers to the address that the pointer points to cannot be changed, but the value on that address can be modified.
- pointer constant: `const int* p` refers to a pointer that points to a constant value, meaning that you cannot change the value of the variable pointed by p. But you could change where p points to. 
	常量指针是指向常量的指针，指针常量是指针本身是常量
20. 引用和指针的区别？
 - A pointer is a variable to hold the memory address of another varibale. It can be reassigned to point to different variables. It cannot be null, i.e point to nothing. Pointers are declared using asterik, and dereferenced using the same symbol to access the value. Pointer has its own memory address and size on the stack. 
 - A reference is an alias for another variable. Once established, a reference cannot be changed to refer to another variable. Declared using ampersand. Reference does not occupy additional memory, as it is just an alias of an existing variable. References cannot be reassigned .
- 
```c++
void increment(int &ref){
	ref++;
}
int main(){
	int var = 10;
	int *ptr; 
	ptr = &var; // example of pointer 

	increment(var); 
	return 0;
}
```
21. 是否了解C++四种强制类型转换？举出代码例子
	- `static_cast`
	 - Used for standard conversion of non-polymorphic types(between numeric types, pointers upcasts)
	 - 
	 ```c++
	 double pi = 3.1415926;
	 int intPi = static_cast<int>(pi); //convert double to int 
	 ```
	- `dynamic_cast`
	 - Primarily used with pointers and references to classes in a class hierarchy involving inheritance and polymorhpism. 
	 - It ensures the validity of the conversion at runtime, especially useful for downcasting. 
	 - If the cast fails, it returns `nullptr` for pointers and throws an exception for references. 
	 ```c++
	 class Base { virtual void dummy() {}};
	 class Derived : public Base {};
	 Base *basePtr = new Derived; 
	 Derived *derivedPtr = dynamic_cast<Derived*>(basePtr); //Downcasting 
	 ```

	- `const_cast`
	 - used to remove or add the `const` qualifier from a variable 
	 - It is the only c++ cast that can remove the const ness 
	 ```c++
	const int num = 10;
	int *modifiableNum = const_cast<int*>(&num);
	*modifiableNum = 20; // Modifying a const variable(unsafe, and not recommended)
	 ```
	- `reinterpret_cast`
	   - This kind of cast bypasses the C++ type system, allowing conversion between almost any pointer or integral types. This can lead to scenarios where memory is misinterpreted, breaking the safety net of strong typing.
 
22. static的作用？
	- static variable: If it is a static global variable, its scope is within the current source file, and cannot be accessed from other soruce files. If it is a locally declared variable, it is only initialized once in the program's lifetime.
	- static function: Turns the function into internal linkage, meaning the funcation can only be used within the current source file, not the other source files. 
	- static class member: All the instances of this class share this static class member. 
	- static class object
	 
23. static变量的作用域与生命周期？
	作用域：声明在函数之外时，在本源文件中都可以被访问，其他源文件不能访问；声明在函数内部时，仅在声明它的函数内部可见
	生命周期：在整个程序运行期间；在函数体内的static变量会在函数调用之间保留，即在函数调用之后不会被重置，而是保持其上一次函数调用结束时的值
