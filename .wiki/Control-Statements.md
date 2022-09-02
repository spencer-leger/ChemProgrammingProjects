An executable program consists of a series of statements/instructions that the computer executes in order. That is each line of code is executed from top to bottom sequentially. Control statements define when the default flow of the program should be disrupted. Either by repeating, or skipping instructions: 

<table>
  <tr>
    <td><b>if-else</b></td>
    <td>Select different instructions based on a given set of conditions.</td>
  </tr>
</table>

```C++
if(iter >= maxiter){
    cout<< " Number of iterations exceeded\n";
    exit(2);
}
else {
    cout << "iter = " << iter << endl;
}
```

<table>
  <tr>
    <td><b>switch</b></td>
    <td>A multi-choice if statement.</td>
  </tr>
</table>

```c++
switch(val){
   case 0:
     cout << " Zilch\n";
   case 1:
     cout << " Uno\n";
   case 2: 
     cout << " Dos\n";
   case 3:
     cout << " Tres\n;
   default:
     cout << "A bunch";
     break;
}
```

<table>
  <tr>
    <td><b>while loop</b></td>
    <td>Repeat a series of instructions while the given conditions hold true.</td>
  </tr>
</table>

```c++
int iter = 0;
int maxiter = 10;
while(iter < maxiter){
  x_coord += 2.0;
  iter++; 
}
```

Note the values of `iter` and `maxiter` must be initialized outside the loop. Also note that if `iter` is not incremented inside the loop body, this would result in a infinite loop because the condition would always be true!

<table>
  <tr>
    <td><b>do-while loop</b></td>
    <td>Similar to while, but the instructions are always evaluated at least one time, since the condition is not checked until the end.</td>
  </tr>
</table>

```c++
do {
   x_coord += 2.0;
   iter++;
} while(iter < maxiter);
```

<table>
  <tr>
    <td><b>for loop</b></td>
    <td>One of the most common and powerful control statements. Defines an initial state, a condition, and increment. Loop continues incrementing until the condition is no longer true.</td>
  </tr>
</table>

Standard for loop:
```c++
for(int i = 0; i < max; i++)
{
    cout<< " iter = "<< i << endl;
}
```

The for loop also has several C++ only variants involving what are called `iterators` and are very powerful when used in combination with the  Standard template library (stl) containers. 

For loop using `std::vector<double>::iterator`
```c++
#include <vector>
std::vector<double> vec = {1.0,2.0,3.0,4.0};
cout << " vec = { ";
for(std::vector<double>::iterator it = vec.begin(); it != vec.end(); ++it) {
  cout << " " << *it <<" "; \\ it is a pointer to an element of the vector
                            \\ we use the * operator to get the value pointed to by "it"
}
cout << " }\n"; 

```
Range based for loop. Here is a shorthand for the example above but we will capture the value rather 
than a pointer by declaring the loop variable as a reference. 
```c++
#include <vector>
std::vector<double> vec = {1.0,2.0,3.0,4.0};
cout << " vec = { ";
for(double& val: vec} {
  cout << " " << val << " ";
}
cout << " }\n";
```

<table>
  <tr>
    <td><b>break</b></td>
    <td>Exit a loop or switch based on a given condition (usually placed after an if statement for some condition).</td>
  </tr>
</table>

```c++
for(int i = 0; i < max; i++) { 
   energy += 2.0; 
   cout << " iter = " << i << endl;
   if(energy > 10.0) break;
}

```
This for loop will exit when `energy` gets larger than 10.0, even if `i` is less than `max`!