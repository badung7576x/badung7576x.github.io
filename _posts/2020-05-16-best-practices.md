---
layout: post
title: Viết hàm trong python - Best Practices 
date: 2020-05-16
description:  Luyện tập viết hàm trong python ? # Add post description (optional)
img:  # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Python, Function, Cleancode] # add tag
---
Mình đang học 1 khóa học trên datacamp về cách viết 1 funtion trong python. Sau đây là chia sẻ của mình về những kiến thức đã học được.

Các bạn có thể tham khảo khóa học ở đây: [Writing function in python](https://campus.datacamp.com/courses/writing-functions-in-python) 

## 1. Docstring trong function?

Một function trong python nên có thêm phần docstring để mô tả về chức năng của function đó:

```python
def function_name(arguments):
    """
    Description of what the function does
    Description of the arguments, if any.
    Description of the return value(s), if any.
    Description of the error raised, if any.
    Optional extra note or examples of usage.
    """
```

Có 1 số kiểu viết docstring như: Google Style, Numpydoc, reStructedText, EpyText.
Ví dụ về Google Style:

```python
def function(arg_1, arg_2=42):
    """ Description of what the function does.
    Args:
        arg_1 (str): Description of arg_1 that can break onto the next line
                if needed.
        arg_2 (int, optional): Write optional when an argument has a default
                value.
    Returns:
        bool: Optional description of the return value
        Extra lines are not indented.
    Raises:
        ValueError: Include any error types that the function intentionally
        raises.
    Notes:
        See https://www.datacamp.com/community/tutorials/docstrings-python
        for more info.
    """
```

Ví dụ về Numpydoc:

```python
def function(arg_1, arg_2=42):
    """
    Description of what the function does.
    Parameters
    ----------
        arg_1 : expected type of arg_1
        Description of arg_1.
        arg_2 : int, optional
        Write optional when an argument has a default value.
        Default=42.
    Returns
    -------
        The type of the return value
        Can include a description of the return value.
        Replace "Returns" with "Yields" if this function is a generator.
    """
```
Xem docstring của 1 function:

```python
# Cách 1:
print(function_name.__doc__)
# Cách 2:
import inspect
print(inspect.getdoc(function_name))
```

---

## 2. DRY và "Do One Thing"

<b> DRY: Don't repeat yourself </b>

DRY và "Do One Thing" là những nguyên tắc căn bản để viết code clean trong bất kì ngôn ngữ nào.

1. <b>DRY:</b> Đối với những đoạn code được lặp đi, lặp lại trong 1 chương trình, chúng ta nên viết nó lại thành 1 function để sử dụng lại ở nhiều nơi, tránh bị lặp code, giảm lượng code phải viết và dễ dàng maintain hơn.
2. <b>Do One Thing:</b> Một function thì chỉ nên thực hiện 1 chức năng duy nhất, tránh việc xử lý nhiều thứ trong 1 function, sau này sẽ khó maintain.

Những lợi ích khi viết code theo các nguyên tắc:

- More flexible (Code linh hoạt hơn)
- More easily understood (Dễ hiểu hơn)
- Simpler to test and to debug (Đơn giản hơn trong việc test và debug)

---

## 3. Pass by assignment

Python là ngôn ngữ <i><b>dynamically-typed</b></i>. Vì vậy khi code function cần chú đến việc truyền dữ liệu và function:

```python
def foo(x):
    x[0] = 99

my_list = [1,2,3]
foo(my_list)
print(my_list)   # [99,2,3]
```

```python
def foo(x):
    x = x + 99

a = 3
foo(a)
print(a)   # 3
```

Trong 2 đoạn code trên, đoạn 1 thì kết quả in ra dữ liệu trong `my_list` đã thay đổi, còn đoạn 2 thì dữ liệu của biến `a` vẫn dữ nguyên không thay đổi. Tại sao lại vậy?

Giải thích: 

- Khi chúng ta tạo 1 list: `a = [1,2,3]`, python sẽ tạo ra 1 vùng nhớ chứa các giá trị `[1,2,3]` và sau đó chỉ `a` đến vùng nhớ đó. Nếu chúng ta lại gán `b = a` thì `b` cũng được trỏ tới vùng nhớ đó. Khi dữ liệu của biến `a` thay đổi thì `b` cũng bị thay đổi và ngược lại. 

```python
a = [1,2,3]
b = a
print(b)    # [1,2,3]
a.append(5)
print(b)    # [1,2,3,5]
b.append(10)
print(a)    # [1,2,3,5,10]
```

- Đối biến int thì khi chúng ta gán giá trị mới cho biến thì biến đó sẽ trỏ tới 1 vùng nhớ mới khác với vùng nhớ cũ.

```python
a = 5
b = a
print(a, b)     # 5 5
a = a + 5
print(a, b)     # 10 5 => a đã trỏ tới 1 vùng nhớ mới còn b vẫn trỏ tới vùng nhớ cũ
```

**Note**

> <b>Immutable(Bất biến): </b> int, float, bool, string, bytes, tuple, frozenset => Những kiểu biến này không thay đổi giá trị của vùng nhớ.<br><b>Mutable(Khả biến): </b> list, dict, set, bytearray, objects, functions, ... => Những kiểu biến này có thể thay đổi giá trị vùng nhớ.

**Alert:**
Trong function, việc truyền các biến mutable rất là nguy hiểm. Chú ý khi truyền biến mutable vào trong function.

Ví dụ sau sẽ giúp bạn hiểu rõ hơn:

```python
def foo(x = []):
    x.append(1)
    return x

print(foo()) # [1]
print(foo()) # [1,1]  
print(foo()) # [1,1,1]
```

Cách khắc phục ví dụ trên:

```python
def foo(x = None):
    if x is None:
        x = []
    x.append(1)
    return x
```

