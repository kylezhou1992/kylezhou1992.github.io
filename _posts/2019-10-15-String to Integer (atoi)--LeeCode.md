---
layout:     post
title:      String to Integer (atoi)--LeeCode
subtitle:   CodeTraining String to Integer (atoi)--LeeCode
date:       2019-06-02
author:     MY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - LeeCode
    - C/C++
---


# 字符串转换整数 (atoi)


请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

### Solution1
```c++
class Solution {
public:
    int myAtoi(string str) {
    	bool ispositiveDigit = true;
        bool firstFlag = true;
        long long result = 0;
        if (str.size() == 0) {
            return 0;
        }

        for (size_t i = 0; i < str.size(); i++) {
            if (str[i] == ' ' && firstFlag) {
                continue;
            }
            else if (str[i] == '-' && firstFlag) {
                firstFlag = false;
                ispositiveDigit = false;
            }
            else if (str[i] == '+' && firstFlag) {
                firstFlag = false;
                ispositiveDigit = true;
            }
            else if (str[i] >= '0' && str[i] <= '9') {
                firstFlag = false;
                result = result * 10 + str[i] - '0';
                if (result >= INT_MAX && ispositiveDigit) {
                    return INT_MAX;
                }
                else if (result >= (long long)INT_MAX + 1 && !ispositiveDigit) {
                    return INT_MIN;
                }
            } else {
                return (int)result * (ispositiveDigit ? 1 : -1);
            }
        }
        return (int)result * (ispositiveDigit ? 1 : -1);
    }
};
```

### Time Complexity
O(n)



### Soluton2 ---正则表达式(python Leecode)

^：匹配字符串开头
[\+\-]：代表一个+字符或-字符
?：前面一个字符可有可无
\d：一个数字
+：前面一个字符的一个或多个
\D：一个非数字字符
*：前面一个字符的0个或多个

max(min(数字, 2^31^ - 1),  -2^31^) 用来防止结果越界

```python
class Solution:
    def myAtoi(self, s: str) -> int:
		return max(min(int(*re.findall('^[\+\-]?\d+', s.lstrip())), 2^31 - 1), -2^31)
```