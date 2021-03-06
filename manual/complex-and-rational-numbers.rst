.. _man-complex-and-rational-numbers:

************
 复数和分数
************

Julia 提供复数和分数类型，并对其支持所有的 :ref:`标准数学运算 <man-mathematical-operations>` 。对不同的数据类型进行混合运算时，无论是基础的还是复合的，都会自动使用 :ref:`man-conversion-and-promotion` .

.. _man-complex-numbers:

复数
----

全局变量 ``im`` 即复数 *i* ，表示 -1 的正平方根。因为 ``i`` 经常作为索引变量，所以不使用它来代表复数了。Julia 允许数值文本作为 :ref:`代数系数 <man-numeric-literal-coefficients>` ，也适用于复数：

.. doctest::

    julia> 1 + 2im
    1 + 2im

可以对复数做标准算术运算：

.. doctest::

    julia> (1 + 2im)*(2 - 3im)
    8 + 1im

    julia> (1 + 2im)/(1 - 2im)
    -0.6 + 0.8im

    julia> (1 + 2im) + (1 - 2im)
    2 + 0im

    julia> (-3 + 2im) - (5 - 1im)
    -8 + 3im

    julia> (-1 + 2im)^2
    -3 - 4im

    julia> (-1 + 2im)^2.5
    2.729624464784009 - 6.9606644595719im

    julia> (-1 + 2im)^(1 + 1im)
    -0.27910381075826657 + 0.08708053414102428im

    julia> 3(2 - 5im)
    6 - 15im

    julia> 3(2 - 5im)^2
    -63 - 60im

    julia> 3(2 - 5im)^-1.0
    0.20689655172413793 + 0.5172413793103449im

类型提升机制保证了不同类型的运算对象能够在一起运算：

.. doctest::

    julia> 2(1 - 1im)
    2 - 2im

    julia> (2 + 3im) - 1
    1 + 3im

    julia> (1 + 2im) + 0.5
    1.5 + 2.0im

    julia> (2 + 3im) - 0.5im
    2.0 + 2.5im

    julia> 0.75(1 + 2im)
    0.75 + 1.5im

    julia> (2 + 3im) / 2
    1.0 + 1.5im

    julia> (1 - 3im) / (2 + 2im)
    -0.5 - 1.0im

    julia> 2im^2
    -2 + 0im

    julia> 1 + 3/4im
    1.0 - 0.75im

注意： ``3/4im == 3/(4*im) == -(3/4*im)`` ，因为文本系数比除法优先。

处理复数的标准函数：

.. doctest::

    julia> real(1 + 2im)
    1

    julia> imag(1 + 2im)
    2

    julia> conj(1 + 2im)
    1 - 2im

    julia> abs(1 + 2im)
    2.23606797749979

    julia> abs2(1 + 2im)
    5
    
    julia> angle(1 + 2im)
    1.1071487177940904

As usual, the absolute value (``abs``) of a complex number is its
distance from zero. The ``abs2`` function gives the square of the
absolute value, and is of particular use for complex numbers where it
avoids taking a square root. The ``angle`` function returns the phase
angle in radians (also known as the *argument* or *arg* function). 所有的 :ref:`man-elementary-functions` 也可以应用在复数上：

.. doctest::

    julia> sqrt(1im)
    0.7071067811865476 + 0.7071067811865475im

    julia> sqrt(1 + 2im)
    1.272019649514069 + 0.7861513777574233im

    julia> cos(1 + 2im)
    2.0327230070196656 - 3.0518977991517997im

    julia> exp(1 + 2im)
    -1.1312043837568138 + 2.471726672004819im

    julia> sinh(1 + 2im)
    -0.48905625904129374 + 1.4031192506220407im

作用在实数上的数学函数，返回值一般为实数；作用在复数上的，返回值为复数。例如， ``sqrt`` 对 ``-1`` 和 ``-1 + 0im`` 的结果不同，即使 ``-1 == -1 + 0im`` ：

.. doctest::

    julia> sqrt(-1)
    ERROR: DomainError
    sqrt will only return a complex result if called with a complex argument.
    try sqrt(complex(x))
     in sqrt at math.jl:284

    julia> sqrt(-1 + 0im)
    0.0 + 1.0im

:ref:`代数系数 <man-numeric-literal-coefficients>` 不能用于使用变量构造复数。乘法必须显式的写出来：

.. doctest::

    julia> a = 1; b = 2; a + b*im
    1 + 2im

但是， *不* 推荐使用上面的方法。推荐使用 ``complex`` 函数构造复数：

.. doctest::

    julia> complex(a,b)
    1 + 2im

这种构造方式避免了乘法和加法操作。

``Inf`` 和 ``NaN`` 也可以参与构造复数 (参考 :ref:`man-special-floats` 部分)：

.. doctest::

    julia> 1 + Inf*im
    complex(1.0,Inf)

    julia> 1 + NaN*im
    complex(1.0,NaN)


.. _man-rational-numbers:

分数
----

Julia 有分数类型。使用 ``//`` 运算符构造分数：

.. doctest::

    julia> 2//3
    2//3

如果分子、分母有公约数，将自动约简至最简分数，且分母为非负数：

.. doctest::

    julia> 6//9
    2//3

    julia> -4//8
    -1//2

    julia> 5//-15
    -1//3

    julia> -4//-12
    1//3

约简后的分数都是唯一的，可以通过分别比较分子、分母来确定两个分数是否相等。使用 ``num`` 和 ``den`` 函数来取得约简后的分子和分母：

.. doctest::

    julia> num(2//3)
    2

    julia> den(2//3)
    3

其实并不需要比较分数和分母，我们已经为分数定义了标准算术和比较运算：

.. doctest::

    julia> 2//3 == 6//9
    true

    julia> 2//3 == 9//27
    false

    julia> 3//7 < 1//2
    true

    julia> 3//4 > 2//3
    true

    julia> 2//4 + 1//6
    2//3

    julia> 5//12 - 1//4
    1//6

    julia> 5//8 * 3//12
    5//32

    julia> 6//5 / 10//7
    21//25

分数可以简单地转换为浮点数：

.. doctest::

    julia> float(3//4)
    0.75

分数到浮点数的转换遵循，对任意整数 ``a`` 和 ``b`` ，除 ``a == 0`` 及 ``b == 0`` 之外，有：

.. doctest::

    julia> isequal(float(a//b), a/b)
    true

可以构造结果为 ``Inf`` 的分数：

.. doctest::

    julia> 5//0
    Inf

    julia> -3//0
    -Inf

    julia> typeof(ans)
    Rational{Int64} (constructor with 1 method)

但不能构造结果为 ``NaN`` 的分数：

.. doctest::

    julia> 0//0
    ERROR: invalid rational: 0//0
     in Rational at rational.jl:7
     in // at rational.jl:17

类型提升系统使得分数类型与其它数值类型交互非常简单：

.. doctest::

    julia> 3//5 + 1
    8//5

    julia> 3//5 - 0.5
    0.09999999999999998

    julia> 2//7 * (1 + 2im)
    2//7 + 4//7im

    julia> 2//7 * (1.5 + 2im)
    0.42857142857142855 + 0.5714285714285714im

    julia> 3//2 / (1 + 2im)
    3//10 - 3//5im

    julia> 1//2 + 2im
    1//2 + 2//1im

    julia> 1 + 2//3im
    1//1 - 2//3im

    julia> 0.5 == 1//2
    true

    julia> 0.33 == 1//3
    false

    julia> 0.33 < 1//3
    true

    julia> 1//3 - 0.33
    0.0033333333333332993

