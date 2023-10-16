---
title: BigDecimal(double)和BigDecimal(String)的区别
tags:
  - BigDecimal(double)
  - BigDecimal(String)
categories:
  - BigDecimal(String)
  - BigDecimal(double)
top_img: /img/bigdecimal.jpg
cover: /img/bigdecimal.jpg
keywords: BigDecimal
description: BigDecimal(double)和BigDecimal(String)的区别
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
abbrlink: ce3886d7
date: 2022-10-03 19:30:18
---
先说答案，BigDecimal(double)和BigDecimal(String)有区别，而且区别很大。

因为double是不精确的，所以使用一个不精确的数字来创建BigDecimal，得到的数字也不是精确的。如0.1这个数字，double只能表示它的近似值。

所以，当我们使用BigDecimal(0.1)创建一个BigDecimal的时候，其实创建出来的值并不是正好等于0.1的。

而是0.1.00000000000055511151231257827021181583404541015625。这是因为double自身表示的只是一个近似值。

而对于BigDecimal(String)，当我们使用new BigDecimal("0.1")创建一个BigDecimal的时候，其实创建出来的值正好就是等于0.1的。


那么它的标度就是1。


## 扩展知识
在《阿里巴巴Java开发手册》有一条建议，或者要求是说：
![图片](/img/decimal-wenzhang-1.png)


**BigDecimal如何精确计数？**

通过BigDecimal的源码，我们可以看到，实际上一个BigDecimal是**一个“无标度值”和一个标度来表示一个数的**。


在BigDecimal中，标度是通过scale字段来表示的。

而无标度值的表示比较复杂。当unscale value超过阈值(默认为Long.MAX_VALUE)时采用intVal字段存储unscale value, intCompact字段存储Long.MIN_VALUE,否则对unscale value进行压缩存储到long型的intCompact字段用于后续计算，intVal为空。

涉及到的字段就是这几个：
```
public class BigDecimal extends Number implements Comparable<BigDecimal> {
    /**
     * The unscaled value of this BigDecimal, as returned by {@link
     * #unscaledValue}.
     *
     * @serial
     * @see #unscaledValue
     */
    private final BigInteger intVal;

    /**
     * The scale of this BigDecimal, as returned by {@link #scale}.
     *
     * @serial
     * @see #scale
     */
    private final int scale;  // Note: this may have any value, so
                              // calculations must be done in longs
    
    /**
     * If the absolute value of the significand of this BigDecimal is
     * less than or equal to {@code Long.MAX_VALUE}, the value can be
     * compactly stored in this field and used in computations.
     */
    private final transient long intCompact;
    
}
```


**那么标度是什么呢？**


除了scale这个字段，在BigDecimal中还提供了scale()方法，用来返回这个BigDecimal的标度。

```
    /**
     * Returns a {@code BigDecimal} whose scale is the specified
     * value, and whose value is numerically equal to this
     * {@code BigDecimal}'s.  Throws an {@code ArithmeticException}
     * if this is not possible.
     *
     * <p>This call is typically used to increase the scale, in which
     * case it is guaranteed that there exists a {@code BigDecimal}
     * of the specified scale and the correct value.  The call can
     * also be used to reduce the scale if the caller knows that the
     * {@code BigDecimal} has sufficiently many zeros at the end of
     * its fractional part (i.e., factors of ten in its integer value)
     * to allow for the rescaling without changing its value.
     *
     * <p>This method returns the same result as the two-argument
     * versions of {@code setScale}, but saves the caller the trouble
     * of specifying a rounding mode in cases where it is irrelevant.
     *
     * <p>Note that since {@code BigDecimal} objects are immutable,
     * calls of this method do <i>not</i> result in the original
     * object being modified, contrary to the usual convention of
     * having methods named <tt>set<i>X</i></tt> mutate field
     * <i>{@code X}</i>.  Instead, {@code setScale} returns an
     * object with the proper scale; the returned object may or may
     * not be newly allocated.
     *
     * @param  newScale scale of the {@code BigDecimal} value to be returned.
     * @return a {@code BigDecimal} whose scale is the specified value, and
     *         whose unscaled value is determined by multiplying or dividing
     *         this {@code BigDecimal}'s unscaled value by the appropriate
     *         power of ten to maintain its overall value.
     * @throws ArithmeticException if the specified scaling operation would
     *         require rounding.
     * @see    #setScale(int, int)
     * @see    #setScale(int, RoundingMode)
     */
    public BigDecimal setScale(int newScale) {
        return setScale(newScale, ROUND_UNNECESSARY);
    }
```
那么scale表示的是什么，上面的注释已经解释了。

`如果scale为零或正值，则该值表示这个数字小数点右侧的位数。如果scale为负数，则该数字的无标度值需要乘以10的该负数的绝对值的幂。例如：scale为-3，则这个数需要乘1000，即在末尾有3个0。`

如123.123，那么如果使用BIgDecimal表示，那么它的无标度值为123123，它的标度为3。

而二进制无法表示的0.1，使用BigDecimal就可以表示了，及通过无标度值1和标度值1来表示。


在BigDecimal中一共有以下4个构造方法：

```
BigDecimal(int)
BigDecimal(double)
BigDecimal(long)
BigDecimal(String)
```

以上4个方法，创建出来的BigDecimal的标度(scale)是不同的。

其中BigDecimal(int)和BigDecimal(long) 比较简单，因为都是整数，所以他们的标度都是0。


需要注意的是，用BigDecimal(String)创建对象，new BigDecimal("0.10000")和new BigDecimal("0.1")这两个数的标度分别是5和1，如果使用BigDecimal的equals方法比较，得到的结果是false。

想要创建一个精确到0.1的BigDecimal，可以使用以下两种方式：

```
BigDecimal decimal1 = new BigDecimal("0.1");

BigDecimal decimal2 = new BigDecimal.valueOf(0.1);
```

示例如下：

![图片](/img/decimal-wenzhang-2.png)

