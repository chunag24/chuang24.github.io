---
title: Smart Pointer
subtitle: This is a post about how to use smart pointer in C++

# Summary for listings and search engines
summary: We all know C++ raw pointers are troublesome, read this post to understand how we can use smart pointers to better manage the memory. 

# Link this post with a project
projects: []

# Date published
date: '2020-12-30T00:00:00Z'

# Date updated
lastmod: '2020-12-30T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

authors:
  - admin
  

tags:
  - Academic
  - C++
   

categories:
  - 教程
---

#### Understanding RAII
Resource Acquisition Is Initialization (RAII) is a programming concept that binds the life cycle of a resource (memory, network sockets, file handles) to the lifetime of an object. RAII ensures that resources are acquired during object creation and released when the object is destroyed. This is particularly useful in C++, where constructors acquire resources and destructors release them, providing exception safety and avoiding resource leaks by ensuring that resources are always released when the scope ends, regardless of how the scope is exited (normal execution or due to an exception).

- Resource Acquisition
  - Open a file 
  - Allocate memory
  - Acqurie a lock
- Is initialization 
  - The resource is acquired in a constructor 
- Resource relinquishing
  - Destructor would close the file 
  - deallocate the memory
  - release the lock

##### Issues with Raw pointers
- Uninitialized pointers 
- memory leaks 
- Dangling pointers 
- Not exception safe 

#### Types of Smart pointers 
- unique_ptr
  - simple smart pointer, very efficient. 
  - unique_ptr<T> points to an object of type T on the heap.
  - Pointer is unique, and owns what it points to, cannot be assigned or copied. 
  - Can be moved using move semantics 
  - Object get destroyed whenever the pointer is destroyed. 
```c++
// Initializing and using 
 {
      std::unique_ptr<double> p1 {new double {1.1}};
      std::unique_ptr<string> s1 {new string {"hello"}};
      cout << *p1 << endl; 
      cout << *s1 << endl;
  }

 {
        unique_ptr<int> p1 {new int {200}};
        cout << p1.get() << endl; //0x6000015dc040
        p1.reset();
  }

  //in c++14, use make_unique
  {
    std::unique_ptr<int> p1 = make_unique<int>(100);
  } 
```
- shared_ptr
  - Points to an object of type T on the heap 
  - there can be many shared_ptrs pointo to the same object on the heap.
  - CAN be assigned, copied, and moved. 
  - When .use_count() is 0, managed object on the heap get destroyed.

```c++
{
  shared_ptr<int> p1 {new int {100}};
  cout << p1.use_count() << endl; //1 
  shared_ptr<int> p2{p1}; //shared ownership
  cout << p1.use_count(); //2
  p1.reset();
  p1.use_count(); // 0
  p2.use_count(); //1


  // make_shared(c++11)
  shared_ptr<int> p1 = make_shared<int>(100); //usecount 1
  shared_ptr<int> p2 = p1;//2
  shared_ptr<int> p3 = p1;//3
}
```

- weak_ptr 
  - Does not participate in owning relationship
  - Does not increment/ decrement use count
  - Used to prevent strong reference cycles which could prevent objects from deleted
  
Example below shows that Object A and B, which were instantiated from 2 different classes. They both has a shared pointer pointing ot each other. When they go out of scope, their shared resources on the heap will not be destroyed and will cause memory leaks. (A keeps B alive, and B keeps A alive)
![screen reader text](weakptr1.png "Figure 1 Strong ownership, chicken and egg problem ")

With weak_ptr, we can decide A owns B, and we can replaced the shared pointers in B with a weak pointer. 
![screen reader text](weakptr2.png "Figure 2 Weak_ptr make one pointer non-owning")

Check code for above demo: 

```c++
using namespace std;

class B; //forward declaration 

class A{
  std::shared_ptr<B> b_ptr;
  public:
    void set_b(shared_ptr<B> &b){
      b_ptr = b;
    }
    A(){cout << "A Constructor" <<endl;}
    ~A() {cout << "A Deconstructor";}
};


class B{
  std::shared_ptr<A> b_ptr; // -> Change it to weak pointer, would affect the reference count now. Destructor can now be called.
  public:
    void set_b(shared_ptr<A> &a){
      a_ptr = a;
    }
    B(){cout << "B Constructor" <<endl;}
    ~B() {cout << "B Deconstructor";}
};

int main(){
  shared_ptr<A> a = make_shared<A>();
  shared_ptr<B> b = make_shared<B>();
  a->set_B(b);
  b->set_A(a);

  return 0;
}
```

#### Custom deleter 
```c++
void my_deleter(Test *ptr){
  cout << "Using my custom function deleter" << endl;
  delete ptr;
}

int main(){
  {
    std::shared_ptr<Test> ptr1 {new Test{100}, my_deleter};
  }

  //Could also do this in lambda expression
  std::shared_ptr<Test> ptr2 (new Test{10000}, 
      [](Test *ptr){
        cout << "Using my custom lambda deleter" << endl;
        delete ptr;
  })

}
```
### Exercise
```c++

#include <iostream>
#include <memory>
#include <vector>

class Test {
private:
    int data;
public:
    Test() : data{0} { std::cout << "\tTest constructor (" << data << ")" << std::endl; }
    Test(int data) : data {data} { std::cout << "\tTest constructor (" << data << ")" << std::endl; }
    int get_data() const {return data; }
    ~Test() {std::cout << "\tTest destructor (" << data << ")" << std::endl; }
};

// Function prototypes
std::unique_ptr<std::vector<std::shared_ptr<Test>>> make(){
      return make_unique<vector<shared_ptr<Test>>>();
}
void fill(std::vector<std::shared_ptr<Test>> &vec, int num){
  for (auto i = 0; i < num; i++){
         cout << "Enter an integer for vec[" << i << "]:" ;
         int number;
         cin >> number;
         vec.push_back(make_shared<Test>(number));
     }
}
void display(const std::vector<std::shared_ptr<Test>>&vec){
   for (auto &obj : vec){
        cout << obj->get_data() << endl;
    }
}


int main() {
    std::unique_ptr<std::vector<std::shared_ptr<Test>>> vec_ptr;
    vec_ptr = make();
    std::cout << "How many data points do you want to enter: ";
    int num;
    std::cin >> num;
    fill(*vec_ptr, num);
    display(*vec_ptr);
    return 0;
}

```