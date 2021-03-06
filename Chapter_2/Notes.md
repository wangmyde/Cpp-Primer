# Chapter 2. Variables and Basic Types
### 1. Arithmetic Types
- Integral types: may be signed or unsigned. We obtain the corresponding `unsigned` type by adding unsigned to the type
> - character
>> char: a char is the same size as a single machine byte. char is **not** the same type as signed char
> - boolean types
- floating point types
### 2. Deciding which Type to Use
- the value can not be negative --> use an `unsigned` type
- `int` is not enough --> use `long long`
- only use `signed char` or `unsigned char`, but not `char`
- only use `double` for floating-point computations
### 3. Type Conversions
```
bool b = 42; // b is true
int i = b; // i has value 1
i = 3.14; // i has value 3
double pi = i; // pi has value 3.0
unsigned char c = -1; // assuming 8-bit chars, c has value 255
signed char c2 = 256; // assuming 8-bit chars, the value of c2 is undefined. 
                      //The program might appear to work, it might crash, or it might produce garbage values.
```
### 4. Don’t Mix Signed and Unsigned Types in Expressions
### 5. Literal
```
20 /* decimal */ 
024 /* octal */ 
0x14 /* hexadecimal */
```
- scientific notation
> the exponent is indicated by either `E` or `e`: `3.14159E0` `0e0`. By default, floating-point literals have type double.
```
'a' // character literal
"Hello World!" // string literal
```
- The type of a string literal is array of constant `char`s
- The compiler appends a null character (`’\0’`) to every string literal. Thus, the actual size of a string literal is one more than its apparent size.
- nonprintable
```
newline \n          horizontal tab \t    alert (bell) \a
vertical tab \v     backspace \b double  quote \"
backslash \\        question mark \?     single quote \'
carriage return \r  formfeed \f
```
- `ture` or `false`: are literals of type bool
### 6. Variable Definitions
```
int sum = 0, value, units_sold = 0;  
// value without an initializer, the variable is default initialized.
// Such variables are given the “default” value. What that default value is depends on
// the type of the variable and may also depend on where the variable is defined.
// Variables defined outside any function body are initialized to zero.
// variables of built-in type .defined inside a function are uninitialized.
// The value of an uninitialized variable of built-in type is undefined causing an error.
```
A simple variable definition consists of a **type specifier**, followed by a list of one or more **variable names** separated by **commas**, and ends with a **semicolon**.
- objective: an object is a region of memory that has a type.
```
double price = 109.99, discount = price * 0.16;  
//it is possible to initialize a variable to the value of one defined
//earlier in the same definition.
```
- Initialization and assignment are different operations in C++.
初始化不是赋值，初始化的含义是创建变量时赋予其一个初始值，而赋值的含义是把对象的当前值擦除，而以一个新值来替代。
```
// four different ways to define an int variable
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
```
- **initializing every object of built-in type**
### 7. Variable Declarations and Definitions
- separate compilation: Separate compilation lets us split our programs into several files, each of which can be compiled independently.
> - To support separate compilation, C++ distinguishes between declarations and definitions.
>> - declaration: A variable declaration specifies the type and name of a variable.
>> - definition: A variable definition is a declaration. In addition to specifying the name and type, a definition also allocates storage and may provide the variable with an initial value.
- 如果想声明一个变量而非定义它，就在变量名前添加关键字`extern`,而不要显式地初始化变量：
```
extern int i; // declares but does not define i
int j; // declares and defines j
```
- 若给`extern`关键字标记的变量赋一个初始值，便抵消了`extern`的作用，该语句就不再是声明，而是定义
```
extern double pi = 3.1416; // definition
```
- It is an **error** to provide an initializer on an `extern` inside a function.
- Variables must be defined exactly once but can be declared many times.
- To use the same variable in multiple files, we must **define** that variable in **one—and only one—file**. Other files that use that variable must **declare—but not define—that variable**.
### 8. Identifiers
> - Identifiers in C++ can be composed of letters, digits, and the underscore character.
> - Identifiers must begin with either a letter or an underscore. 
> - Identifiers are case-sensitive
> - 自定义的Identifiers不能连续出现两个下划线
> - 不能是以下划线开头加大写字母
> - 定义在函数体外的indentifiers不能以下划线卡头
>> - Variable names normally are lowercase
>> - classes we define usually begin with an uppercase letter, like `Sales_item`.
### 9. Scope of a Name
A scope is a part of the program in which a name has a particular meaning.
```
#include <iostream>
int main()  // defined outside a function—has global scope
{
    int sum = 0;  // The variable `sum` has block scope.
    for (int val = 1; val <= 10; ++val)  // The name val is defined in the scope of the for statement.
        sum += val; // equivalent to sum = sum + val
    std::cout << "Sum of 1 to 10 inclusive is "
              << sum << std::endl;
    return 0;
}
```
- Define Variables Where You First Use Them
### 10. References
A reference defines an alternative name for an object. We define a reference type by writing a declarator of the form &d, where d is the name being declared:
```
int ival = 1024;
int &refVal = ival; // refVal refers to (is another name for) ival. 
int &refVal2; // error: a reference must be initialized
refVal = 2; // assigns 2 to the object to which refVal refers, i.e., to ival
int ii = refVal; // same as ii = ival
// ok: refVal3 is bound to the object to which refVal is bound, i.e., to ival
int &refVal3 = refVal;
// initializes i from the value in the object to which refVal is bound
int i = refVal; // ok: initializes i to the same value as ival

```
- References must be **initialized**.
- A reference is **not** an object. Instead, a reference is just another name for an already existing object.
- After a reference has been defined, all operations on that reference are actually operations on the object to which the reference is bound
- 因为引用本身不是一个对象，所以不能定义引用的对象
```
int i = 1024, i2 = 2048; // i and i2 are both ints
int &r = i, r2 = i2; // r is a reference bound to i; r2 is an int
int i3 = 1024, &ri = i3; // i3 is an int; ri is a reference bound to i3
int &r3 = i3, &r4 = i2; // both r3 and r4 are references
```
- 允许在一条与剧中定于多个引用
- the type of a reference and the object to which the reference refers must match exactly.
```
int &rval1 = 1.01;  // WRONG. error: initializer must be an object
```
### 11. Pointers
A pointer holds the address of another object. We get the address of an object by usin the address-of operator(the `&`operator):
```
int ival = 42;
int *p = &ival; // p holds the address of ival; p is a pointer to ival
```
- Because references are not objects, they don’t have addresses. Hence, we may not define a pointer to a reference.
```
int first = 42, second = 43;
int *p = &first;
std::cout << "  &first: " << &first << std::endl;
std::cout << "  *p: " << *p << std::endl;
std::cout << "  p: " << p << std::endl;
p = &second;
std::cout << "  &second: " << &second << std::endl;
std::cout << "  *p: " << *p << std::endl;
std::cout << "  p: " << p << std::endl;
p = &first;
std::cout << "  &first: " << &first << std::endl;
std::cout << "  *p: " << *p << std::endl;
std::cout << "  p: " << p << std::endl;
int &j = *p;
std::cout << "  &j: " << &j << std::endl;
std::cout << "  *p: " << *p << std::endl;
std::cout << "  p: " << p << std::endl;
```
输出：
```
&first: 0076F980
*p: 42
p: 0076F980
&second: 0076F974
*p: 43
p: 0076F974
&first: 0076F980
*p: 42
p: 0076F980
&j: 00ECFEE0
*p: 42
p: 00ECFEE0
```
- `*`:作用就是把p里存的地址里的**值**取出来
- `p = &second`:由`&`把`second`这个变量的地址取出来，赋给`p`,所以`p`里存的是地址。但是`*p`是把p存的地址里对应的值取出来。
```
int *ip = i;  //但这样是illegal
```
- We may dereference only a valid pointer that points to an object.
- **null pointer**: A null pointer does not point to any object.以下两个方法都可行：
```
int *p1 = nullptr; // equivalent to int *p1 = 0;
int *p2 = 0; // directly initializes p2 from the literal constant 0
```
- 记住要**Initialize all Pointers!!!!!!!** 如果暂时没有地址用于初始化，就用`nullptr`或者`0`.
- If the pointer is 0, then the condition is false:
```
int ival = 1024;
int *pi = 0; // pi is a valid, null pointer
int *pi2 = &ival; // pi2 is a valid pointer that holds the address of ival
if (pi) // pi has value 0, so condition evaluates as false
// ...
if (pi2) // pi2 points to ival, so it is not 0; the condition evaluates as true
// ...
```
- Given two valid pointers of the same type, we can compare them using the equality (==) or inequality (!=) operators.
- `void*`: is a special pointer type that can hold the address of any object. But the type of the object at that address is unknown. We cannot use a `void*` to operate on the object it addresses.
```
int ival = 1024;
int *pi = &ival; // pi points to an int
int **ppi = &pi; // ppi points to a pointer to an int
```
```
&ival: 004FFB2C
*pi: 1024
&pi: 004FFB20
*ppi: 004FFB2C
**ppi: 1024
```

关于定义的格式：
We say that this definition might be misleading because it suggests that `int*` is the type of each variable declared in that statement.
```
int* p; // legal but might be misleading
int* p1, p2; // p1 is a pointer to int; p2 is an int
```
There are **two** common styles used to define multiple variables with pointer or reference type.
```
int *p1, *p2; // both p1 and p2 are pointers to int
```
This style emphasizes that the variable has the indicated compound type.
```
int* p1; // p1 is a pointer to int
int* p2; // p2 is a pointer to int
```
This style emphasizes that the declaration defines a compound type.

### 12. const Qualifier
A variable unchangeable by defining the variable’s type as `const`:
```
const int bufSize = 512; // input buffer size
bufSize = 512; // error: attempt to write to const object
```
- By Default, **`const` Objects Are Local to a File**
```
// file_1.cc defines and initializes a const that is accessible to other files
extern const int bufSize = fcn();
// file_1.h
extern const int bufSize; // same bufSize as defined in file_1.cc
```
- To share a `const` object among multiple files, you must define the variable as `extern`.
### 13. Type Aliases
A **type alias** is a name that is a synonym for another type.
- use a `typedef`
```
typedef double wages; // wages is a synonym for double
typedef wages base, *p; // base is a synonym for double, p for double*
```
- use a `using`
```
using SI = Sales_item; // SI is a synonym for Sales_item
```
```
wages hourly, weekly; // same as double hourly, weekly;
SI item; // same as Sales_item item
```
### 14. `auto` 
we can let the compiler figure out the type for us by using the `auto` type specifier. `auto` tells the compiler to deduce the typefrom the initializer. By implication, a variable that uses auto as its type specifier must have an initializer.
```
// the type of item is deduced from the type of the result of adding val1 and val2
auto item = val1 + val2; // item initialized to the result of val1 + val2
```
- we can define multiple variables using `auto`
```
auto i = 0, *p = &i; // ok: i is int and p is a pointer to int
auto sz = 0, pi = 3.14; // error: inconsistent types for sz and pi
```
### 15. The `decltype` Type Specifier

### 16. Defining Our Own Data Structures
    struct Sales_data {
        std::string bookNo;
        unsigned units_sold = 0;
        double revenue = 0.0;
    };  // **Notice the semicolon**
Our class begins with the keyword `struct`, followed by the `name` of the class and a (possibly empty) class body.

    struct Sales_data { /* ... */ };
    Sales_data accum, trans, *salesptr;

### 17. Writing Our Own Header Files
1. The most common technique for making it safe to include a header multiple times relies on the `preprocessor`. The preprocessor is a program that runs before the compiler and changes the source text of our programs. Our programs already rely on one preprocessor facility, `#include`. When the preprocessor sees a `#include`, it replaces the `#include` with the contents of the specified header.

2. C++ programs also use the preprocessor to define **header guards**. Header guards rely on **preprocessor variables**. Preprocessor variables have one of two possible states: defined or not defined. The `#define` directive takes a name and defines that name as a preprocessor variable. There are two other directives that test whether a given preprocessor variable has or has not been defined: `#ifdef` is true if the variable has been defined, and `#ifndef` is true if the variable has not been defined. If the test is true, then everything following the `#ifdef` or `#ifndef` is processed up to the matching `#endif`.  
```
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```
The first time Sales_data.h is included, the `#ifndef` test will succeed. The preprocessor will process the lines following `#ifndef` up to the `#endif`. As a result, the preprocessor variable SALES_DATA_H will be defined and the contents of Sales_data.h will be copied into our program. If we include Sales_data.h later on in the same file, the `#ifndef` directive will be false. The lines between it and the `#endif` directive will be ignored.
  
  
