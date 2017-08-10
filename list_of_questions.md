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

��� ��������������� ���������� ������ ���� (� ��� ����� � "�������"), ��� ���� ������ ���� �������� **�����������** ���������� ������, ��������� � ������� ������� � �++ ����� ���� �������� `operator &` (��������� ������). ������ ��������� ��� ������� ������ ���������� ���������� ��������� ����� ������ � 1 ����.

**See also:** [Why is the size of an empty class not zero?](http://www.stroustrup.com/bs_faq2.html#sizeof-empty) 



