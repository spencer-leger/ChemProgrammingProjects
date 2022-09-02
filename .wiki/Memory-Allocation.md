The computer's physical memory is used to store the data contained in variables, arrays, etc. In quantum chemistry programs, where the size of the computed data can range from megabytes to terabytes (and petabytes are approaching rapidly), efficient use of memory is vital. The purpose of this section is to explain the fundamentals of C++-language memory allocation/deallocation.

## Pointers
Each piece of data used by a program (integers, doubles, characters, etc.) is associated with a memory location/address, often referred to by a hexadecimal number (e.g., 0x8130). A pointer is a data type that can be used to store such addresses. For example, imagine we have an integer variable named “natom” whose address we need [e.g., to pass to a function]. We can obtain the address using the “address of” operator (also called the “reference” operator):
```c++
int natom
...
great_function(&natom);
```
Note carefully the difference between natom and &natom: the former refers to the *value* of the variable natom; the latter refers to the *memory address* of the value of the variable natom. (An explanation of the need to pass a pointer in this case can be found in the section on [scope](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/Variable-Scope-and-Reference-Types).)

If we needed to *store* this address for later use, we could declare a variable of type “pointer-to-int”:
```c++
int *natom_address;
 
natom_address = &natom;
```
Notice the use of the “*” syntax to identify the pointer variable. This syntax can be used with any data type: int, double, char, etc. (even “void”).

What if we know the pointer (address), and we'd like to know the value stored there? We can access that using the “indirection” (dereference) operator:
```c++
cout << "Number of atoms: " << *natom_address << endl;
```
(If you instead tried to print natom_address instead of *natom_address, above, you would most likely get a strange-looking integer value, i.e. the hexadecimal representation of the actual memory address.)

The notion of a pointer also means that we can indirectly change the value of natom using its address. For example, the following code will print the number 10, not 7:
```c++
natom = 7;
natom_address = &natom;
*natom_address = 10;
cout << "Number of atoms: " << natom << endl;
```
## Arrays
One way to declare an array in C++ is using the standard bracket notation, e.g.
```c++
int zvals[10];  /* declare an array of 10 integers */
...
cout << "The z-value of the first atom is: " << zvals[0] << endl;
```
Notice that in C++, array indices begin at element “0”. However, the variable named “zvals” is actually a pointer-to-int, meaning we could also use the syntax:
```c++
cout << "The z-value of the first atom is: " << *zvals << endl;
```
How do we access the other elements of the array using this notation? We can take advantage of so-called “pointer arithmetic”:
```c++
cout << "The z-value of the second atom is: " << *(zvals+1) << endl;  // Same as zvals[1]
```
The notation “zvals+1” refers to the memory address of the next element in the array, which we dereference with the “*” operator (This notation works because the integers in the array occupy consecutive memory locations). Hence, we could print all the elements of the array using the bracket notation:
```c++
for(i=0; i < 10; i++)
  cout << "Z-value of atom " << i << " is " << zvals[i] << endl;
```
or, equivalently,
```c++
for(i=0; i < 10; i++)
  cout << "Z-value of atom " << i << " is " << *(zvals+i) << endl;
```

## new[] and delete[]
There are two ways to allocate memory for things like arrays: statically or dynamically. In fact, there are two types of memory available to your program: the “stack” and the “heap”. The former is used for all data, instructions, etc. allocated at compile-time (static); the latter is for memory allocated at run time (dynamic). 

Static allocation means that the memory is allocated when the program is compiled (aptly named "compile-time"), as in cases when one uses syntax like:
```c++
int zvals[10];
```
Dynamic means that the memory is allocated when the program is executed (“run-time”). The advantage of dynamic allocation is that the program itself can determine how much memory it needs as it runs (e.g. based on input data).

In C++, a common approach to allocate memory dynamically is using the new[] operator:
```c++
int *zvals = new int[10];
...
delete[] zvals;
```
Alternatively, one can use the malloc() function:
```c++
int *zvals = (int *) malloc(10 * sizeof(int));
...
free(zvals);
```
In these examples, we explicitly declare the variable zvals to be a pointer-to-int, but until we assign it a value, it's meaningless. The new[] operator takes the data type (in this case, “int”) and the number of elements needed and returns a pointer. The malloc() function takes as an argument the number of bytes needed and returns a pointer to the allocated memory. The sizeof() function returns the number of bytes associated with a given data type (See the section on [data types](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/Data-Types-and-Variables) for more information). (Note: Technically, the malloc() function takes as an argument a value of type “size_t”, which can generally be thought of as a “byte”). The “(int *)” syntax to the left of malloc() is called a “cast”; it converts the value returned by malloc() (called a “pointer-to-void” or “void *”) into a pointer-to-int, to match the declared type of zvals. Note also the use of stdlib.h, which provides the necessary declaration of malloc().

If the call to new[] or malloc() is successful, we may then access the elements of zvals using the usual syntax: zvals[0], zvals[1], etc.

After we are finished with the memory, we must release it back to the computer. If we allocated it using new[], then we return it using the delete[] operator:
```c++
delete[] zvals;
```
If we used malloc(), then we return it with the free() function:
```c++
free(zvals);
```
It's important to delete[]/free() memory allocated with new[]/malloc(), not only because it's good coding practice, but also because it can often reveal subtle memory access errors elsewhere in your program.

## Matrices
As with arrays, C++ provides syntax to declare matrices and higher-dimensional structures:
```c++
int geom[10][3];
```
With the above syntax, the matrix “geom” would be allocated at compile-time (statically) using 30 consecutive integers in memory stored “row-wise” or in “row-major” ordering (i.e., the first three elements in memory would constitute row 0, the next three row 1, etc. Note that the Fortran programming language stores matrices “column-wise”, such that consecutive memory locations follow along the matrix columns). We access the elements of the matrix using the usual bracket notation, e.g. “geom[6][2]” would yield the value on the 7th row and 3rd column of the matrix.

An equivalent way to achieve the above using dynamic memory allocation is to declare geom as a “pointer-to-pointer-to-int”:
```c++
int **geom = new int* [10];
for(int i=0; i < 10; i++)
  geom[i] = new int[3];
```
This is complicated the first few times you go through it, so be patient:
* The variable geom is a pointer-to-pointer-to-int, but the value to which it points is also a pointer, e.g. geom is of type “int **”, but geom[0] is of type “int *”.
* The first call to new[] allocates space for an array of pointers-to-int, each of which will contain the address of the first element of each row of the matrix.
* The calls to new[] inside the loop independently allocate each row of the matrix as an array of integers.
* We can access the elements of the matrix with exactly the same syntax as that for the statically allocated matrix, e.g. “geom[6][2]”.
Note that the static allocation of a matrix above yields a set of *consecutive* addresses in memory (“contiguous” memory allocation), while the dynamic method above only guarantees that each row of the matrix is contiguous. A method for dynamically allocating a matrix with contiguous storage will be described elsewhere.

To release the memory after we are finished with it, we could use:
```c++
for(int i=0; i < 10; i++)
  delete[] geom[i];
delete[] geom;
```
Notice that we have one delete[] call for every new[] call above and that the release of the rows occurs before the release of the pointers to the rows.