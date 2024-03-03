---
title: STL library
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
- std::array C++11
	- #include <array>
	- Fixed size, size must be known at compile time. 
	- Direct element access 
	- Iterators do not invalidate. 
-  std::array<int, 5> arr {1, 2, 3, 4, 5};
-  arr.size() 
-  arr.at(0), arr[1]
- arr.front(), arr.back() 
- arr.data() returns the raw array address 


- std::vector
- dynamic size, direct element access, rapid insertion and deletion
- insertion or removal of elements(linear time) 