[[Course Files]]
C++ is a multi-paradigm programming language. The pardigms that C++ support are:
1. Procedural paradigm
2. Object oriented paradigm
3. Generic programme paradigm
4. Genre independent programming paradigm

Legacy C++ codes can be compiled with setting up compiler switches.

First C++ standards are C++98 and C++03. Actually these standards are same. A guarantee has been given on C++98 standard to not the change the language features until existing problems are discovered and reported in order to fix them.

The new rule has been established that new C++ standards are published apart over three years. And 11, 14, 17, 20 standards are published.

C++11 and later standards are mentioned Modern C++ informally.

# C++ C core and C Differences
C++ has core that based on C. But this C core not exactly same as C language.

Main reason to this differences is compile time checks are more restricted in C++.

In programming languages there are two basic type concepts. These are:
1. static typing
2. dynamic typing

Compiler detect data's type depends on source code and can make checks at compile time. For example if we try to assign a real number on a integer type, compiler can detect that and warn us. This is static typing concept. Dynamic typing concept is to detect datas type on run time. For example x = "string", than x = 12.

C++, C#, and Java like programming languages are actually kind of static typing concept. But also they have support to dynamic typing concepts.

Though C has static typing concept. Dynamic typing concept can be ensured by writing code.

Although both C and C++ has static typing concept, this not mean their concepts are identical. C designed as a small and generic language. For this reason the type checking responsibility of C on compile time is not exact as C++. C has weak type control. C++ provides strict type controls.

For example type conversion form void* to any other type is allowed and okay. Bu in C++ this kind of conversion is a syntax error. In C++ the type conversion operator is a must.

```cpp
int *p = malloc(n); // malloc return type is void*
```

Because type of string literals of string in C is char*, following syntax is legal in C:
```c
char *p = "Hello";
```

But in C++, this literal type is const char*. In the other mean, type conversion of an const address to non-const address is syntax error in C++.

This kind of incompatible situations are increased at every new C++ standard.

For example auto keyword is in both C and C++. But meaning of it are differs between them. The register keyword is also has different meanings between C and C++. The register keyword is reserved for different meaning in C++.

## Three important term to know in both C and C++
There are three important term to know both in C and C++. These are:
1. Undefined behavior
2. Unspecified behavior
3. Implementation defined behavior

Small part of undefined behavior that in C is a syntax error in C++. And there are some situations that undefined behavior in C but defined in C++.

## Differences of functions

### Implicit int rule
C was had implicit int rule until 99. After that standard this rule has been removed. But some compiler has been keep the support for this rule in order to keep support to legacy C code. 
Implicit int rule means the language assumes the type of data as int in some cased when we not mention the int explicitly. For example function returns:
```c
func(void) // There is no return type. The compiler assumes that as int
{

}
```

Don't use a implicit int in C. This is only for legacy code. In the other hand this syntax is an error in C++.

### Void function parameter
In C, there is a difference between empty function argument list and void function argument.
```c
int func();
int foo(void);
```
If we call the function func() with an argument, the compiler doesn't throw an error. Bu if we call function foo() with an argument the compiler does it.

However, calling both function with an argument is a syntax error in C++.

Note: "..." is not an operator. This is a token named eclipses. This token is used in various area in C and C++. But usage of it is much more in C++. Variadic function in C: void f(int, ...);

### C old style function definition
This rule is not used in C anymore though, it is supported from compilers to give support to legacy C code. For example:
```c
int foo(a, b, c)
double b, c;
{
	///
	return a;
}
```
In this legacy syntax, the function argument types not mentioned in function parentheses but given after that before curly braces. Not given argument types are assumes as int by default.

Old style function definition not supported in C++.

### Implicit function declaration
In C, the compiler look for identifiers (making name lookup) and throws an warning if it not found a declaration of it. 
But in C++, the compiler throws an error instead of a warning. This is a name look error.
For example:
```c
int main()
{
	func(1, 2);
}
```
The C compiler looks for the function name and assumes it as a function name that defined in an external module if the compiler not find the identifier of function. And also assumes its return value type as int.
This implicit function declaration is also for supporting legacy code.

## Type and type conversion related differences

1. The conversation between arithmetic types and address types. This is legal in C but in-legal in C++. For example:
```c
int main()
{
	int x = 10;
	int* p = x;
	//
	int* p2 = &x;
	int y;
	y = p2;
}
```
2. There is not type conversation between different kind of address types in C++. This is a syntax error. But this is legal in C. For example:
```c
int main()
{
	int x = 34;
	int* p = &x;
	char* ptr = p;
}
```

3. There is automatic type conversion from void* to other address types in C. This is completely legal in C. But this is syntax error in C++. For example:
```c
int main()
{
	int x = 10;
	void* p = &x;
	int* iptr = p;
}
```
4. Character literals type in C is int type. But character literal type in C++ is char type. This is important to figure out undefined behaviors. For example C++ has feature named function overload.
```cpp
int main()
{
	printf("%zu\n", sizeof('A'));
}
```
This is important to figure out undefined behaviors. For example, C++ has feature named function overload.  Which type of argument the overloaded function is using determined which function is called. For example:
```cpp
void func(int);
void func(char);

int main()
{
	func(10);
	func('A');
}
```
5. String literals are actually and array. The string literals are const in C++, however not const in C. This not mens changing string literals is true in C. This is undefined behavior in C but error in C++. For example:
```cpp
"hello" char[6] in C
"hello" const char[6] in C 
```

```cpp
int main()
{
	// Following is legal in C but syntax error in C++
	char * p = "hello";
	// Following is undefined behavior in C but error in C++
	*p = 'A';
}
```

## Const keyword differences
1. Initializing const objects is a must in C++. Keep caution that top-level const pointer must be initialized in C++, other way it will be syntax error.
```c
// constant pointer to int
// Top-level pointer
// right-const
int* const ptr; // ! Syntax ERROR in C++. Not initialized const

// pointer to const int
// Low-level ponter
// left-pointer
const int* ptr; // ! Not syntax erro on C++
```

2. An array size must be cont in C and C++.  In C++ language, If a const variable initialized with a const literal (constant expression), the compiler let us to use this variable in areas that needs const literal such as array size and case of a switch and bit field. This is not allowed in C. For example:
```cpp
int main()
{
	const int size = 10;
	int a[size] = { 0 };
}
```

3. In C++, global const objects has internal linkage. But in C, global const objects has external linkage. In other expression global const objects in C++ has a static keyword in front of them.  For example:
```cpp
const int y = 20;
// Means below in global scope:
static const int y = 20;
```
Note: if we want a external linkage const in C++, we must define it with extern keyword: <mark style="background: #D2B3FFA6;">extern const int x = 10;</mark>

4. In C, we have automatic type conversion from const T* to T*. But this is syntax error in C++. For example:
```cpp
int main()
{
	const int x = 10;
	int * p = &x; // Valid in C but syntax error in C++
}
```

Because of this the following is a syntax error in C++.
```cpp
int main()
{
	char* p = "Hello"; // const char* to char* is syntax error in C++
}
```

## Type conversion differences
1. C has \_Bool keyword that added with C99 standard. This is a unsigned integer type and we can assign an integer value to that type. stdbool lib provides macros inside it to write bool  flag = true; but this is actually same as \_Bool. That leads the logic operators and the comparison operators in C is also results integer value, not exact bool value. But in C++, bool is real. There are keyword bool, true and false. Therefore in C++, we shouldn't use int instead of bool.
2. In C++ an another type has a automatic type conversion to bool. The rule is that if the value zero than the bool value if false and other ways are always true.
3. In C++ bool type has automatic type conversion to other types. true bool value is converted to integer 1 or real 1. false bool value is converted to 0.
4. In C++, there are automatic type conversion from pointer types to bool type. Except nullptr, all pointers are converted to bool as true.
5. In C++, that is guarantee to bool variables are 1 byte. But if we want to bool value as 1 bit, C++ offers different tools to do it.
6. In C++, increment operator (++) can't be used with bool object. This was has been allowed older standards of C++, but in modern C++ this is not allowed.

## Pointer differences
In C++, we have other options that alternative to pointer. One of them is reference semantics. But of course we can use pointer semantics in C++ too. Some alternatives to pointer in C++ is followings:
- reference semantics
- smart pointers
- std: reference_wrapper

## Array differences
In C++, classical (C style) arrays is not widely used. Can be used at low level codes but in general, std::vector, std::array are preferred.

## Null pointer differences
In C, NULL is a macro not a keyword. And that means (void*)0. Also assigning 0 as a integer literal to a pointer means same thing. But in C++, this is differs. nullptr is a keyword that added to Modern C++. Don't use NULL macro instead of nullptr in C++, if you are not dealing with legacy code. Not use 0 either.

nullptr is a keyword in C++. This means we are not needed to include a header file to use it. nullptr is a literal that type nullptr_t. nullptr_t not an address type but nullptr is assigning to addresses. But it can transform to a pointer type. The advantage of nullptr, it can't be transform into not pointer types. For example:
```cpp

int* ptr = nullptr; // OK
int ptr2 = nullptr; // ! NOT OK. Syntax ERROR.
```
nullptr also important to function overloading concept. But we will mention in later.

## User defined type differences
 1. In C, default enumerator underlying type is integer. But in C++, enumerator underlying type determined during compile time.
 ```c
enum Color {Blue, Red, Black};
// underlying type
int main()
{
	sizeof(enum Color) == sizeof(int); // True in C
}
```

2. In C we define an enum object with enum keyword: enum Color mycolor = Red; . But in C++ there is no need to enum keyword (using it is optional): Color mycolor = Red;. 
int underlying type enum codes written in C is can not be compatible with C++.
3. In C, there is automatic type conversion between enums and arithmetic types. But in C++, there is no implicit (automatic) type conversion from other types to enum types. And also there is no type conversion between enum types.
```cpp
enum Color {Blue, Red, Black};
int main(void)
{
	Color c = Blue;
	c = 3; // This is NOT allowed in C++
}
```
4. But in C++, there is implicit (automatic) type conversion from classical enums to arithmetic types (Reverse not allowed) only with classical enum types. One of the reason of the existing of enum class is this. This conversion not allowed with enum classes.
```cpp
enum Color {Blue, Red, Black};
int main(void)
{
	Color c = Blue;
	int x = c; // This is allowed in C++
}
```
5. We can define an enums underlying type in C++. This must be integer type and can't be real number type. The included with modern C++. We can chose underlying type for classical enum types and scoped enum (enum class) types.
```cpp
enum Color : unsigned char {Blue, Red, Black};
```
6. In C++, there is an another approach to enums that is scoped enum. Both C and C++ classical enum is in scope that it is defined. For that reason we can't use same enum member name in two different enums in same scope. For overcome of this problem scoped enums (enum class) are added in C++. For example:
```cpp
enum class Color {Blue, Red, Black};
enum class TrafficLight {Yellow, Red, Green};
// it is possible to define red in two enum class

int main()
{
	Color c = Color::Blue; // Scope resolution operator must bu used
	TrafficLight tl = TrafficLight::Red;
}
```

## Another difference
```c
# include <stdio.h>

int main()
{
	char str[5] = "Hello"; // Alloved in C, but sytax ERROR in C++

	puts(str); // Undefined bevior in C
}
```