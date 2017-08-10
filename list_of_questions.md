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

��� ��������������� ���������� ������ ���� (� ��� ����� � "�������"), ��� ���� ������ ���� �������� ����������� ���������� ������, ��������� � ������� ������� � �++ ����� ���� ��������  `operator &`  (��������� ������). ������ ��������� ��� ������� ������ ���������� ���������� ��������� ����� ������ � 1 ����.

**See also:** [Why is the size of an empty class not zero?](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 


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

**See also:** 




