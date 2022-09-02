An important feature of the C++ language is its facility for object-oriented programming (OOP), enabled through constructs called “classes”. Classes often allow the programmer to design code with a more natural structure that can be easier to maintain and extend. We'll briefly summarize the concepts and syntax here, and the later programming projects will help to solidify the ideas.

## A Simple Example: Molecule
What are the basic characteristics of a molecule? A molecule consists of a number of nuclei and electrons, and the difference between the sum of the atomic numbers of the nuclei and the number of electrons is the overall molecular charge. In the Born-Oppenheimer approximation, the massive nuclei have fixed relative positions in space, and these positions may or may not exhibit some amount of symmetry.

What do we typically do with these data? We can imagine rotating the molecule in space or translating it to a new position. Or we might imagine computing the principal moments of inertia of the molecule and subsequently its rotational constants. We might want to compute the distances between the molecule's substituent atoms, or the relevant bond angles or torsional angles.

A class is a special type that collects data (e.g. the molecule's characteristics) and operations/functions (e.g. actions we take on such data) together. For example, we could define a “Molecule” class using the above information as follows:
```c++
#include <string>
 
using namespace std;
 
class Molecule
{
  public:
    int natom;
    int charge;
    int *zvals;
    double **geom;
    string point_group;
 
    void print_geom();
    void rotate(double phi);
    void translate(double x, double y, double z);
    double bond(int atom1, int atom2);
    double angle(int atom1, int atom2, int atom3);
    double torsion(int atom1, int atom2, int atom3, int atom4);
 
    Molecule();
    ~Molecule();
};
```
The syntax gives a name of the class (“Molecule”), defines the relevant data/variables (natom, charge, etc.), and provides declarations (but not yet definitions) of various member functions associated with the class [rotate(), translate(), bond(), etc.]. The “public:” directive indicates that all of these variables and functions are available to any code making use of an object of type Molecule. If some data needs to be kept accessible only by member functions, then they would be placed under a “private:” directive.

The last two functions “Molecule()” and “~Molecule()” are called the “constructor” and “destructor”, respectively. The former is automatically called whenever an object of type “Molecule” is declared (“instantiated”, to use the language of C++ afficionados), while the latter is automatically called whenever the object goes out of [scope](https://github.com/CrawfordGroup/ProgrammingProjects/wiki/Variable-Scope-and-Reference-Types).

If we place the above code into a file called “molecule.h” then we can create another file called, for example, “molecule.cc”, in which we define the relevant functions:
```c++
#include "molecule.h"
#include <cstdio>
 
void Molecule::print_geom()
{
  for(int i=0; i < natom; i++)
    printf("%d %8.5f %8.5f %8.5f\n", zvals[i], geom[i][0], geom[i][1], geom[i][2]);
}
 
void Molecule::translate(double x, double y, double z)
{
  for(int i=0; i < natom; i++) {
     geom[i][0] += x;
     geom[i][1] += y;
     geom[i][2] += z;
  }
}
 
Molecule::Molecule(){ }
Molecule::~Molecule(){ }
```
Here we've explicitly defined the “print_geom()” and “translate()” member functions using the name of the class as a prefix (“Molecule::”) to keep them separate from any other “print_geom()” or “translate()” functions that might exist in some other part of the program (i.e., these functions can only be used with Molecule objects). In addition, we've defined constructor and destructor functions that don't do anything (for now).

How do we use this new class? Here's an example in which we prepare a water molecule, print its coordinates, translate it along the x-axis, and print the coordinates again:
```c++
#include "molecule.h"
 
using namespace std;
 
int main(int argc, char *argv[])
{
  Molecule h2o;
 
  h2o.natom = 3;
  h2o.charge = 0;
  h2o.zvals = new int[h2o.natom];
  h2o.geom = new double* [h2o.natom];
  for(int i=0; i < h2o.natom; i++)
    h2o.geom[i] = new double[3];
 
  h2o.zvals[0] = 8;
  h2o.geom[0][0] =  0.000000000000;
  h2o.geom[0][1] =  0.000000000000;
  h2o.geom[0][2] = -0.122368916506;
  h2o.zvals[1] = 1;
  h2o.geom[1][0] =  0.000000000000;
  h2o.geom[1][1] =  1.414995841403;
  h2o.geom[1][2] =  0.971041753535;
  h2o.zvals[2] = 1;
  h2o.geom[2][0] =  0.000000000000;
  h2o.geom[2][1] = -1.414995841403;
  h2o.geom[2][2] =  0.971041753535;
 
  h2o.print_geom();
  h2o.translate(5, 0, 0);
  h2o.print_geom();
 
  delete[] h2o.zvals;
  for(int i=0; i < h2o.natom; i++)
    delete[] h2o.geom[i];
  delete[] h2o.geom;
 
  return 0;
}
```
First, we declare an object named “h2o” that is of type “Molecule” and directly set the number of atoms, the molecular charge, the atomic numbers, and the Cartesian coordinates (in bohr) using the name of the object and a period to identify the member data or function (e.g. “h2o.natom”). If we place this code into a separate file named “water.cc” in the same directory as “molecule.h” and “molecule.cc”, we can compile this code using the following commands:
```
c++ -c molecule.cc
c++ -c water.cc
c++ water.o molecule.o -o water
```
The output from the program is:
```
8  0.00000  0.00000 -0.12237
1  0.00000  1.41500  0.97104
1  0.00000 -1.41500  0.97104
8  5.00000  0.00000 -0.12237
1  5.00000  1.41500  0.97104
1  5.00000 -1.41500  0.97104
```
You can extend the class by adding your own definitions of the member functions rotate(), bond(), etc., which may be very useful for [Project #1](https://github.com/CrawfordGroup/ProgrammingProjects/tree/master/Project%2301).

## Constructor and Destructor Functions
In the above molecule.cc example, we entered blank placeholders for the constructor [“Molecule()”] and destructor [“~Molecule()”]. Let's make them more useful. First, let's have the constructor take arguments as the number of atoms and the molecular charge:
```c++
Molecule::Molecule(int n, int q)
{
  natom = n;
  charge = q;
  zvals = new int[natom];
  geom = new double* [natom];
  for(int i=0; i < natom; i++)
    geom[i] = new double[3];
}
```
Next, let's have the destructor delete[] the memory we allocated in the constructor automatically (Remember that the destructor member function is called when the object goes out of scope).
```c++
Molecule::~Molecule()
{
  delete[] zvals;
  for(int i=0; i < natom; i++)
    delete[] geom[i];
  delete[] geom;
}
```
Since we changed the constructor to take two arguments, we also need to modify its declaration in molecule.h:
```c++
#include <string>
 
class Molecule
{
  public:
    int natom;
    int charge;
    int *zvals;
    double **geom;
    string point_group;
 
    void print_geom();
    void rotate(double phi);
    void translate(double x, double y, double z);
    double bond(int atom1, int atom2);
    double angle(int atom1, int atom2, int atom3);
    double torsion(int atom1, int atom2, int atom3, int atom4);
 
    Molecule(int n, int q);
    ~Molecule();
};
```
Now our water.cc code becomes a bit simpler because the constructor and destructor do the work of allocating and deleting the memory for us:
```c++
#include "molecule.h"
 
using namespace std;
 
int main(int argc, char *argv[])
{
  Molecule h2o(3,0);
 
  h2o.zvals[0] = 8;
  h2o.geom[0][0] =  0.000000000000;
  h2o.geom[0][1] =  0.000000000000;
  h2o.geom[0][2] = -0.122368916506;
  h2o.zvals[1] = 1;
  h2o.geom[1][0] =  0.000000000000;
  h2o.geom[1][1] =  1.414995841403;
  h2o.geom[1][2] =  0.971041753535;
  h2o.zvals[2] = 1;
  h2o.geom[2][0] =  0.000000000000;
  h2o.geom[2][1] = -1.414995841403;
  h2o.geom[2][2] =  0.971041753535;
 
  h2o.print_geom();
  h2o.translate(5, 0, 0);
  h2o.print_geom();
 
  return 0;
}
```
However, this is still very inconvenient, because the programmer is required to include the Z-values and Cartesian coordinates of each atom explicitly in the program. What if we instead define the constructor such that the values could be read from a file containing the necessary data, such as:
```
3
8 0.000000000000  0.000000000000 -0.122368916506
1 0.000000000000  1.414995841403  0.971041753535
1 0.000000000000 -1.414995841403  0.971041753535
```
The first integer is the number of atoms, and each subsequent line contains the atomic number, and coordinates (in bohr) each each atomic center. A molecule constructor that can read this file for us could be written as:
```c++
#include <iostream>
#include <fstream>
#include <iomanip>
#include <cstdio>
#include <cassert>

Molecule::Molecule(const char *filename, int q)
{
  charge = q;

  // open filename
  std::ifstream is(filename);
  assert(is.good());

  // read the number of atoms from filename
  is >> natom;

  // allocate space
  zvals = new int[natom];
  geom = new double* [natom];
  for(int i=0; i < natom; i++)
    geom[i] = new double[3];

  for(unsigned int i=0; i < natom; i++)
    is >> zvals[i] >> geom[i][0] >> geom[i][1] >> geom[i][2];

  is.close();
}
```
Don't forget to change the declaration of the constructor in “molecule.h” to use the new arguments:
```c++
Molecule(const char *filename, int q);
```

This code simplifies our main program considerably:
```c++
#include "molecule.h"

using namespace std;

int main(int argc, char *argv[])
{
  Molecule h2o("geom.dat", 0);

  h2o.print_geom();
  h2o.translate(5, 0, 0);
  h2o.print_geom();

  return 0;
}
```
If we place the Z-values and coordinates in the file “geom.dat”, the above code will automatically build the molecule object making use of these data, including dynamic allocation of the requisite memory. Furthermore, when the object goes out of scope, the memory is automatically returned to the system.