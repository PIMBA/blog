---
title: Java String.valueOf
date: 2018-04-21 11:11:35
tags:
---

Java `String.valueOf` 方法对`null`参数的表现有以下问题:

```java
final Map<String,Object> map = new HashMap<>();
assert String.valueOf(map.get("a")).equals("null"); // true

assert String.valueOf(null).equals("null"); // NPE
```

究其原因是因为`String.valueOf`的重载，上面的方法使用了

```java
    /**
     * Returns the string representation of the {@code Object} argument.
     *
     * @param   obj   an {@code Object}.
     * @return  if the argument is {@code null}, then a string equal to
     *          {@code "null"}; otherwise, the value of
     *          {@code obj.toString()} is returned.
     * @see     java.lang.Object#toString()
     */
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }
```

下面的方法使用了

```java
    /**
     * Returns the string representation of the {@code char} array
     * argument. The contents of the character array are copied; subsequent
     * modification of the character array does not affect the returned
     * string.
     *
     * @param   data     the character array.
     * @return  a {@code String} that contains the characters of the
     *          character array.
     */
    public static String valueOf(char data[]) {
        return new String(data);
    }
```

查看源代码后发现`valueOf`方法还有其他重载

```java
public static String valueOf(long);
public static String valueOf(int);
public static String valueOf(char);
public static String valueOf(float);
public static String valueOf(double);
public static String valueOf(boolean);
```

这些方法由于是基本类型，不能赋`null`所以不会被调用，调用重载函数的时候，如果未指明类型，那么会得到最匹配类型的方法调用，比如如果未指定`null`的类型，那么默认`null`是`char[]`类型，如果是一个返回泛型的方法，那么这个方法会被java解析为返回`Object`类型。