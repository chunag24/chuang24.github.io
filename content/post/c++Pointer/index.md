---
title: Pointers and References 
subtitle: This is a post about how to use pointers and references in C++

# Summary for listings and search engines
summary: Welcome 👋 We know that first impressions are important, so we've populated your new site with some initial content to help you get familiar with everything in no time.

# Link this post with a project
projects: []

# Date published
date: '2020-12-13T00:00:00Z'

# Date updated
lastmod: '2020-12-13T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
  

tags:
  - Academic
  - C++
   

categories:
  - 教程
---

### Why do we want to use it in the first place? 
- Pointers can be used to access data outside a function. 
- Pointers can be used to operate on arrays efficiently.
- Within Object Oriented Programming, pointers are are how polymorphism works.
- Allocate memory dynamically 
- Access specific addresses in memory(embedded and system applications)

#### Declaring pointers 

```c++
int * int_ptr;
double* double_ptr;
char* char_ptr;
string* string_ptr;

int * null_ptr {nullptr};
```
- Always initialize pointers 
- Uninitialized pointers could contain garbage data and point anywhere 
- Initalize to zero or nullptr, implies the pointer is point nowhere 
- Pointer is represented by hexadecimal, which is base of 16

#### Dereferencing a pointer

```c++
int score = 100;
int *score_ptr = &score;

cout << *score_ptr << endl; // 100

*score_ptr = 200;

cout <<*score_ptr; //200
cout << score; //200

```

```c++
//another example
vector<string> stooges{"larry", "moe", "curly"};
vector<string> * vector_ptr;
vector_ptr = &stooges;
cout << "First stooge:" << (*vector_ptr)[0] << endl;
```

#### Dynamically allocate memory on the heap

```c++
double * temp_ptr = nullptr;
size_t size = 0;
cout << "How many temps?" << endl;
cin >> size;

temp_ptr = new double[size];

delete [] temp_ptr;
```
#### Arrays vs Pointers 
```c++
 int scores[] = {100, 95, 89};

int * scores_ptr = scores;//no need for &, since scores is already an address
cout << "Value of scores_ptr" << scores << endl;

cout << "Array subscript notation" << endl;
cout << scores[0] << endl;
cout << scores[1] << endl;
cout << scores[2] << endl;

cout << "Pointer subscript notation" << endl;
cout << scores_ptr[0] << endl;
cout << scores_ptr[1] << endl;
cout << scores_ptr[2] << endl;

cout << "Pointer offset notation" << endl;
cout << *scores_ptr << endl;
cout << *(scores_ptr + 1) << endl;
cout << *(scores_ptr + 2) << endl;

cout << "Array offset notation" << endl;
cout << *scores << endl;
cout << *(scores+1) << endl;
cout << *(scores+2) << endl;
```

#### Pointer Arithmetic 
```c++
int_ptr++;
int_ptr--;
int_ptr += n; // increment pointer by n * sizeof(type)
int_ptr -= n;
//subtracting two pointers, to determine the elements in between 
int n = ptr2 - ptr1;
```
Example below, stop when you see a negative number in an array
```c++
int scores[] = {100, 95, 89, 69, -1};
int *scores_ptr = scores;
while(*scores_ptr != -1){
    cout << *score_ptr << endl;
    score_ptr++; // increment the address 
}
//another way of writing the above code is 
while(*score_ptr != 1){
  cout << *score_ptr++ <<endl; //derefernece the pointer and then increment it 
}
```
Example below, character array with pointer arithmetic.
```c++
char names [] = "Frank";
char * char_ptr1 = &names[0]; // char_ptr1 0x16fb23474 "Frank"
char * char_ptr2 = &names[3]; // char_ptr2 0x16fb23477 "nk" 
cout << char_ptr2 << endl;
cout << *char_ptr2<< "is " << (char_ptr2-char_ptr1) << "away from" << *char_ptr1 << endl;
// n is 3 away from F
```

Exercise: Swap values using pointers 
```C++
//Write a fucntion swap_pointers to swap the values of two integers pointers 
void swapPointers(int* ptr1, int* ptr2){
    // ptr1 and ptr2 were passed by reference
    int temp = *ptr1; //dereference ptr1 and store value in int temp
    *ptr1 = *ptr2;
    *ptr2 = temp;
}

void swapPointer2(int* ptr1, int* ptr2){
  //use pointer addition 
  *ptr1 = *ptr1 + *ptr2; //store the sum of both value into the address ptr1 points to
  *ptr2 = *ptr1 - *ptr2; // Subtract the value of ptr2 from sum to get value of ptr1
  *ptr1 = *ptr1 - *ptr2; // Subtract the value of ptr1 from sum to get value of ptr2
}
```


#### const and pointers 
There are several ways to qualify pointers using const
- Pointers to constant
- Constant pointers 
- Constant pointers to constants 

```c++
// Data pointed to by the pointer is constant can cannot be changed
int high_score = 100;
int low_score = 65;

const int* score_ptr = &high_score; 
*score_ptr = 86; //Error 
score_ptr = & low_score; //ok

// Constant pointers 
int* const  score_ptr = &high_score;
*score_ptr = 86; // ok
*score_ptr = &low_score; //error

// constant pointers to constants 

const int* const  score_ptr = &high_score;
*score_ptr = 86; // error
*score_ptr = &low_score; //error

```

#### Return pointer from a function 
```c++
//write a function that returns a pointer to an array
int *creat_array(size_t size, int int_value) {

  int * new_storage = new int[size];
  for (size_t i = 0; i < size; ++i)
    *(new_storage + i) = int_value;
  return new_storage;
}

void display(const int * const array, size_t size) {
  for (size_t i{0}; i < size; ++i){
    cout << array[i] << endl;
  }
}

//Exercise 30:
void multiply_with_pointer(int* ptr, int multiplier){
    *ptr = *ptr * multiplier
}

//Exercise 32: Reverse array using pointers 
void reverse_array(int* array, int size){

    // Given an array, reverse it
    // You want to initialize two pointers to  this array, start and end.
    int* start = array; // 1
    int* end = array + size - 1; // 5
    while (start < end){
        int temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }
}

//Exercuse 33: Reverse string using pointers
string reverse_string(const string& str){
    const char* start = str.c_str();
    const char* end = str.c_str() + str.size() - 1;

    string reversed;
    while(end >= start){
        reversed.push_back(*end);
        end--;
    }
    cout << reversed;
    return reversed;
}

```
##### Potential pointer pitfalls 
- Uninitialized pointer 
- Dangling Pointers (2 pointers point to the same data, while one pointer release the data with delete, the other one still accessing)
- Not checking if new failed to allocate memory (exception will be thrown if new failed)
- Memory leak (Forget to delete allocated memory)


##### Understanding Reference 
- An alias for a variable. 
```c++
vector<string> stooges {"Larry", "Moe", "Curly"};
for (auto str: stooges)
  str = "Funny"; // str would just be a copy of each vector element


for (auto &str: stooges)
  str = "Funny"//str is a reference to each vector element now.
```

##### L-value and R-value 
L-values 
- values that have names and are addressable 
- modifiable if they are not cosntants 
R-values 
- A temporary which is intented to be non-modifable 
L-value references 
- The references we have used are lvalue referneces 
```c++
int &ref = 100; // is not allowed, because 100 is an r-value
```

