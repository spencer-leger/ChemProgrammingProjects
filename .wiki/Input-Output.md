The code examples you've seen so far in this tutorial have used the cout object, which automatically writes to the screen. However, one often wishes to write to files for longer-term storage. In addition, one often finds it necessary to read from existing files, e.g. to obtain data necessary for a computation.

There are two fundamental types of file-based input/output (I/O): formatted and unformatted. Formatted means that the data is written to/read from the file as integers, doubles, characters, etc., while unformatted means that the data is handled as raw bytes and (perhaps) interpreted later. Formatted files are convenient because they can be read by simple text editors like vi or emacs (or even Microsoft Word!), while unformatted files appear as gibberish to such programs. On the other hand, unformatted files are usually much more compact than formatted files, and they store numerical data with no additional loss of precision. In this section, we focus on formatted I/O as it's easier to learn and will get you started.

## Header files: cstdio and iostream
To use the functionality described here in a C++ program, you must use the standard I/O header file:
```c++
#include <iostream>
#include <fstream>  // necessary for formatted I/O
```
However, it is also possible to make use of older (but often more convenient) C-language output:
```c
#include <cstdio>
```
which provides declarations for functions like printf() that will be described below.

## File Streams
In C++-based I/O, writing to or reading from files involves special objects called "streams" of type "ifstream" (input stream) or "ofstream" (output stream). Three special C++ streams are:
<table>
  <tr>
    <td><b>cin</b></td>
    <td>Stream for input to a program. This can be keyboard input or input from a "<" redirect, for example.</td>
  </tr>
  <tr>
    <td><b>cout</b></td>
    <td>Stream for output from a program. This can be to the screen or output from a ">" redirect, for example.</td>
  </tr>
  <tr>
    <td><b>cerr</b></td>
    <td>Stream for error or diagnostic information.</td>
  </tr>
</table>

In C-style I/O, the corresponding streams "stdin", "stdout", and "stderr" are of the type "FILE *".

A nice Wikipedia page on standard streams [exists](https://en.wikipedia.org/wiki/Standard_streams)

## Opening and Closing File Streams
### C++ Style
Before you can read or write data, you must first open the desired file stream. If you intend only to read from a given file, the simplest approach declare the stream to be of type “ifstream” and including the name of the file with the declaration:
```c
ifstream my_input("data_file");

// Do something with the file here

my_input.close();
```
If you intend only to write to the file, use “ofstream” instead of “ifstream”. Alternatively, the more general approach is to declare the access mode explicitly:
```c
ifstream my_input("data_file", ios::out | ios::app);
  
// Do something with the file here
  
my_input.close();
```
The name of the file stream is “my_input”, while the physical file is “data_file”. The “ios::out | ios::app” indicates that the file is opened for output, and we will write all new data starting at the end of the file (“append”). Selected C++ access modes are:
<table>
  <tr>
    <td><b>ios::in</b></td>
    <td>Open the file for reading.</td>
  </tr>
  <tr>
    <td><b>ios::out</b></td>
    <td>Create the file for writing.</td>
  </tr>
  <tr>
    <td><b>ios::ate</b></td>
    <td>Set the initial position at the end of the file.</td>
  </tr>
  <tr>
    <td><b>ios::app</b></td>
    <td>All writing occurs at the end of the file (“append”).</td>
  </tr>
  <tr>
    <td><b>ios::trunc</b></td>
    <td>Create a new file for read/write. If the file already exists, delete (“truncate”) its contents.</tr>
</table>
### C Style
In C, open the file with the fopen() function. After you're finished with the file, but before your program ends, you should also close the file stream with the fclose() function.
<table>
  <tr>
    <td><b>fopen()</b></td>
    <td>Open a file for reading and/or writing data.</td>
  </tr>
  <tr>
    <td><b>fclose()</b></td>
    <td>Close an open file stream.</td>
  </tr>
</table>
For example:

```c
#include <cstdio>
FILE *my_input;
my_input = fopen("data_file", "r");
 
// Do something with the file here
 
fclose(my_input);
```
The name of the file stream is “my_input”, while the physical file is “data_file”. The “r” argument (called the “mode”) to fopen() indicates that you will only be reading data from the file. (If you try to write to the file, you'll get an error.) Other common modes:
<table>
  <tr>
    <td><b>r</b></td>
    <td>Open the file for reading.</td>
  </tr>
  <tr>
    <td><b>w</b></td>
    <td>Create the file for writing.</td>
  </tr>
  <tr>
    <td><b>a</b></td>
    <td>Open an existing file for writing, starting at the end of the file ("append").</td>
  </tr>
  <tr>
    <td><b>r+</b></td>
    <td>Open an existing file for read/write.</td>
  </tr>
  <tr>
    <td><b>w+</b></td>
    <td>Create a new file for read/write.</td>
  </tr>
  <tr>
    <td><b>a+</b></td>
    <td>Open an existing file for read/write, starting at the end of the file.</td>
  </tr>
</table>
Note that the standard C-I/O header file must be included for proper declaration of functions like fopen() and fclose().
## Writing Data: << and fprintf()
### C++ Style
Writing data to a file stream in C++ is easy, as long as one isn't too picky about how the output looks. To write to a stream, one need only use the “«” operator, e.g.:
```c++
double a = 3.1415927;
double b = 1.414;
my_file.precision(5);
my_file << a << b << endl;
```
where the precision function sets the number of significant figures to be printed and the “endl” identifier prints an end-of-line and carriage return. The difficulty with C++ formatted output arises when one wants to control the number of decimal places, significant figures (precision), scientific notation, etc. on a compact, field-by-field basis. The syntax for this is significantly less concise than in C, and we will not discuss it further in this tutorial.
### C Style
Writing data to a file stream in C is accomplished with the fprintf() function. For example:
```c
fprintf(my_output, "The number of atoms is %d\n", natoms);
```
Here the file stream is identified by the variable my_output. The second argument to fprintf() is the “format string”, which includes control characters like “%d”. Variables containing data related to the control characters make up the remaining arguments. Note that the “printf()” function is the same as “fprintf(stdout,…).”

Some examples of commonly used control characters include:
<table>
  <tr>
    <td><b>%d</b></td>
    <td>Signed integer.</td>
  </tr>
  <tr>
    <td><b>%u</b></td>
    <td>Unsigned integer.</td>
  </tr>
  <tr>
    <td><b>%f</b></td>
    <td>Floating-point number.</td>
  </tr>
  <tr>
    <td><b>%e</b></td>
    <td>Scientific notation with lower-case "e".</td>
  </tr>
  <tr>
    <td><b>%E</b></td>
    <td>Scientific notation with upper-case "E".</td>
  </tr>
  <tr>
    <td><b>%s</b></td>
    <td>String.</td>
  </tr>
  <tr>
    <td><b>%c</b></td>
    <td>Character.</td>
  </tr>
</table>
You can control the number of digits printed by numerical values between the “%” and the control character. For example:
<table>
  <tr>
    <td><b>%3d</b></td>
    <td>Use three digits for integers, with spaces for leading zeroes.</td>
  </tr>
  <tr>
    <td><b>%03d</b></td>
    <td>Use three digits for integers, including leading zeroes, e.g. "007".</td>
  </tr>
  <tr>
    <td><b>%3d</b></td>
    <td>Print a 20-digit floating point number, with 12 digits to the right of the decimal point.</td>
  </tr>
</table>
## Reading Data: >> and fscanf()
### C++ Style
C++ provides a very simple means for reading data from a file, naturally using the “»” operator (the opposite of “«”):
```c++
int natom;
double energy;
 
my_input >> natom >> energy;
```
Similarly, one may read from the keyboard using the “cin” stream.
### C Style
There are numerous ways to read data from a file stream, but we'll focus here on the fscanf() function, which works very similarly to fprintf():
```c
int natom;
double energy;
 
fscanf(my_input, "%d %lf", &natom, &energy);
```
This function reads from the current position of the stream “my_input” an integer and a double, placing the results into the variables natom and energy, respectively. (Note that, in order for the values of the variables to be changed, the **addresses** of natom and energy must be given to fscanf(). More on this in later sections of this tutorial.)

The control characters are much like those used with fprintf(), though field widths are not specified. Some common control characters:
<table>
  <tr>
    <td><b>%d</b></td>
    <td>Signed integer.</td>
  </tr>
  <tr>
    <td><b>%f</b></td>
    <td>Single-precision floating-point number.</td>
  </tr>
  <tr>
    <td><b>%lf</b></td>
    <td>Double-precision floating-point number.</td>
  </tr>
  <tr>
    <td><b>%s</b></td>
    <td>String.</td>
  </tr>
  <tr>
    <td><b>%c</b></td>
    <td>Character.</td>
  </tr>
</table>
Note that the “scanf()” function is the same as “fscanf(stdin,…).”