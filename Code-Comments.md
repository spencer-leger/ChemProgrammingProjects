Comments are text inserted into the code that programmer can read, but the complier will skip. In C, comments are surrounded by `/* */` pair. In c++ a comment begin with `// ` and includes everything from that point until the end of the line.

Some examples:

```c++
/* This is a c-style comment */
// this is a c++-style comment

/* This is a 
  C-syle comment that spans 
  several lines */

// This is a C++ style comment 
// that spans several lines.

int a=0; //comments can be on the same line as code. 

/* C-style comments can even be before code */ int b = 0;

// C++ comments can't before the code since they go from the comment indicator
// until the end of the line.
// This int won't be in the code int c= 0;
int c = 0; // but this will! 
``` 

Note: It is extremely important to document your code with appropriate comments, particularly when dealing with complicated scientific code, whose purpose can be difficult to glean by looking at the source.

