A variable can be “local” (available only to the function that declared it) or “global” (available to all or some subset of functions in a program).

## Local Variables
Here's a simple example:
```c++
#include <iostream>
 
using namespace std;
 
void junk(int);
 
int main()
{
  int i=1;
 
  cout << "in main, i= " << i << endl;
  junk(i);
  cout << "in main, i= " << i << endl;
 
  return 0;
}
 
void junk(int i)
{
  i=2;
  cout << "in junk, i= " << i << endl;
}
```
If we compile this code and run it, we obtain the following output:
```
in main, i=1
in junk, i=2
in main, i=1
```
Notice that the variable “i” in main() is completely independent of the variable “i” in junk(). Even though its name appears to be the same, the variable i is declared separately in each function and is therefore **local** to each function.

How can we change the value of a variable in a function? By providing the function with the address of the variable (i.e. a pointer to it):
```c++
#include <iostream>
 
using namespace std;
 
void junk(int *);
 
int main()
{
  int i=1;
 
  cout << "in main, i= " << i << endl;
  junk(&i);
  cout << "in main, i= " << i << endl;
 
  return 0;
}
 
void junk(int *i)
{
  *i=2;
  cout << "in junk, i= " << *i << endl;
}
```
This code gives a slightly different output:
```
in main, i=1
in junk, i=2
in main, i=2
```
What did we modify?
1. The argument of junk() changed from int to pointer-to-int.The argument of junk() changed from int to pointer-to-int.
2. Inside junk(), since i is now an address, we must dereference it (“*i”) to refer to the value at that address.
3. The call to junk() in main() changed to use the reference to its local variable i (“&i”).

## Global Variables
If you want a variable to be available to several functions simultaneously, you must declare the variable *globally*. For example, we could make the variable “i” above global by moving its declaration outside of both main() and junk():
```c++
#include <iostream>
 
using namespace std;
 
int i;
void junk(void);
 
int main()
{
  i=1;
 
  cout << "in main, i= " << i << endl;
  junk();
  cout << "in main, i= " << i << endl;
 
  return 0;
}
 
void junk(void)
{
  i=2;
  cout << "in junk, i= " << i << endl;
}
```
This gives exactly the same output as before, even though “i” is handled very differently:
```
in main, i=1
in junk, i=2
in main, i=2
```

## Reference Types
C++ also provides reference types, which are related to pointers, but a bit safer to use (and thus less flexible). Reference types are declared using the “&” syntax, as we can see by the following example:
```c++
#include <iostream>
 
using namespace std;
 
void junk(int &);
 
int main()
{
  int i=1;
 
  cout << "in main, i= " << i << endl;
  junk(i);
  cout << "in main, i= " << i << endl;
 
  return 0;
}
 
void junk(int &i)
{
  i=2;
  cout << "in junk, i= " << i << endl;
}
```
Here “i” is of type int, but is implicitly cast to type “reference to int” when passed into junk(). Modifying “i” inside junk() now changes its value from within the scope of main() as well.

Reference types differ from pointers in that, once a reference type has been initialized, it cannot be re-assigned (“reseated”). This is helpful to compilers that cannot make such assumptions with pointers, which can be reseated at will (and often dangerously!). This also implies that reference types cannot be NULL (again unlike pointers), and so must be initialized as soon as they are defined.