# Reference Semantics
There is an alternative semantic to pointer semantics named <mark style="background: #BBFABBA6;">reference semantic</mark> in C++. This is a language layer tool. Compiler generates same assembly code as like as we use pointer semantic most of the time. Small differences can be depending on optimization level.

Reason to add reference semantic to the language is that pointer semantic not compatible with other language features like <mark style="background: #BBFABBA6;">operator overloading</mark>.

References was has been existed before modern C++. This was only called "reference". When <mark style="background: #BBFABBA6;">move semantics</mark> and <mark style="background: #BBFABBA6;">perfect forwarding</mark> tools were move in with modern C++, the "reference" concept is expanded and divided into two category, Old "reference" is called "<mark style="background: #BBFABBA6;">L value reference</mark>" and  "<mark style="background: #BBFABBA6;">R value reference</mark>" is added.

Why R value references added to the language? Modern C++ (C++11 and later) added various tool to the language. Some of these tools are very effected the language. These are move semantics, perfect forwarding, lambda expressions etc. R value references added to support these features.

> In conclusion there are two reference concept in C++:
> 1. L value reference
> 2. R value reference

Now L value references are mentioned. We will look to R value reference later.

<mark style="background: #FFB8EBA6;">Additional Information</mark>: <mark style="background: #FFB86CA6;">godbolt.org</mark> is a web site that shows assembly code of given code. (Compiler explorer)

## Value category
<mark style="background: #ADCCFFA6;">Value category</mark>: Qualification of an expression. That means <mark style="background: #D2B3FFA6;">int x;</mark> not has a value category. Value category qualify an expression like: <mark style="background: #D2B3FFA6;">x, 10, x+5, a[3], *ptr</mark>. There is no concept like value category of an declared variable. Date type and value category are different concepts. <mark style="background: #FFF3A3A6;">Value category describes which contexts is an expression can used as legal and which situation is that expression be subjected to certain conversion operations</mark>. When we describe an expression's value category, we can determine the other tools of the language how reacts that expression and is there a syntax error or how to make inferences like similar situations.

Value categorization is differs between C and C++ and this makes it more complex. In C, an expression can be a L value expression or can be an R value expression. L value means that the expression corresponds an object and has a area on memory and we can access that area. We can test that with address of operator. If we can use address operator on the expression than it is a L value reference, and in the other way it is R value reference.

But in C++, this not that simple. Value category of an C expression may not be same as value category of C++. In modern C++, there are three value categories, and there are called <mark style="background: #BBFABBA6;">primary value categories</mark>.

> Primary value categories are:
> 1. <mark style="background: #BBFABBA6;">L value</mark>
> 2. <mark style="background: #BBFABBA6;">PR value</mark>
> 3. <mark style="background: #BBFABBA6;">X value</mark>

<mark style="background: #FFF3A3A6;">An expression can be belong to only one of the primary category in same time.</mark>. 
For exmaple:
```cpp
x // L value
x + 10 // PR value

int&& foo();

foo() // X value
```

- L value: It was has been came from "left value". But after, it has been evaluated to "locators value".
- PR value: pure R value.
- X value: expiring value

There are <mark style="background: #BBFABBA6;">composite value categories</mark> too. There are:
- <mark style="background: #BBFABBA6;">GL value</mark>: L value and X value combination set is GL value. Generalized L value.
- <mark style="background: #BBFABBA6;">R value</mark>: PR value and X value combination set is R value. 
![[Pasted image 20231029002611.png]]

## L value references
An reference is a name that replaces an object. L value reference:
```cpp
int x = 20;
int &r = x; // L value reverence

++r;
```
That means r is a reference to x. r means x. That means there are no two abject names x and r. There only one object that is x and r means x. & is not an operator here, & is a <mark style="background: #BBFABBA6;">declarator</mark>. For example:
```cpp
 r = 90; // we used x
 &r // we take the reference
 
///

int x = 10;
int& r =x; // Here, & is a declerator

int* p = &r; // Here, * is a declerator but & is a operator (address of) 
```

We bind r to x.
And also that corresponds following syntax:
```cpp
int x = 20;
int* const ptr = &x;

++(*ptr);
```

That make easier to writing and ensures the compliance with C++ tools like operator overloading.

L value references can't point to another object after initial value. That means L value references corresponds to top level pointers and means address that <mark style="background: #FFF3A3A6;"> L value references pointed can't be changed</mark>.

<mark style="background: #FFF3A3A6;">All initialize methods can be used with  L value reference syntax</mark>. For example:
```cpp
#include ‹iostream>

int main()
{
	int x = 10;
	int& r1 = x;
	int& r2｛ x ｝；
	int& r3(x);
}
```

<mark style="background: #FFB8EBA6;">Additional information</mark>: There is another name of the heap in C++, that is <mark style="background: #BBFABBA6;">free store</mark>.

Let's look at some legal and illegal (syntax error) situations:
1. <mark style="background: #FFF3A3A6;">References must be initialized</mark>. That means references cannot be default initialized. For example:
```cpp
int main()  
{  
	int& X; // Syntax error, a reference must be initialied
}
```

2. <mark style="background: #FFF3A3A6;">We must initialize with same type of the object</mark>. For example:
```cpp
int main()
{
	unsigned int x = 10;
	int& r = x; // Syntac error, not same type
}
```

3. <mark style="background: #FFF3A3A6;">We cannot initialize L value references with R value references</mark>. For examaple:
```cpp
int main()  
{  
	unsigned int x = 10;  
	int& r = 10;  // Syntax error, we cannot initialize with a R value
}
```

4. Following syntaxes are legal to use:
```cpp
int main()
{
	int al]{ 1, 2, 3, 4 };
	int* p= a;
	int& r1 = a[2];
	int& r2 = *p;
}
```

5. And also followings is legal:
```cpp
int main()
{
	int x = 10;
	int* p{ &x };
	int* &r = p;

	++ *r; // increment of x
｝
```

<mark style="background: #FFF3A3A6;">There is no reference to reference as similar to pointer to pointer</mark>. For example:
```cpp
int main()
{
	// Classical pointer to ponter syntax
	int x = 10;
	int* p = &x;
	int** ptr = &p;

	// Followings syntax doesn't corresponds to pointer to pointer
	int x = 10;
	int& r1 = x;
	int& r2 = r1; // r2 is same as r1

}
```

<mark style="background: #FFF3A3A6;">Question: Can we take a reference to an array?</mark> The answer is yes, but first let's look how would we take a pointer to an array. Pointer to an array is different from pointer to an array member. For example:
```cpp
int main()
{
	// Pointer to an array
	int ar[] = {2, 5, 6, 9 };
	int(*p)[4] = &ar;

	// Reference to an array
	int(&r)[4] = ar;
	r[2] = 34; // OK
	int *ptr = r; // OK
｝
```
But upper there is less complex syntax that corresponds upper syntax. We can benefit of type deduction and we can write it like following:
```cpp
int main()
{
	int ar[] = {2, 5, 6, 9 };
	//int (&r)[4] = ar;
	
	auto& r = ar; // auto type deduction
｝
```

## Function that has reference argument

 ```cpp
 #include <iostream>

void func(int& r)
{
	r = 999;
}

int main()
{
	int ival{ 3 };
	
	sta:: cout << "ival= " « ival « "\n";
	
	func(ival);
	std:: cout << "ival = " « ival « "\n";
｝
```

<mark style="background: #FFF3A3A6;">Keep caution</mark> on that; we cannot determine that the func can be changed the arguments or not without showing func's definition in C++. In C, we can say it if the function is call-by-value or call-by-reference by seeings the function call. But in C++, because of the reference semantic we cannot say that and we must see the function implementation to say if the function can change the passed argument or not.

<mark style="background: #FFF3A3A6;">Important: Function call expression that the function return type is a L value reference is a L value expression in C++ language.</mark>  That means the function call expression to <mark style="background: #D2B3FFA6;">int& foo();</mark> is a L value expression but, the function call expression to <mark style="background: #D2B3FFA6;">int foo();</mark> is a PR value expression. For example the function call <mark style="background: #D2B3FFA6;">foo();</mark> is a L value expression in below code, because the function returns an L value reference:
```cpp
#include <iostream>

int g = 10;

int& foo()
{
	return g; // returns global g
}

int main()
{
	int x = foo(); // Here, x not meas g, because x isn't a reference
	int& r = foo(); // Here, r means g, because r is a reference

	++r; // global g is increased

	foo() = 999; // Assign 999 to g
}
```

Since <mark style="background: #D2B3FFA6;">int& foo()</mark> is a L value expression ve can assign it to a L value reference.
And we can use function <mark style="background: #D2B3FFA6;">foo():</mark> call at a left side of the assignment operator: <mark style="background: #D2B3FFA6;">foo() = 999;</mark>. 

<mark style="background: #FFB8EBA6;">Additional Information</mark>: <mark style="background: #BBFABBA6;">Array decay</mark>.
```c
int main()  
{
	int a[] = { 1,2,3 }; //
	int (*p)[3] = &a;
	
	(*p)[2] = 99;
	
	int* ptr = *p; //array decay
}
```

# Const L value reference
If we want to use an L reference to only access, we use const L value reference. we use const keyword to do that.
<mark style="background: #BBFABBA6;">Const correctness</mark> is determines the code quality and it is very important.
```cpp
#include <iostream>

// accessor
void foo(const T&);
//mutator
void bar(T &r);

int main()
{
	int x = 10;
	const int& r = x;
	
	int ival = r;
	ival = r + 3;
	
	r = 9; // Syntax error
}
```

<mark style="background: #FFB8EBA6;">Additional Information</mark>: <mark style="background: #ADCCFFA6;">Strict aliasing rule</mark>:
```c
int main()
{
	double x = 10.5;
	int* ptr = (int*)&x; //strict aliasing rules
}
```

<mark style="background: #FFF3A3A6;">Normally reference type conversion isn't legal in C++. But if the reference type is a const type, type conversion becomes legal</mark>. For example:
```cpp
int main()
{
	int x = 10;
	
	double& r = x; // This is not legal
	const double& r = x; // This is legal
}
```
But how r replaces an another type in upper example? The answer is it doesn't, <mark style="background: #FFF3A3A6;">r replaces a double objects that generated by compiler</mark>. The compiler generates an temporary object with same type of the reference, than assign the value to it and links our reference to that temporary object. And the lifespan of this temporary objects last in scope of the reference's scope. This can be done because it is possible to implicit (automatic) type conversion from int to double. If it is not possible, than that doesn't work and that means syntax error.

The objects that not existed in source code but compiler generated is named <mark style="background: #BBFABBA6;">temporary object</mark>. This is a important topic of the C++. The compiler generated code is like following:
```cpp
int main()
{
	int x = 10;
	
	// double& r = x; // This is not legal
	const double& r = x; // This is legal
	/******  Following syntax represents the compiler generated code *******/
	{
		const double temp = x;
		const double& r = temp;
	}
	/******  End of representation *******/
}
```

<mark style="background: #FFF3A3A6;">what would be if we assign an R value to a const L value reference? This wouldn't be a syntax error</mark>. Because like the upper description the compiler creates a temporary object for that R value and assign out cont L value reference to it. For example:
```cpp
int main()
{
	const int& r = 5;
	/******  Following syntax represents the compiler generated code *******/
	{
		const int temp = 5;
		const int& r = temp;
	}
	/******  End of representation *******/
}
```

What that mean in practically? This means we can call the function <mark style="background: #D2B3FFA6;">void foo(T &);</mark> with only L value category expression arguments. <mark style="background: #FFF3A3A6;">Bu we can call the function <mark style="background: #D2B3FFA6;">void foo(const T &);</mark> with the L value category expression arguments and also R value category expression arguments.</mark> 
## Pointer semantics vs reference semantics
Most of the time pointer semantics and reference semantics are alternative each other. But sometimes we cannot use one of them instead of other.

> 1. There is pointer to pointer but there is no reference to reference.
> 2. pointer array is possible but reference array is not possible. But this need solved with <mark style="background: #BBFABBA6;">std::reference_wrapper</mark> class of C++ standard library. We will loot it later.
> 3. There is nullptr, but there is no null reference. This means if we use nullptr with pointers there is no corresponding here to references.
> 4. We can assign another address to a pointer, if the pointer is not top level pointer. But we cannot change a reference. That means references is not <mark style="background: #BBFABBA6;">rebindable</mark>.
> 5. A reference is corresponds the top level pointer. Because the references must be initialized and we cannot change where it points later on.
> 6. Pointers can be default initialized but references not.

Bullet 1: Let's look pointer to pointer example:
```cpp
void pswap(int** ptrl, int** ptr2)
{
	int* ptemp = *ptr1;
	
	*ptr1 = *ptr2;
	*ptr2 = ptemp;
}

int main
{
	int x = 10, y = 34;
	int* p1 = &x, * p2 = &y;
	
	pswap (&p1, &p2);
}
```

And let look the reference semantic of same code:
```cpp
void pswap(int* &r1, int* &r2)
{
	int* ptemp = r1;
	
	r1 = r2;
	r2 = ptemp;
}

int main
{
	int x = 10, y = 34;
	int* p1 = &x, * p2 = &y;
	
	pswap (p1, p2);
}
```

Bullet 2: Below code is for pointer array example:
```cpp
int main()
{
	int* p[10];
	int* ptr;

	// there is no alternative of upper code with references
	int x, y, z;
	int &r[3] // Syntax error
}
```

Bullet 3: Where nullptr or NULL is used?
1. address returning function like <mark style="background: #D2B3FFA6;">FILE* fopen():</mark>. returns NULL in case of failure. Or malloc() is same. This kind return a address of the object in case of success, other way it return NULL. Also same system functions or 3rd party codes uses this approach.
2. NULL is used frequently in search functions. They return the address of the objects if it founds and NULL if it is not found.
3. NULL function argument can be used to give an option to the caller. For example a function takes an pointer argument and if the value of the pointer is NULL, the function makes different operation. For example <mark style="background: #D2B3FFA6;">fflush()</mark>, if we give it a file object address, the function flashes the file's buffer; but if we give it NULL, the function flashes the all open files buffer. 

In upper cases or similar cases that NULL is used, we cannot use references. In this cases we use pointers. Or we can use alternative tools of C++ such as <mark style="background: #BBFABBA6;">std::optional</mark>.

<mark style="background: #FFF3A3A6;">We can declare function reference like function pointers</mark>. For example:
```cpp
int foo(int);

int main()  
{  
	int (*fp)(int) = foo;  
	int (&fpr)(int) = foo;
}
```

Now we will briefly mention about R value references. We will mention it later in detail with move semantics and perfect forwarding. R value references came with modern C++.
<mark style="background: #FFF3A3A6;">We can initialize L value references with L value expressions.
We can initialize R value references with R value expressions.
We can initialize const  L value references with both L value expressions and R value expressions.</mark>
R value references can be created with <mark style="background: #FFF3A3A6;">&& operator</mark>.
```cpp
int&& r = 10;
```
<mark style="background: #FFF3A3A6;">L value references must be initialized and must be initialized with an R value expression. If we try to initialize it with an L value expression, it would be a syntax error.</mark>

Which expression is L value and which expression is R value:
1. If an expression is exist of an variable name than it is a L value (only nameb).
2. Literal expressions is PR value.
3. If an expression consist of arithmetic operator, than it is a PR value.
4. Some expression that created with same operators are L value. Front ++ operator is one of them. For example ++x is a L value. But X++ is a PR value. It is same with -- operator.
5. Expression that created with comma if right expression of the coma is L value that the expression is a L value
6. Expression created with assignment operator is an L value
7. If an function return type is not a reference that a call to that function is an PR value
8. If an function return type is a L value reference that a call to that function is an L value
9. If an function return type is a R value reference that a call to that function is an X value
```cpp
int x{}; // x is a L value

int foo();
int& func();
int&& bar();

int&& r = 10;

int main 
{
	int x = 10;
	int y = 10;
	int a[120];
	int&& r = 10;
	
	pvcat (x); // L value because it is a name
	pvcat (10); // PR value because it is a literal
	pvcat(x + 4); // PR value because it consist of an arithmetic operator
	pvcat (a); // L value because it is a name
	pvcat (++x); // L value because of pre increment operator
	pvcat (x++); // PR value because of post incemetn operator 
	pvcat (--x); // L value because of pre decrement operator
	pvcat（X--); // PR value because of pre decremetn operator
	pvcat ((x, y)); // L value because right handside of coma is a L value
	pvcat(x = 5); // L value because of assignment operator
	pvcat (x += 5); // L value because of assignment operator
	pvcat (foo()); // PR value because function doesn't return a reference
	pvcat (func()); // L value because function returns an L value referenca
	pvcat (bar()); // X value because function returns an R value reference
	pvcat(r); // L value because r is a name
	pvcat (foo); // L value because foo is a function name
	pvcat (nullptr); // PR value because nullptr is a literal
}
```

Some exercises:
```cpp
int main()
{
	int x = 10;
	
	int &r1 = ++Х; // OK. ++x is a L value
	int &r2 = x++; // syntax ERROR. x++ is not a L value (PR value)
	int &&r2 = --x; // syntax ERROR. --x is not a R value (L value)

	const int &r2 = x++; // OK. we can assing a PR value to const L value reference
}
```

Value category comparison with C:
1. ++x is R value expression in C
2. x, y (comma operator) is R value expression in C
3. x > 10 ? a : b is R value expression in C. (L value expression in C++)

<mark style="background: #FFB8EBA6;">Additional Information</mark>: pointer or reference function parameters can be take default value.

## Auto type deduction
Type deduction is a compile time tool. Now we look only auto type deduction. With auto keyword we must initialize the variable. We can use following initialize methods:
```cpp
int main()
{
	auto x = 12;
	auto y{ 12 };
	auto z(12);
｝
```

There are differences between these initialization methods, but we will mention it later.
There are some rules to deducting a type. This rules are complicated. We have three separated rule set and we must learn them separately,

Following syntaxes have different type deduction rules:
```cpp
auto x = expr;
auto &x = expr;
auto &&x = expr; // This is not a R value referance. This is forwarding reference
```

### auto x = expr;
If auto keyword is used with && like <mark style="background: #D2B3FFA6;">auto &&x = expr;</mark> this means <mark style="background: #BBFABBA6;">forwarding reference</mark> not he R value reference. Or it named <mark style="background: #BBFABBA6;">universal reference</mark>. We look the when we look forwarding reference.
For now, we will be focusing first two one.

<mark style="background: #FFF3A3A6;">What rules are implemented to determine which type is replaced the auto keyword?</mark>
Here some examples:
```cpp
 int main()
{
	char c = 'A'; 
	auto x1 = 12; // The type is int
	auto x2 = 3.4; // The type is double
	auto x3 = 3.4L; // The type is long double
	auto ×4 = 'A'; // The type is char
	auto x5 = nullptr; // The type is nullptr_t
	auto x6 = 10 > 5; // The type is bool
}
```

```cpp
int main()
{
	char c ='A';
	
	auto x1 = c;
	auto x2 = +c; // Type of x2 is integer, beceuse there are integral promotion rule 
}
```

<mark style="background: #ADCCFFA6;">Integral promotion rule</mark>: If an operand has an operator, under integer types are promoted to integer.

```cpp
 int main()
{
	int x = 10;
	double d = 3.4;
	auto x = × > 5? 3 : d; // type of x is double, type conversion rules is valid here
}
```

```cpp
int main()
{
	int x = 10;
	int& r = x;
	
	auto a = r; // Type of a is int, references are ignored in this situation
}
```

```cpp
int main()
{
	const int x = 10;
	auto a = x; // type of a is int, cosnt is ignored
}
```
const and reference are ignored when auto type deduction.
```cpp
int main()
{
	int x = 10;
	const int& cr = x;
	
	auto a = cr; // Type of a is int, const and reference are ignored
}
```

```cpp
int main()
{
	int a[] = {1, 2, 3 };
	auto b = a; // type of b is int* , array decay

//---------

	const int a[] = {1, 2, 3 };
	auto b = a; // type of b is const int* , array decay
}
```

```cpp
 int main()  
{
auto x = "mesut" ; // type of x is const char *
}
```

<mark style="background: #FFB8EBA6;">Important Note: Additional info: </mark>There is no type difference between a variable type and const type of the same variable. That means <mark style="background: #D2B3FFA6;">int</mark> and <mark style="background: #D2B3FFA6;">const int</mark> is same type. But const makes difference in pointers; <mark style="background: #D2B3FFA6;">int*</mark> and <mark style="background: #D2B3FFA6;">const int*</mark> is not same type. For example:
```cpp
// Following is not a function redecleration. They are the decleration of different functions
void func(int* p);
void func(const int*);

// Following code is a function redecleration. They are redecleration of same functions
void func(int* p);
void func(int* const p);
```

```cpp
int foo(int);

int main()
{
	auto x = foo; // type of x is int (*)(int), it is a decay
	auto x = &foo; // type of x is int (*)(int)
}
```

```cpp
int main()  
{  
	int a[10][20];  
	auto x = a; //type of x int (*)[20], array decay
}
```

### auto & = expr;

#### Const difference

Const difference between auto and auto&:
```cpp
int main()
{
	const int ival{};
	auto x = ival;

//--------

	int ival{};
	auto &x = ival;

//--------

	const int ival{};
	auto &x = ival; // const int &x = ival;
}
```

In upper example,
<mark style="background: #FFF3A3A6;">At first example, const was being ignored. Type of x is int.</mark>
<mark style="background: #FFF3A3A6;">At second example, type of auto and type of x are differ. type of auto is int and type of x is int&. </mark> 
<mark style="background: #FFF3A3A6;">At third example, type of auto is const int and type of x is const int&. </mark>
<mark style="background: #FFF3A3A6;">So, with auto& const is not ignored.</mark>

#### Array decay difference
array decay difference between auto and auto&

```cpp
int main()
{
	int a[10]{};
	auto &x = a; // int (&x)[10]
	// int (&x)[10] = a;
｝
```

In upper example, the deducted type that corresponds to the auto is int[10].  x is reference to that type

```cpp
int main()
{
	auto& r = "hello"; // const char[6] is corresponds the auto
	// cosnt char (&r)[6] = "hello";
}
```
So with auto& there is no array decay, but with auto there is.

#### pointer reference difference
pointer reference difference between auto and auto&
```cpp
void foo(int);

int main)
{
	//auto f1 = foo;
	void (*f1)(int) = foo; // The deducted type for auto is:  void (*)(int)
	
	//auto &f2 = foo;
	void (&f2)(int) = foo; // The decucted tyep for auto is: void (int)
}
```

Additional Information: We can use auto keyword with other keywords. For example:
```cpp
int main()
{
	int x = 120;
	
	const auto y =X;
}
```

```cpp
int main()
{
	int x = 10;
	auto p1 = &x; // auto = int*
	auto *p1 = &x; // auto = int, p1 = int*

}
```

```cpp
int main()
{
	int x = 10;
	const auto p2 = &x; // p2 = int * const , top level
}
```

```cpp
int main()
{
	auto &x = 10; // syntax error, not related to deduction, related to R value: int &x = 10; (R value expression to L value syntax error)
}
```