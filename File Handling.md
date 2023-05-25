****File Handling****

[File Handling](https://www.scaler.com/topics/cpp/file-handling-in-cpp/)

- **What is File Handling in C++?**

The basic entity that stores the user's relevant data is called a file. Files can have a lot of types that are depicted by their extensions. For example : .txt(text file), .cpp(c++ source file), 
.exe(executable file), .pdf(portable document file) and many more

**File Handling** stands for the manipulation of files storing relevant data using a programming language, which is C++ in our case. This enables us to store the data in permanent storage even
after the program performs file handling for the same ends of its execution. C++ offers the library fstream for file handling. Here we will discuss the classes through which we can perform 
I/O operations on files. For taking input from the keyboard and printing something on the console, you might have been using the `cin`(character input stream) and `cout`(character output stream)
of the `istream` and `ostream` classes. File streams are similar to them. Only the console here is replaced by a file.

Below are three stream classes of the fstream library for file handling in C++ that are generally used for file handling in C++.

- **ofstream**
The ofstream is derived from the `ostream class`. It provides the output stream to operate on file. The output stream objects can be used to write the sequences of characters to a file. 
This class is declared in the `fstream` header file.

- **ifstream**
The ifstream is derived from the `istream class`. It provides the input stream to operate on file. We can use that input stream to read from the file. This class is declared in the `fstream` header 
file.

- **fstream**
The fstream is derived from the iostream class, and the iostream is further derived from the istream and ostream classes. It provides the input as well as output streams to operate on file. 
If our stream object belongs to the `fstream class`, we can perform read and write operations on the file with the same `stream object`. This class is declared in the `fstream` header file.


- **Opening a File**

To start working with the file, first, we need to open it in our program. We can either open our file with the constructor provided by the file I/O classes or call the open method on the 
stream object. Before discussing how the file can be opened, it is necessary to discuss several opening modes.

- **Opening Modes**

![image](https://github.com/golukr42220/Object-Oriented-Programming/assets/45598340/c79bf304-d328-4eb0-b1ed-d17a20c0fe45)

**1. Open a File Using Constructor**

Each class has two types of constructors: default and those that specify the opening mode and the associated file for that stream.

```cpp
ifstream Stream_Object(const char* filename, ios_base::openmode = ios_base::in);
```
```cpp
ofstream Stream_Object(const char* filename, ios_base::openmode = ios_base::out);
```
```cpp
fstream Stream_Object(const char* filename, ios_base::openmode mode = ios_base::in | ios_base::out);
```

**2. Open a File Using stream.open() Method**


The open() is a public member function of all these classes. Its syntax is shown below.

```cpp
void open (const char* filename, ios_base::openmode mode);
```

The open() method takes two arguments: the file name and the mode in which the file will open.

The is_open() method is used to check whether the stream is associated with a file. It returns true if the stream is associated with some file; otherwise returns false.
```cpp
bool is_open();
```
There are several modes we can specify when opening a file. These modes correspond to various controls given to stream objects during file handling in c++. The opening mode's description 
and syntax are in the tabular form below.

****Reading from a File****

We read the data of a file stored on the disk through a stream. The following steps must be followed before reading a file,

Create a file stream object capable of reading a file, such as an object of ifstream or fstream class.

```cpp
ifstream streamObject;
// Or
fstream streamObject;
```

Open a file through the constructor while creating a stream object or calling the open method with a stream object.
```cpp
ifstream streamObject("myFile.txt");
// Or
streamObject.open("myFile.txt");
```

// Note:- If a stream is already associated with some file, then the call to open method will fail.
Check whether the file has been successfully opened using is_open(). If yes, then start reading.
```cpp
if(streamObject.is_open()){
    // File Opened successfully.
}
```

**1. Using get() Method**

```cpp
#include <fstream>
#include<iostream>

int main () {
    std::ifstream myfile("sample.txt");
    if (myfile.is_open()) {
        char mychar;
        while (myfile.good()) {
            mychar = myfile.get();
            std::cout << mychar;
        }
    }
    return 0;
}
```

**Output:**

```
Hi, this file contains some content.
This is the second line.
This is the last line.
```

**Explanation:**

- First, we have created a stream object of the ifstream class and are providing the file name to open it in reading mode(default).
- Next, we check whether the file is being opened successfully. If yes, we read one character at a time until the file is good.
- The good() function returns true if the end of the file is not reached and there is no failure.

**2. Using getline() Method**

```cpp
#include <fstream>
#include<iostream>
#include<string>

int main () {
    std::ifstream myfile("sample.txt");
    if (myfile.is_open()) {
        std::string myline;
        while (myfile.good()) {
            std::getline (myfile, myline);
            std::cout << myline << std::endl;
        }
    }
    return 0;
}
```

**Output:**

```
Hi, this file contains some content.
This is the second line.
This is the last line.
```

**Explanation:**

- At the beginning of the program, we opened the file with the constructor of the ifstream class.
- If the file is successfully opened, the 'open' function will return true, and the if block will be executed.
- In the while loop, we check whether the file stream is good for operations. When the end of the file is reached, the good function will return false.
- We have declared a string to store each line of file through the getline function, and later we are printing that string.

****Writing to a File****

In writing, we access a file on disk through the output stream and then provide some sequence of characters to be written in the file. The steps listed below need to be followed in writing a file,

Create a file stream object capable of writing a file, such as an object of ofstream or fstream class.
```cpp
ofstream streamObject;

// Or
    
fstream streamObject;
```

Open a file through the constructor while creating a stream object or calling the open method with a stream object.

```cpp
ofstream streamObject("myFile.txt");

// Or

streamObject.open("myFile.txt");
```

Check whether the file has been successfully opened. If yes, then start writing.

```cpp
if(streamObject.is_open()){
    // File Opened successfully.
}
```

**1. Writing in Normal Write Mode**

```cpp
#include <fstream>
#include<iostream>
#include<string>

int main () {
    // By default, it will be opened in normal write mode, ios::out.
    std::ofstream myfile("sample.txt");
    
    myfile << "Hello Everyone \n";
    myfile << "This content was being written from a C++ Program";
    return 0;
}
```

**Output:**
```
Hello Everyone 
This content was written from a C++ Program.
```

**Explanation:**
```
<< operator is used for writing on the file.
The text given above will be shown in our sample.txt after running the program.
```

**2. Writing in Append Mode**

```cpp
#include <fstream>
#include<iostream>
#include<string>

int main () {
    std::ofstream myfile("sample.txt", std::ios_base::app);
    
    myfile << "\nThis content was appended in the File.";
    return 0;
}
```

**Output:**

```
Hello Everyone 
This content was written from a C++ Program
This content was appended in the File.
```

**Explanation:**

The same sample.txt, which was used in the last example, has more content appended to it now.

**3. Writing in Truncate Mode**

```cpp
#include <fstream>
#include<iostream>
#include<string>

int main () {
    std::ofstream myfile("sample.txt", std::ios_base::trunc);
    
    myfile << "Only this line will appear in the file.";
    return 0;
}
```

**Output:**

```
Only this line will appear in the file.
```

**Explanation:**

Again, we use the same sample.txt file from the previous Example. Now all older content is being removed.
