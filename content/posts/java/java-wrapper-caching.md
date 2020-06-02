+++ 
draft = false
date = 2020-05-31T19:18:47+05:30
title = "Wrapper Class Caching"
description = ""
slug = "" 
author = "Akhilesh Dubey"
tags = ["java","heap","stack"]
categories = ["java"]
externalLink = ""
series = []
showthedate = true
+++

### Wrapper Class Caching: 
Added in JAVA 5. It is used to improve the performance and save memory. It only works on autoboxing.

- Byte : ``` private static ByteCache ``` inner class : Range -127 to +127
- Short : ``` private static ShortCache ``` inner class : Range -127 to +127
- Long : ``` private static LongCache ``` inner class : Range -127 to +127
- Integer : ``` private static IntegerCache ``` inner class : Range -127 to +127
- Character : ``` private static CharacterCache ``` inner class : Range 0 to +127
      
We can be modified this range only for Integer by using a VM argument -XX:AutoBoxCacheMax=size

``` java

    Integer x = 10; //autoboxing
    Integer y = Integer.valueOf(10); //inside story
    Integer z = new Integer(100);
    
    System.out.println(x == y); // true
    System.out.println(x == z); // false
    System.out.println(y == z); // false
    
    Integer x1 = 128; //autoboxing
    Integer x2 = 128; //autoboxing
    System.out.println(x1 == x2); // false   
    
    // Integer's valueOf API
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }

```
