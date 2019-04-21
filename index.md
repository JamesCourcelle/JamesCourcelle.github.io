## Welcome to My Project Page

This project allowed me to convey on advanced understanding of data structures and algorithms. I demonstrated the ability to create effective classes, functions, and logical loops and conditionals. My functions utilize advance concepts like recursion and properly utilize local variables and returns when appropriate. This is clearly demonstrated in the functions to search, delete, print, and update for the BinarySearchTree class. The project also shows my attention to clear and effective software design. My python files are properly extracted and my file structure is clear and concise. I feel I abstracted code to appropriate classes and methods without abstracting too much. It demonstrated my understanding of creating easily understood and updateable code. Lastly, the project demonstrated my understanding of the functionality of a database, how to create one and implement basic features. This part of the project was the area I felt I had the least experience with and my ability to properly implement it shows my ability to learn new technology.

Although this project showcased many of the skills and knowledge I obtained, my time at SNHU provided much more than I can show in this project. My coursework gotten me acquainted with using Git for personal projects and team projects. This included reviewing other peer’s codes before a pull request was approved and providing feedback along with getting feedback on my own code. I also was exposed to Agile methodology and learned how to work in a team in that type of development environment. I learned the importance of manual and automated testing and how to develop with these tools to create more efficient code with less of a chance at having bugs and errors. 

Completing the capstone project for my Computer Science program has pulled together all the software design and engineering, data structures and algorithms, and database knowledge that I have gained throughout my time at SNHU. For my project, I took a lab project from my Data Structures and Algorithms course and converted it from C/C++ into Python. The project required that I build a program that took a comma separated value (csv) file and read it in into a form that I could parse and manipulate the data. The data provided was information of a municipal data for objects that were auctioned off. It included data on what the object was, what fund it was for, a unique identifying number and the auction fee. This is just a few of the fields and were the ones that were focused on for the project. To enhance the project further, I created the ability to remove rows from the data and export that new data into a csv file. I also created a database setup that allow the user to interact with a MongoDB server to achieve CRUD functionality, advance searching, and database export to a csv file. 

The software design and the data structures and algorithms artifacts were intertwined while the database artifact ran parallel to them. I encompassed all the artifacts into one Python file that can be ran to interact with both.

## Security
For my project I had no online interaction and many of the security concerns that come with developing in that space weren’t anything I needed to be concerned with. Python, unlike C/C++, has great garbage collecting and handling memory buffers. This was something handled by the language and I believe is one of the reasons it is such a great language to use. My one security concern that is particularly a concern with Python is that it is considered a weak type when it comes to whether or not variables are static. You do not need to declare what data type a variable is and it can be changed throughout the program. This isn’t something that can be done in languages like Java and C/C++. Those languages require that declare the data type when declaring the variable and does not allow you to easily change that later in the program. For this reason, taking user input in Python can cause unexpected behaviors if you do not validate its data type. By default, Python’s input() function reads everything in as a string. If you need to get an integer, you have to explicitly wrap the input() in an int wrapper. This is important when you are using user input for conditional statements. For my code, I created try/except blocks when taking in user input to validate that a integer was passed in. If the user inputted any nonnumerical characters, it would output and error message and prompt them to enter a valid response. This not only ensured integers were being entered, but that the program didn’t crash if there wasn’t


Please visit the repository page to view the current state of my [Final Capstone Project](https://github.com/JamesCourcelle/FinalProject).

-----------------------------------------------------------------

### Code Review
###### C++ code is not being presented as it is a SNHU course assignment

 [Project Code](https://github.com/JamesCourcelle/FinalProject).
 
 
#### Code Review Video

[Code Review Video](https://www.youtube.com/watch?v=9Ui4pQbeaEI)

--- WARNING ---
Due to technical issues, I had to change mic during recording and the sound goes from quiet to loud. This occurs around 11:50. There is a short pause.

#### Code Design Summary
My final project consists of three major aspects. I am to demonstrate my knowledge with software design/engineering, algorithms and data structures and databases. The software design/engineering and algorithms and data structures are intertwined and closely related in terms of how I will be demonstrating theses skills. 
The binary search tree that I have created is a balanced binary tree. I accomplished this by sorting a Python list of all the bids by bid ID. The binary tree in my C++ code is not a balanced binary tree lowering its efficiency for traversing as one side relative to the root is likely larger than the other. The point of sorting the bids in ascending order is to create the root node as the middle most bid ID number. With a balanced tree we have a complexity of 1 + log2(n) where n is the number of entries when traversing the tree. With my data file, there are 17938 entries. This means, including the root, the tree has a height of 15. It is here we can see the benefits it offers when traversing the tree. There is never anymore than 15 computations.  
The current project utilizes three classes, BinarySearchTree, Bid and Node. The Bid holds the bid information from the CSV file itself and the Node instantiates a Bid class object while also providing a left and right variable to act as pointers for the tree. The BinarySearchTree initializes with a root and a count variable and the methods associated with loading, displaying, searching and deleting bids are class methods to the Binary Search Tree. The large_file_search.py acts as the main program as it handles user interaction with the program. There are two files parse_csv.py and file_output.py which handle reading in and outputting data respectively.

#### Structure
Much of the left-over test code and now unused methods have been cleaned up, but further clean up will continue as development continues. There are still string literals for field names and file names that will be updated in the near future. Other than this, there are no string constants that would cause any concern other than messages outputted to the user when action is taken. The first iteration of the delete function was overly complex and wasn’t functioning correctly. The function has since been rewritten to be much simpler and provide intended behavior. Other pieces of code have been reworked to provide intended behavior and be much simpler. 

#### Documentation
Comments are clear and provide necessary insight to coding behavior although it is sparse to non-existent for some of the code. This will be updated to ensure proper documentation.

#### Variables
Variable names are consistent and properly convey their purpose through naming. Python does not have type casting and thus is not a concern. If time is adequate, I may apply type information to variables for the sack of clarity and expectations. In its current state, there are no redundant or unused variables in the program.

#### Arithmetic Operations
Actual mathematic computations are simple and no floating values are currently in use. This has ensured that no floating values are being compared for equality and there are no rounding errors. In its current state, there are some division operations that are not validated to ensure division of zeroes and noise are not present. This will be an added process to the code.

#### Loops and Branches
Loops and conditional branching are logically implemented. Common cases are tested first in conditional chains and base case operations are conducted at the start of methods as required. All cases are covered as necessary with conditional branching. Case statements do not exist in python like in many other languages. There is no concern of functionality in this regard. Loops termination conditions are clear and logical and ensure no instances of infinite loops. Indices are logically and adequately set up to avoid any out of bound errors or unexpected behavior. 

#### Defensive Programming
The fact that this program is not a web application avoids the concern of many common issues that these types of applications face. SQL injection is not a concern with MongoDB as it does not parse the same way that SQL does. Input validation in the python code is a concerns and additional work is required on the current code base to ensure this validation occurs. For instance, integer input requests do not stop strings from being entered. Further input validation is required to ensure buffer overflows do not occur. Python has built-in protection by throwing an index out of bounds error, but behavior can be unpredictable.
Python has built in trash collecting and allocating and deallocating manually are not needed. File checks do occur before attempting to access and file output operations are properly closed using the ‘open with’ function to handle the CSV files.

#### Conclusion
The current state of the project is making good progress. The need to satisfy the software design and algorithms and data structures are nearly complete. There is still a need to integrate the necessary code for the MongoDB database. The current plan is to integrate CRUD operations and more advanced searching options. This aspect of the project will have some overlap with the current project, at least in using the same data, but may end up being a separate artifact.

-----------------------------------------------------------------

## Software Design/Engineering

[Software Design/Engineering](SoftwareDesignEngineering.md)

I began to work on this project back in August of 2018. Most of the work has focused on the software design/engineering and the data structures and algorithms. For the enhancement one. I have decided to take the binary search tree lab from the Data Structures and Algorithm course and convert it into Python while adding some additional features. In the current artifact, everything but the delete feature has been converted into Python. My features I plan to add are outputting files with the desired fields and parsed data allowing the user to get more meaning from the CSV file. The goal of this artifact is to showcase my understanding of data structures like binary search trees, Python lists and dictionaries along with showcasing my understanding of complex algorithms through recursive tree building and tree traversal. If time permits, I will build an algorithm to sort the CSV passed in, but in its current state I am relying on the built-in python sort function. Ultimately this project parses large and small data files to search and display data in a meaningful way.

I feel this artifact is worth adding to my ePortfolio as it demonstrates my ability to fully understand the software design. I was capable of converting the C++ code, which a lot of it was provided for the assignment, into a different language. I wrote the necessary code to parse a CSV file which in the Data Structures and Algorithms course was provided to us with no contribution from us. I show an understanding of OOP concepts like abstracting code into small building block for reuse and clarity, building classes for instantiation and building recursive functions to name a few. Although I haven’t included additional features, I have mapped out what I would like to include, and I have a plan of how to properly implement it. 


-----------------------------------------------------------------

## Algorithms and Data Structures

[Algorithms and Data Structures](AlgorithmsDataStructures.md)

The enhancement for the algorithms and data structures is closely integrated with the first enhancement for software design. For this enhancement I translated and recreated the algorithms from the binary search tree project C++ code into Python. This artifact had the creation of reading in a CSV file, creating a binary search tree from the data, displaying all the data in proper format, searching for an entry by a bid ID, and the deletion of bid based on a bid ID entered by the user. The creation of this artifact started in August of 2018, however much the work has been completed of the last few weeks with most over the last three days.

I used this as my artifact for algorithms and data structures as I feel it showed not only my competency with basic algorithms that utilize simple loops and conditionals but also more advanced data structures and algorithms like the Binary search tree and recursive functions to manipulate the binary tree structure. I chose this artifact as I enjoyed learning about and building the binary search tree. I also feel like it shows my understanding of complex algorithms and data structures and showcases the skills and knowledge that I have gained since I started my degree program.

To enhance the program’s algorithms, I have created code that reads CSV files into a usable python list of dictionary objects that then can be used to create the binary search tree. I have also created code to output a formatted file that only includes the fields taken in from the initial file read in. Not only does this format the CSV file with the desired fields but if deletion is completed before the file creation, the file output with include this updated state. It is important to note that my deletion and file input of large files were not functioning correctly in my C++ code. This was not discovered until I more thoroughly tested my Python code. For these reasons I had to rewrite a few of my functions to get the proper behavior. This wasn’t exactly an enhancement, but the deletion algorithm is simpler, and functions as intended.

I feel this project has been enriching and highly enjoyable. Its one of the first times that I can strongly notice an increase in my knowledge and develop skills since I created the C++ code. I have learned that testing is extremely important and to really think about how a piece of code should tested as I missed issues with my C++ code that rendered it non-functional. This in and of itself has provided experience on correcting code. It has also provided better insight into how recursion works. I still struggle to sometimes conceptualize its process but feel I have gained a better understanding with having to rewrite and fix the deletion function. 


-----------------------------------------------------------------

## MongoDB Creation
[Database Creation Process](MongoDB.md)

This artifact was the artifact that I had the least confidence with. For this artifact I implemented a MongoDB database to accommodate some features of my current two artifacts. I initially worked with MongoDB in Advanced Programming Concepts last term. The database runs parallel with the local file using the same data, but not interacting as MongoDB handles all of the functionality that the software design and algorithms and data structures artifacts handle. The current code handles the launching of the MongoDB server, creating an index, creating a new document, finding and reading a document, updating an existing document, and deleting and existing document. The access to the database has been added to the main menu and prompts the user to choose between working on a local file (artifact one and two) and the database when it is launched.

The inclusion of this artifact demonstrates my strong understanding of how-to setup and manipulate a MongoDB database. The integration of using PyMongo to complete operations allows for a robust control that allows the user to interact with the database without needing to interact with the MongoDB shell or launch the MongoDB server. I chose to use MongoDB because of its ease of use and the API that allows me to use python to interact with it. I was able to create CRUD commands to handle basic manipulation. Additionally, I created ease of use functionality by using the API in Python to run the executables for mongod.exe and the mongoimport.exe. 

I feel I have met most of the planned objectives I made from module one. I have had to adjust what I will be completing regarding the RESTful API and interface. The project has gotten away from that direction. I have enhanced the database artifact by integrating it with my current project, creating the import to CSV function and writing a method to launch the MongoDB Server. Over the final week of the project I hope to add more functionality in the ways a user can search the database.

Enhancing this artifact by adding features and integrating it with my current python project was challenging. Some of the methods from the Advanced Programming Concepts course were deprecated, but easy enough to swap out. I did have to alter the use of my written methods to properly work in my program. Additional to getting functionality with my code, I also ensured input validation to ensure security and continued functionality. This step of the project has taught me a lot and I have become much more comfortable with using MongoDB and writing API. I learned how to use the subprocess Python module to run OS operations to launch exe files. I hope to add more search functionality by the final product allowing the user to search specific value ranges.

