Here is our simple program from the initial example:
```c++
#include <iostream>

using std::cout;
using std::endl;

int main(int argc, char *argv[])
{
    cout << "Welcome to the Crawford Group!" << endl;

    return 0;
}
```
Building a program from a source-code file like the above works in two stages: 
  - "compilation" of object files (\*.o) from the raw source code files (\*.cc): `c++ -c welcome.cc`
  - "linking" the object codes into an final executable: `c++ welcome.o -o welcome`

An object file is a translation of the source code into machine code, i.e. instructions recognizable by the computer's CPU.  However, an object file alone is not generally executable because it doesn't usually define all the necessary steps for a complete program.  The purpose of the linking step is to provide all the missing pieces to generate the final product.

Consider the simple program above as an illustration.  The code is intended only to print a few words to the screen, which is identified by the "file stream" named "cout".  However, the source code above does not *define* what `cout` actually is.  Thus, the object file, welcome.o, produced by the compilation step, includes an undefined reference to cout (called a *symbol*), and the linking step supplies the definition. (This definition is actually found in a library file named `libstdc++` located elsewhere in the computer system.)

The program above consists of two parts:
  - The preamble, which refers to the *header file*, iostream.h (but called "iostream" in the code).  This header includes a *declaration* of objects like cout that tell the compiler about the object's fundamental characteristics. If you don't include a declaration of every function or object you use in a program, the compiler should complain.  (Try it!  Remove the `#include <iostream>` line and re-compile.)
  - The `main()` function is the starting point for all programs.  Every C++language program must contain one - and only one - main().