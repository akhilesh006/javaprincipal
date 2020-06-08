+++ 
draft = false
date = 2020-06-08T11:55:55+05:30
title = "Common Tips & Tricks for Competitive Programming"
description = ""
slug = "" 
tags = ["Competitive Programming"]
categories = []
externalLink = ""
series = []
showthedate = true
+++

### Common Tips & Tricks for Competitive Programming.

### Big-O: Time Complexity Order
```
O(1) < O(log n) < O(\sqrt{n}) < n < n log n < n<sup>2</sup> < n<sup>3</sup> ... < 2<sup>n</sup> < 3<sup>n</sup> ... < n! < n<sup>n</sup>
```

- #### <sup>n</sup>C<sub>r</sub>

If you have a list of numbers and wants to count the number of pair can be formed in a list. 
e.g. 
[Problem Statement : CodeChef : PLMU](https://www.codechef.com/DEC19B/problems/PLMU)
- Given an array of N integers, find the number of pairs of (i,j) that satisfies the condition 
A[i]+A[j] = A[i]*A[j].

Only 2 and 0 will satisfy the above condition.
So, Count the number of 2s let say n and 0s ley say m in the input array,
and calculate the <sup>n</sup>C<sub>2</sub> and <sup>m</sup>C<sub>2</sub>.
and add them, this will be the answer.

Formula: 

- <sup>n</sup>C<sub>r</sub> = n! / r! * (n - r)!
- <sup>n</sup>C<sub>0</sub> = 1
- <sup>n</sup>C<sub>n</sub> = 1
- <sup>n</sup>C<sub>1</sub> = n
- 0! = 1
- <sup>n</sup>C<sub>r</sub> = <sup>n</sup>C<sub>n-r</sub>

```java
    // Using DP
    static int[][] cr = new int[33][33];
    // fill the array with -1.
    for(int i = 0; i <= 32; i++){
			Arrays.fill(cr[i], -1);
    }
	public static int ncr(int n , int r){
		if(cr[n][r] != -1)
			return cr[n][r];
		if( r == 0 || n == r)
			return 1;
		int ans = (ncr(n-1,r)+ncr(n-1,r-1)) % (int)1E9;
		return cr[n][r] = ans;
	}
```
```java
    static int ncr(int n, int r) { 
        long a = 1, b = 1;   
        // C(n, r) == C(n, n-r),  
        // choosing the smaller value  
        if (n - r < r) { 
            r = n - r; 
        }   
        if (r != 0) { 
            while (r > 0) { 
                a *= n; 
                b *= r; 
                // gcd of a, b  
                long x = gcd(a, b);   
                a /= x; 
                b /= x;
                n--; 
                r--; 
            }
        } else { 
            a = 1; 
        } 
        return a;
    } 
  
    static long gcd(long n1, long n2) { 
        long gcd = 1; 
        for (int i = 1; i <= n1 && i <= n2; ++i) { 
            if (n1 % i == 0 && n2 % i == 0) { 
                gcd = i; 
            } 
        } 
        return gcd; 
    }
```




