This is a simple variation of the common "Hello World" program. It is one of the simplest, yet complete, C++ programs:

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

Open a new c++ source file called `welcome.cc` in your favorite editor and put the above code inside.
Then we must compile the source:

```
c++ -c welcome.cc
c++ welcome.o -o welcome
```

The first command will generate an object file, welcome.o, while the second will generate the actual executable program, `welcome`. You can execute the final program from the command line:

```
$ ./welcome
Welcome to the Crawford Group!
$
```

The lines that begin with `using …` in the source file allow you to use the functions `std::cout` and `std::endl` without typing the prefix `std::` every time. The signature `std::` indicates that these functions belong to the namespace called `std`. Namespaces allow you to have functions that may be called the same thing, without confusing which one is to be used. Declaring `using std::cout;` at the start of the program indicates to the complier that when we type `cout` we really mean `std::cout`. 
