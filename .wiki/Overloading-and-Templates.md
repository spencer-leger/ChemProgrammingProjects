An essential feature of an object-oriented language like C++ is the ability to “overload” functions – that is, to use the same function name to perform different tasks depending on the arguments provided. For example, we could define a new version of the constructor for our molecule class that reads the Z-values and Cartesian coordinates of the atoms from a file, rather than forcing the programming to provide them in the code itself:
```c++
#include <iostream>
#include <fstream>
#include <iomanip>
#include <cstdio>
#include <cassert>

Molecule::Molecule(const char *filename, int n, int q)
{
  natom = n;
  charge = q;

  // allocate space
  zvals = new int[natom];
  geom = new double* [natom];
  for(int i=0; i < natom; i++)
    geom[i] = new double[3];

  std::ifstream is(filename);
  assert(is.good());

  for(unsigned int i=0; i < natom; i++)
    is >> zvals[i] >> geom[i][0] >> geom[i][1] >> geom[i][2];

  is.close();
}
```