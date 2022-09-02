Functions are essential in C++ because they can be used for repetitive tasks and to simplify code. For example,
```c++
#include <iostream>
using namespace std;
 
double power(double, int);
 
int main()
{
  int exponent=18;
  double base=5.3;
  double value;
 
  value = power(base,exponent);
  cout.precision(12);
  cout << value << endl;
 
  return 0;
}
 
double power(double a, int n)
{
  int i;
  double val=a;
 
  for(i=1; i < n; i++)
    val *= a;
 
  return val;
}
```
The function power() calculates the value of (base)^(exponent), and is defined explicitly in the code segment following main(). The function takes two arguments, a double and an int (in that order), and it returns a double, which main() prints to stdout. Some points to note:

* Above the code for main() we find the “declaration” of the power() function, including the types of its arguments and return value. This declaration is necessary so that the compiler can make sure that the use of power() remains consistent throughout the code. (Think of the compiler as reading the source code blindly from top to bottom.)
* Note that the names of the variables passed to power() by main() do not match (a and n vs. base and exponent). This is an example of [variable scope](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/Variable-Scope-and-Reference-Types).
* The cout.precision(12) directive tells the output stream to include 12 significant figures when printing the numerical value.

## Functions in Difference Source-Code Files
The example above includes the definition of the power() function in the same file as main(), but this is not actually necessary. We could instead put all the code down to the end of main() in one file (perhaps called “func.cc”) and the code for power() into another file (perhaps called “power.cc”). If we then try to [compile](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/What-is-a-%22Compilation%22%3F) func.cc, we get an error:
```shell
[arrakis:~] crawdad% c++ -c func.c
[arrakis:~] crawdad% c++ func.o -o func
  "power(double, int)", referenced from:
      _main in func.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```
This rather cryptic-looking output tells us that the compiler doesn't know what this thing called “power()” is and so the [link step](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/What-is-a-%22Compilation%22%3F) failed. Hence, we must first compile each source file and link both together in the end:
```shell
[arrakis:~] crawdad% c++ func.o power.o -o func
[arrakis:~] crawdad% c++ power.o func.o -o func
[arrakis:~] crawdad% func
1.08884397618e+13
```
## Make Utility
When you have more than a couple of source code files, the command-line compilation and link process can get very tedious. The “make” program can simplify one's like greatly. More on this topic is available here (NEED TO ADD LINK).

