> # DataStructuresAnalysis

### Purpose
This repository showcases my work completed for the DSA: Analysis and Design course @ SNHU. In this course we studied and analysed four data structures (array, linked-list, Hash Table, & Binary Search Tree). The goal is to determine the best data structure to store a list of courses for the advisors of a university academic department. 

### Approach
The requirements for the program were: 
- Check a csv file for proper format
- Read course data from the csv file and insert each course object into a courses data structure
- Search data structure for a specific course by course number
	- Print course and, if the course has any, prerequisite courses
- Print courses in alpha numerical order
With these requirements in mind and a thorough understanding of each data structure we conducted a worse case [runtime analysis](./RuntimeAnalysis.md) of each structure. Based on the runtime analysis we made recommendations for the best data structure to use in the program.

### Implementation
I chose the Binary Search Tree(BST) as the best data structure primarily due to its rapid ordering capabilities. The zybook for this course was my essential source for implementation for the duration of the course. However, I did struggle on certain aspects of recursion. [GeeksforGeeks](https://www.geeksforgeeks.org/binary-search-tree-data-structure/) help bridge the gap in my understanding and supplimented zybooks for the especially confusing applications fo recursion. For this project I wanted to create something that could be used for more applications than the program for the advisors so I created a template for a BST class that can store different kinds of objects within nodes. This posed an extra level of difficulty where I heavily realied on [Microsoft](https://learn.microsoft.com/en-us/cpp/cpp/templates-cpp?view=msvc-170), [Devdocs](https://devdocs.io/cpp/language/class_template), [GeeksforGeeks](https://www.geeksforgeeks.org/cpp/templates-cpp/), and [CppReference](https://en.cppreference.com/w/cpp/language/templates.html) to produce the template. 

## Lessons Learned
Creating the runtime analysis made me think about creating programs differently, especially when working with large data sets. I now consider hardware resources and time effeciency as a foundation for good programming. Understanding techniques, data structures, and implementation standards are essential in creating a program that both meets requirements and run effieciently. 
Creating the Template BST class also taught me what is important when creating reusable code when types are undefined. 
