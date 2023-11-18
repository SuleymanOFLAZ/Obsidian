# constexpr
constexpr is added to the language after modern C++. Constexpr says to compiler and reader the the object created an expression that can be used where literal expression is needed.

A const expression must be initialized with a literal. constexpr comprehends the const but also <mark style="background: #FFF3A3A6;">can be used in places where constant expression is needed.</mark>
   
Let's look at cont first. We can use a const in places where a constant expression needed if only the cont is initialized with a const expression. For example:
```cpp
const int x = y; // Not used in places that needs constant expression
const int c = 10; // Can be used as constant expression
```

We can write following but, there is no need for this because constexpr comprehends the const:
```cpp
constxpr const int x = 10; // Actually there is no need for const here
constxpr int x = 10;
```
The type of x is const int in upper two lines. And there is no difference between them.
But there is difference between two expression on below code:
```cpp
const int* p = &g; // This is low level const means p is not const itself
constexpr const int* q = &g; // Here q is const too
q = &x; // Syntax Error

constexpr int* q = &g; // q is const but what it ponits to is not cosnst
q = &x; // OK
```

Constexpr array:
```cpp
constexpr int [a] = { 1, 3, 5, 7}; // all of members are const
// Type of a is const int[4]
```

We can use constexpr with auto:
```cpp
constexpr auto b = 10 > 5; // OK

const int y = 40;
constexpr int z = 909;
constexpr auto x = y + z; // OK
```

## Constexpr functions
constexpr function
```cpp
constexpr int func(int x, int y)
{

}
```

<mark style="background: #FFF3A3A6;">There are conditions to ensure a constexpr function.</mark> The compiler checks these conditions. We inspire these rules later but let's look some of them. 
1. static local variable can't be used inside the function
2. parameters, return type and all variables that defined inside function must be <mark style="background: #BBFABBA6;">literal type</mark>.
3. constexpr functions must be inside header file. Because compiler must know the function definition to calculate at compile-time.

<mark style="background: #FFF3A3A6;">If all the arguments that passed to the constexpr function is constexpr variables, the function is calculated in compile time</mark>. 

```cpp
constexpr int func(int x, int y)
{
	return x * x + y * y;
}

int main()
{
	constexpr int a = 7;
	const int b = 5;
	
	int a[func(10, 20)] {}; // OK
	constexpr auto val = func(a * a, b + 13); // OK
}
```

<mark style="background: #FFF3A3A6;">If we don't pass a constexpr argument to the constexpr function that it is calculated at run time.</mark>
```cpp
constexpr int func(int x, int y)
{
	return x * x + y * y;
}

int main()
{
	int a = 7;
	int b = 5;

	func(a, b); // This is calculated at run time
	int a[func(10, 20)] {}; // This is syntax ERROR because the function not called with constxpr
	constexpr int c = func(a, a + 21); // syntax ERROR because function return is not constexpr, the error not related to the function call
	int e = func(a, a + 21); // OK
}
```

<mark style="background: #FFF3A3A6;">Constexpr functions are implicitly inline functions</mark>. That means following the syntax is same:
```cpp
constexpr int foo(int x)
{
	return x * x;
}
//-----
inline constexpr int foo(int x)
{
	return x * x;
}
```


```cpp
constexpr bool isprime(int x)
{
	if (× == 0 || x == 1）
		return false;

	if (× % 2 == 0)
		return x == 2;
	
	if (× % 3 == 0)
		return x == 3;
	
	if (× % 5 == 0)
		return x == 5;
		
	for (int i = 7; i * i <= x; i += 2)
		if (× % i == 0)
			return false;

	return true;
} 
```

<mark style="background: #FFB8EBA6;">Additional Information</mark>: Let's look a code piece:
```cpp
int main()
{
	for (int i = 0; i < 10; ++i)
		int i = 99; // syntac error in C++ but not an error in C
}
```
This is a syntax error in C++ but not a syntax error in C. We define two same named variable in same scope and that caused syntax error. But why this isn't a syntax error in C? Because in C there is a seamless curly braces at start of the for and end of the for. That is because it was designed like that. So the corresponded code would be look like following in C:
```c
// Corresponded C code
int main()
{
	for (int i = 0; i < 10; ++i)
	{
	{
		int i = 99;
	}
	}
}
```

# Function overloading
We can give same name to various functions that in same scope with using function overloading. <mark style="background: #BBFABBA6;">Function overload resolution </mark>states how compiler choose convenient function between overloaded functions. This increased the size of the compiler and for that reason C doesn't support that tool in order to keep small the compiler size.
<mark style="background: #FFF3A3A6;">If we encounter first time with a C++ tool, we must ask that question first is it has a effect in runtime of the program. This question is also frequently asked in interviews.</mark> So, <mark style="background: #FFF3A3A6;">is there a cost of function overloading at run time of program? The answer is no.</mark> Because this is completely compile time operation. 

There are two question that is confused each other in interview questions:
1. Is there function overload in the example?
2. Which function will be called in case of function overloading? (Related to function overload resolution)

<mark style="background: #FFF3A3A6;">There are three needed condition for function overloading</mark>:
1. Functions must have same name.
2. Functions must be inside the same scope. Different scopes hides names each other. That named <mark style="background: #BBFABBA6;">name hiding</mark> or <mark style="background: #BBFABBA6;">name masking</mark> or <mark style="background: #BBFABBA6;">name shadowing</mark>.
3. Signature of overloaded functions must be different.

<mark style="background: #ADCCFFA6;">Function signature</mark>: <mark style="background: #BBFABBA6;">Function signature</mark> means that parametric structure of the function except return type. For example signature if the function <mark style="background: #D2B3FFA6;">int foo(int, int);</mark> is that the function has two int argument.

<mark style="background: #FFF3A3A6;">If signatures of two functions are same but return types are different, this is a syntax error.</mark> The compiler evaluates that situation there is function redeclaration (because the signatures of the functions are same) and return type of the redeclared function is different. That causes the syntax error. But if the returns values are same too, this evaluated like redeclaration of the function by the compiler without syntax error.

<mark style="background: #FFF3A3A6;">Changing the type name (but just type name, the type is not changed) is not a function overloading. This is function redeclaration. </mark>For example:
```cpp
typedef int Word;

void foo(int);
void foo(Word); // This is redecleration, not a overloading Because the Word is int too
```

Let's look some is it function overloading or not question that asked in interviews:
```cpp
void func (char);
void func(unsigned char);
void func (signed (char);
```

How many function overload are there in upper code? The answer is three. char, unsigned char and signed char types are distinct from each other in C++.

```cpp
void func (int x, int y = 5);
void func(int x);
```
Is there a function overloading in upper code? Yes, there is. Default argument is not effect the function signature.

```cpp
void func(int);
void func (const int);
```
Is there a function overloading in upper code? No. This is a function redeclaration. Top level const doesn't change the function signature.

```cpp
void func(int* p);
void func(const int* p);
```
Is there a function overloading in upper code? Yes, there is. This overloading is used frequently.

```cpp
void func(int* p);
void func(const int* p); 
void func(int* const p);
```
How many function overload are there in upper code? The answer is two. Because first two is redeclaration. In 2nd declaration the const is for object itself and that not change the function signature.

```cpp
void func(int&);
void func (const int&);
```
Is there a function overloading in upper code? Yes, there is. This is used frequently to. Actually this is same as that we done with pointers.

```cpp
#include <cstdint>

void f(int32_t);
void f(int);
```
Is there a function overloading in upper code? It depends on the situation. uint_32 is a typedef declaration and a homonym. It can be differ depending in compiler. So that can be a function overload or can't be too depending on the compiler.

```cpp
void f(int32_t);
void f(int64_t);
```
Is there a function overloading in upper code? Yes, there is.

```cpp
void func(...); // variadic function
void func(int);
```

Is there a function overloading in upper code? Yes, there is. The function signatures is different

```cpp
void func(int&);
void func(int&&);
```
Is there a function overloading in upper code? Yes, there is. L value reference and R value reference makes the function signature different.

```cpp
void func(bool);
void func(int);
```
Is there a function overloading in upper code? Yes, there is. Bool and int are different types in C++.
# Function overload resolution
Existence of function overloading is not related to function overload resolution. Function overload resolution is about which overloaded function will be called after a overloaded function call.
<mark style="background: #FFF3A3A6;">If there is function overload, the compiler determines which function will be called defending on languages complex rules. We must understand this rules very well</mark>. 

<mark style="background: #FFF3A3A6;">Function overload resolution can be conclude in two different way</mark>:
1. The compiler determined which function called depending on language rule and bind the function call to that function.
2. The compiler trows syntax error in case if function overload resolution error. This can be happening in two different ways:
	1. There is overload, but the function call is not compliance with one of them. In this case, the compiler says there is no function like that.
	2. There are two or more options to choose but the compiler rules doesn't let to choose one of them. That means two of the function is valid for the function call but it is impossible to determine which one is the right one to chose. This situation named <mark style="background: #BBFABBA6;">ambiguity</mark>. If one of the function is not exist, the ambiguity doesn't exist and compile will choose the other one.

 <mark style="background: #FFF3A3A6;">An example to explain how critic is that topic</mark>:
```cpp
void func(long double);
void func (char);

int main()
{
	func (3.4);
}
```
Upper code is a syntax error due to ambiguity. We would expect that the 3.4 is double and depending on type conversion there is no loss to type conversion from double to long double, so the function overload resolution is conclude with long double function call. But this assumption would be wrong and it is. <mark style="background: #FFF3A3A6;">In conclusion we must learn the rules, assumptions are doesn't work here</mark>.

## How function overload resolution works?
<mark style="background: #FFF3A3A6;">Now let's look at how function overload resolution works</mark>. The compiler determines which function will called with 4 step.
### Function overload resolution: Phase 1
The compiler makes the list of functions that has same name and in the same scope when a function call is made. That means the compiler makes the list of functions that can be in function overload resolution. The only thing that the compiler cares is is the functions have same name and in the same scope. This list is named <mark style="background: #BBFABBA6;">function overload set</mark>. Of course, the function signatures must be different if it doesn't that would be syntax error. The arguments that used in function calls doesn't considered in this situation. The functions that assumed into the list is named <mark style="background: #BBFABBA6;">candidate functions</mark>.  For example, there are six candidate function in below code:
```cpp
void func (long double);
void func (char);
void func(int *p);
void func(Color);
void func (nullptr_t);
void func(nullptr_t, int);
```

### Function overload resolution: Phase 2
The compiler checks the function call is legal or not with the arguments that used in function call for each candidate function. The compiler ask that question for each candidate function: Is that function call with those arguments
is legal or a syntax error.

Following functions can't pass the check:
- If the number of arguments doesn't match, the function is eliminated.
- If the there is no automatic type conversion between types, the function is eliminated.

Following functions pass the check:
- If there is automatic type conversion, the the function pass the check and the compiler doesn't care is there a value loss or not with that type conversion.
- If there function has more argument but the excess arguments has default value, the function pass the check.

The functions that pass that check is called <mark style="background: #BBFABBA6;">viable function</mark>. The function call would be legal if there was only one of the viable function.

With another expression, In order for being a viable function, the function has to establish followings:
1. The number of argument that in function call must match the number of the function's argument. (default argument and variadic parameter are included)
2. There must be a legal conversion for each argument to the function's relevant parameter variable
 
```cpp
enum Color {black ,red, green};

void func (long double); // This is legal. There is type converion double to long double
void func (char); // This is legal. There is type conversion double to char
void func(int *p); // This is NOT legal. There is not type conversion double to int*
void func(Color); // This is not legal. There is not type conversion double to Color
void func (nullptr_t); // This is not legal. There is not type conversion double to nullprt_t
void func(nullptr_t, int); // This is not legal. Number of arguments doesn't match
void func(int, int = 10, int = 20); // This is legal. The excess arguments has default value

int main()
{
	func(3.4);
}
```

If there is no viable function left in that second phase, that is a syntax error and names <mark style="background: #BBFABBA6;">no match</mark>. The compiler throws following error message: "no instance of overloaded function "func" matches the argument list". 
If there was one viable function left in that phase, that function would be chosen. That means function overload resolution ends here.
If there are two or more viable functions left the function overload resolution is continue with 3rd phase.
### Function overload resolution: Phase 3
In this phase the viable functions are separated into <mark style="background: #BBFABBA6;">electability categories</mark>. 

The least electable functions are variadic functions.

```cpp
void f(int);
void f(...); // Least electable 
```

There is a special conversion category that we not met yet. We met them when we will look classes. This conversion is <mark style="background: #BBFABBA6;">user-defined conversion</mark>. 

<mark style="background: #ADCCFFA6;">User-defined conversion</mark>: We can define functions for type conversion according to the rules of the language at places where there is no automatic type conversion. The compiler uses the function for the conversion. The conversion functions called <mark style="background: #BBFABBA6;">conversion constructor</mark>. An example of user-defined conversion:
```cpp
// User-defined conversion
class Myclass {
public:
	Myclass();
	Myclass (int); // This is a converion constructor
};

void func (Myclass); // Win over variadic conversion
void func (...);

int main()
{
	func (12);
}
```

User defined conversions are chosen over variadic conversions, but not chosen over all other conversions.

Any other conversions are legal according to the rules of the language and these conversions are called <mark style="background: #BBFABBA6;">standard conversion</mark>.

The mentioned conversions are easily electable between each other with following order (first is least electable):
- variadic conversion
- user-defined conversion
- standard conversion

### Function overload resolution: Phase 4
What if there is more than one standard conversions? Standard conversion are divided into own categories.

There are three standard conversion (first is most electable):
- exact match
- promotion
- other conversion
#### The exact match
The best situation is <mark style="background: #BBFABBA6;">exact match</mark>. That means the parameter types and the argument types are exactly same.
 
```cpp
void func (double);
void func (float);
void func (char);
void func(long); // This is exact match

int main()
{
	func(12L);
}
```

The const conversion is included in exact match. For example the conversion from int* to const int* is accepted exact match.
```cpp
void func(const int *);

int main()
{
	int x = 10;
	func(&X); // The converion from in* to const int* is accepted exact match
}
```

Array decay is accepted as exact match:
```cpp
void func(const int *);

int main()
{
	int a[] = {1, 2, 3};
	func(a); // The type of the argument is int[3] the type is converted to int* type with array decay. That means exact match
}
```

Decay of an function is accepted as exact match:
```cpp
int foo(int);

void func(int (*)(int));

int main()  
{  
	func(foo); // This is exact match because of decay.
}
```

<mark style="background: #FFF3A3A6;">Note</mark>: int and unsigned int or signed int isn't same type. So keep caution on that. signed or unsigned type  are not same.
#### The promotion
There are two type of promotion in C++. These are:
1. Integral promotion (comes with C)
2. float to double promotion

<mark style="background: #ADCCFFA6;">Integral promotion</mark>: Int subspecies are converted to int. 
Int subspecies are:
- short, signed short and unsigned short
- char, unsigned char, signed char
-  bool

There is a special promotion that is float to double. But only float to double. There is no promotion from float to long double or double to long double.

#### The other conversions
If the conversion is not a exact match or promotion, this conversion is least electable between others.

## Let's look some function resolution examples

```cpp
void func(int);
void func (double);
void func(char);

int main()
{
	func(12); // First function is called because of exact match
	func(3.4f); // Second function is called because of float to doble promotion
	funct(12u); // This is ambiguity
}
```

```cpp
void func(long double);
void func(char);

int main()
{
	func(2.3); // ambiguity. There is no promotion from double to long double
}
```

```cpp
void func(bool);
void func (int);
void func(double);

int main()
{
	func(10 > 5); // First function is exact match
}
```

```cpp
void func (int);
void func(double);

int main()
{
	func(10 > 5); // First function is integral promotion
}
```

```cpp
enum color {black, green};
void func(color);
void func(int);
void func (double);

int main()
{
	func(green); // First function is exact match
}
```

```cpp
void func(int);
void func (double);

int main()
{
	func(green); // Syntax error
}
```

```cpp
enum color {black, green};
void func(color);
void func(int);
void func (double);

int main()
{
	func(10); // Second function is exact match and first isn't viable, this is not C
}
```

```cpp
enum color {black, green};  
void func(color);
void func(int);
void func (double);

int main()
{
	func(10.f); // Third is promotion
}
```

```cpp
enum color {black, green};
void func(color);
void func(int);
void func (double);

int main()
{
	func(10u); // Ambiguity, 10L would be ambiguity
}
```

<mark style="background: #FFB8EBA6;">Additional Information</mark>: Null pointer conversion:
Before modern C++ (there is no nullptr_t type) we was using 0 as null pointer: <mark style="background: #D2B3FFA6;">void func(int* p);
</mark>  is called like that: <mark style="background: #D2B3FFA6;">func(0);</mark>. This is named <mark style="background: #BBFABBA6;">null pinter conversion</mark>. We not prefer to use NULL, because there is a reason for this that we will mention later.
The danger here is we want to call the mentioned function, but imagine we include a header file that has function <mark style="background: #D2B3FFA6;">void func(int);</mark>. Now the second function is called because of exact match. This problem is not exist with usage on nullptr_t type. nullptr_t has automatic type conversion to all pointer types, but has not to other types.
<mark style="background: #FFF3A3A6;">Following 2 example is <mark style="background: #FF5582A6;">interview</mark> question</mark>. There are special situation. Keep caution on second one.
```cpp
void func(double);
void func(int *);
void func (int);

int main()
{
	func(0); // Third function is exact match
}
```

```cpp
void func(double);
void func(int *);

int main()
{
	func(0); // Ambiguity. Sytax error. Two of them is standard converion. This is special situatuin. 0 is a nullptr literal
}
```

## Special cases of function overload resolution

1. We mentioned that following is exact match:
```cpp
void func (const int*);
{
	int x = 10;
	funt(&x); // exact match
}
```
But below code is function overloading. This named <mark style="background: #BBFABBA6;">const overloading</mark>.
```cpp
void func(int*);
void func(const int*);
```
Which one is exact match? There must be a rule here that important for language.
Let's look in detail:
```cpp
//const overloading

void func(int*)
{
	std:: cout « "func(int *)In";
}

void func(const int*)
{
	std:: cout « "func(const int *)In";
}

int main()
{
	const int cx = 10;
	func (&cx);
}
```
Upper code is exact match to second function. <mark style="background: #FFF3A3A6;">The first one is not viable.</mark> <mark style="background: #FFF3A3A6;">Because there is no type conversion from <mark style="background: #D2B3FFA6;">const T*</mark> to <mark style="background: #D2B3FFA6;">T*</mark> in C++</mark>. This is not special case. But look at below code:
```cpp
 //const overloading

void func(int*)
{
	std:: cout « "func(int *)In";
}

void func(const int*)
{
	std:: cout « "func(const int *)In";
}

int main()
{
	int cx = 10;
	func (&cx);
}
```
<mark style="background: #FFF3A3A6;">Two functions are viable in upper code. In this situation <mark style="background: #D2B3FFA6;">int*</mark> is called. This is a language rule.</mark> <mark style="background: #FFF3A3A6;">Don't forger two of them are exact match.</mark>
Const overloading is frequently used and there are a lot of examples in the standard library.
Corresponding reference semantic is valid too:
```cpp
void func(int&)
{
	std:: cout « "func(int *)In";
}

void func(const int&)
{
	std:: cout « "func(const int *)In";
}

int main()
{
	int cx = 10;
	func (cx);
}
```

<mark style="background: #FFF3A3A6;">Real example of const overloading</mark>: Strchr function has a problem. Because there is no function overloading in C.
Let's look at Strchr (a C function)
```cpp
char* Strchr (const char*, int c);
int main(
{
	const Shar str[100] = "bugun hava cok guzel";
	char c = 'u';
	auto p = strchr(str, c);

	p = "k"; // Unfedined behavior, we assign a value to a strign literal character
}
```
The return type of Strchr is not const char*. So we can assign a value to returned and pointed char object via the pointer. This can cause undefined behavior if we gave a string literal to the function for search.

This problem can be overcame in C++. If we look that function in C++'s standard library, we can see the function is overloaded:
```cpp
const char* Strehr (const char*, int c);
char* Strchr (char*, int c);

int main(
{
	const Shar str[100] = "bugun hava cok guzel";
	char c = 'u';
	auto p = strchr(str, c);

	p = "k";
}
```
With that syntax, the called function will be the function that has <mark style="background: #D2B3FFA6;">const char*</mark> according to the language rule (string literals must be const char* in C++). And that returns a <mark style="background: #D2B3FFA6;">const char*</mark> type. That solves the problem. We see how important is the const overloading with this example.

<mark style="background: #FFB8EBA6;">Additional Information:</mark> <mark style="background: #FF5582A6;">C interview question</mark>: About C.
```c
int main()
{
	char* p = "bilge"; // array has static lifespan, but p is not
	char str[] = "fatih"; // This has not static lifespan
}
```

2. There is a function overloading that we don't want
```cpp
void func(int);
void func(int &);

int main()
{
	func(10); // This calls first function, because the second is not viable, because we cannot assign a R valeu to an L value

	int x = 10;
	func(x); // This is ambiguity. There is no election criteria between call-by value or call-by reference
}
```

But following syntax is even an ambiguity with an R value. Keep caution on const.
```cpp
void func(int);
void func(const int &);

int main()
{
	func(12); // Ambiguity
}
```

3. Default arguments can make confuse
Following is function overloading, because the signatures of the functions are different. But if we make a call like in the example, it would be ambiguity.
```cpp
void func(int, int = 10);
void func(int);

int main()
{
	func(12); // Ambiguity
}
```

4. What if overloaded functions have many arguments. This situation is looks complex at first glance. But there is a simple rule for that.
```cpp
void f(int, double, long);
void f(float, int, double); 
void f(float, char, bool);
```
<mark style="background: #FFF3A3A6;">The rule is : The function that is chosen must gain the upper hand over others at least one parameter. But it must not be worse at other parameters (can be equal). That means if a function has advantage in one argument and an another function has advantage on different argument, that means ambiguity</mark>
<mark style="background: #FFF3A3A6;">There is a ambiguity if no one ensure the rule. But if one of the function ensure the rule, it is chosen.</mark>

For example: 
First function call: Second function has advantage on 2nd argument and not worst at others. Means the second function is chosen.
Second Function call: Ambiguity. First function has advantage on 3rd argument and second function has advantage on 2nd argument.
```cpp
void f(int, double, long); 
void f(bool, int, double);

int main()
{
	f(7u, 12, 'A');
	f(7u, 12, 6L);
}
```

<mark style="background: #FFB8EBA6;">Additional Note</mark>: We can use type cast operator on the parameter of function call in order to obstruct the ambiguity. This is necessary some times and not a bad technique. For example in case of we use somebody's functions. But don't use type-cast operator that comes from C except some exception. C++ has own type cast operators. These are:
- static_cast
- const_cast
- reinterpret_cast
- dynamic_cast


R value references added to the language in order to use move semantics and perfect forwarding in transition to Modern C++. 

R value referanslar tamamen move semantics ve perfect forwarding ile alakalı. Fonksiyon overload resolution açısından oradaki move semantics i desteklemesi için dilen overload resolution kurallarına Modern C++'la birlikte eklemeler yapıldı. Şimdiye kadar gördüğümüz function overload resolution'da bu yoktu. 

R value referanslar genellikle class'lar ile kullanılır. Fakat şimdilik primitive türlerle syntax açısından bakacağız. Aşağıdaki fonksiyonlar overload:
```cpp
void func(int&);
void func (const int&);
void func (int&&);
```
Bu fonksiyonlara eğer bir L value ile çağrı yaparsak en alttaki viable olmaz. Bu durumda const overloading'e dönüşecek ve en üstteki fonksiyon çağırılacak. 
Eğer const bir L value ile çağırırsak alttaki yine viable olmaz ve ikinci fonksiyon çağırılır.

But if we call the function with R value expression the first one is not viable because L value reference can't connect to R value reference. But last two is viable. That means if there is only one of them is exist, it would be called. <mark style="background: #FFF3A3A6;">In this case the special rule that added with C++11 comes into place. The function that has R value reference is called.</mark> Below is example:
```cpp
void func(int&);
void func (const int&);
void func (int&&);

int main()
{
	func(10); // The third one is callsed. This is a special case
}
```

Another function overload example:
There is implicit type conversion from int* to bool and void*. 
The void* is chosen.
```cpp
void foo(bool);
void foo(void *);

int x{};

int main)  
{  
 foo (&x);
}
```
