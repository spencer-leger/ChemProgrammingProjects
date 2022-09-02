There are several data types that are commonly used in C and C++ programs

| Name | Data | Typical Size (bytes) |
| :----- | :----- | :----- |
| int | Integer| 4 |
| char | Character| 1 |
| float | Floating-point number | 4|
| double | Double-precision Floating-point number | 8 |
| void | No Type (useful for defining functions with no return) | N/A |

Note: The size of data types can vary among computer systems, so you should never assume particular values. The sizes given in this table are only guidelines. Use the `sizeof()` function to determine the size of a type or variable inside your programs.

A variable is a location in memory used to store data. Variables are assigned names and data types by the programer either when they are first used or before hand. Here are some examples: 

```C++
int iter = 0;       // An integer with an initial value of zero 
double energy;      // A double-precision floating-point number with no initial value
int z_vals[50];     // An array of 50 integers
double geom[10][3]; // A 2-d array of doubles, there are 10 sets of 3 values each
```

Note: You should never assume that an uninitialized variable has been zero'd out.