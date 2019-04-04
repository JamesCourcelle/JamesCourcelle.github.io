## Welcome to My Project Page

Hello, my name is James. I am currently completing my final term to obtain my Bachelor of Science in Computer Science at Southern New Hampshire University. For my final capstone, I am converting a code base previously written by me in C++ into Python. This code is a program that parses a CSV file and provides the options to load the file, display all each row of data, find a specific entry based on a unique ID number, and remove a bid. Not only will I be converting the code into Python, but I will be adding additional features along with a MongoDB database.

Please visit the repository page to view the current state of my [Final Capstone Project](https://github.com/JamesCourcelle/FinalProject).

-----------------------------------------------------------------

### Code Review
###### C++ code is not being presented as it is a SNHU course assignment
 [Project Code](https://github.com/JamesCourcelle/FinalProject).
###### Code Design Summary
My final project consists of three major aspects. I am to demonstrate my knowledge with software design/engineering, algorithms and data structures and databases. The software design/engineering and algorithms and data structures are intertwined and closely related in terms of how I will be demonstrating theses skills. 
The binary search tree that I have created is a balanced binary tree. I accomplished this by sorting a Python list of all the bids by bid ID. The binary tree in my C++ code is not a balanced binary tree lowering its efficiency for traversing as one side relative to the root is likely larger than the other. The point of sorting the bids in ascending order is to create the root node as the middle most bid ID number. With a balanced tree we have a complexity of 1 + log2(n) where n is the number of entries when traversing the tree. With my data file, there are 17938 entries. This means, including the root, the tree has a height of 15. It is here we can see the benefits it offers when traversing the tree. There is never anymore than 15 computations.  
The current project utilizes three classes, BinarySearchTree, Bid and Node. The Bid holds the bid information from the CSV file itself and the Node instantiates a Bid class object while also providing a left and right variable to act as pointers for the tree. The BinarySearchTree initializes with a root and a count variable and the methods associated with loading, displaying, searching and deleting bids are class methods to the Binary Search Tree. The large_file_search.py acts as the main program as it handles user interaction with the program. There are two files parse_csv.py and file_output.py which handle reading in and outputting data respectively.

###### Structure
Much of the left-over test code and now unused methods have been cleaned up, but further clean up will continue as development continues. There are still string literals for field names and file names that will be updated in the near future. Other than this, there are no string constants that would cause any concern other than messages outputted to the user when action is taken. The first iteration of the delete function was overly complex and wasn’t functioning correctly. The function has since been rewritten to be much simpler and provide intended behavior. Other pieces of code have been reworked to provide intended behavior and be much simpler. 

###### Documentation
Comments are clear and provide necessary insight to coding behavior although it is sparse to non-existent for some of the code. This will be updated to ensure proper documentation.

###### Variables
Variable names are consistent and properly convey their purpose through naming. Python does not have type casting and thus is not a concern. If time is adequate, I may apply type information to variables for the sack of clarity and expectations. In its current state, there are no redundant or unused variables in the program.

###### Arithmetic Operations
Actual mathematic computations are simple and no floating values are currently in use. This has ensured that no floating values are being compared for equality and there are no rounding errors. In its current state, there are some division operations that are not validated to ensure division of zeroes and noise are not present. This will be an added process to the code.

###### Loops and Branches
Loops and conditional branching are logically implemented. Common cases are tested first in conditional chains and base case operations are conducted at the start of methods as required. All cases are covered as necessary with conditional branching. Case statements do not exist in python like in many other languages. There is no concern of functionality in this regard. Loops termination conditions are clear and logical and ensure no instances of infinite loops. Indices are logically and adequately set up to avoid any out of bound errors or unexpected behavior. 

###### Defensive Programming
The fact that this program is not a web application avoids the concern of many common issues that these types of applications face. SQL injection is not a concern with MongoDB as it does not parse the same way that SQL does. Input validation in the python code is a concerns and additional work is required on the current code base to ensure this validation occurs. For instance, integer input requests do not stop strings from being entered. Further input validation is required to ensure buffer overflows do not occur. Python has built-in protection by throwing an index out of bounds error, but behavior can be unpredictable.
Python has built in trash collecting and allocating and deallocating manually are not needed. File checks do occur before attempting to access and file output operations are properly closed using the ‘open with’ function to handle the CSV files.

###### Conclusion
The current state of the project is making good progress. The need to satisfy the software design and algorithms and data structures are nearly complete. There is still a need to integrate the necessary code for the MongoDB database. The current plan is to integrate CRUD operations and more advanced searching options. This aspect of the project will have some overlap with the current project, at least in using the same data, but may end up being a separate artifact.


