# sizeof (\<empty>)?
**complexity:** professional
```cpp
class Empty {};

int main()
{
    cout << sizeof(Empty) << endl;
}
```
Regarding code above what should be present in output?
- A. 0
- B. 1
- C. 2
- D. 4

**Answer:** B

When instantiating an instance of any type (including "empty"), the physical memory must be allocated to it, since `operator &` (address acquisition) can be applied to every object in C ++. According to the Standard, under the "empty" object, the minimum possible amount of memory is allocated - 1 byte.

**See also:** [FAQ by Stroustrup](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 


# virtual destructor
**complexity:** basic
```cpp
struct shape
{
    virtual ~shape()       { cout << "shape destructor" << endl; }
};
struct rectangle : shape
{
   ~rectangle()            { cout << "rectangle destructor" << endl; }
};
struct decorated_rectangle : rectangle
{
   ~decorated_rectangle()  { cout << "decorated rectangle destructor" << endl; }
};

int main()
{
   rectangle* r = new decorated_rectangle();
   delete r;
}
```
Regarding code above what should be present in output?
- A.
    - rectangle destructor
- B.
    - decorated rectangle destructor
    - rectangle destructor
- C.
    - decorated rectangle destructor
    - rectangle destructor
    - shape destructor
- D.
    - shape destructor
    - rectangle destructor
    - decorated rectangle destructor


**Answer:** C

**See also:** [Evvective C++, item 7 by S. Meyers](https://books.google.com.ua/books?id=U7lTySXdFk0C&lpg=PT68&dq=Effective%20C%2B%2B%20declare%20destructors%20virtual&pg=PT68#v=onepage&q&f=false)
 
# class construction
**complexity:** basic
```cpp
struct member
{
   member() { cout << "member" << endl; }
};

struct base
{
   base()  { cout << "base" << endl; }
};

struct derived : base
{
   derived() { cout << "derived" << endl; }
private:
   member m_;
};

int main()
{
   derived obj;
}

```
Regarding code above what should be present in output?
- A.
    - member
    - base
    - derived
- B.
    - base
    - member
    - derived
- C.
    - base
    - derived
- D.
    - base
    - member
    - derived
- F.
    - derived

**Answer:** B 

**See also:** ???
 

# Function overloading. Parameter type: integer vs. pointer
**complexity:** professional
```cpp
void foo(char*) {cout << "pointer argument" << endl; }
void foo(int)   {cout << "integer argument" << endl; }

int main()
{
   foo(NULL);
}
```
Regarding code above what should be present in output?
- A. pointer argument
- B. integer argument
- C. compiler error: ambiguous call to overloaded function

**Answer:** B

**NULL** is predefined macro with integer constant 0 (zero). foo(int) will be resolved due to better conformance between argument & parameter type.  

**See also:** ???
 





