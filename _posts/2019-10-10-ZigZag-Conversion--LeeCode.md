---
layout:     post
title:      ZigZag Conversion--LeeCode
subtitle:   CodeTraining ZigZag Conversion--LeeCode
date:       2019-10-10
author:     MY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - LeeCode
    - C/C++
---

# ZigZag Conversion
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
请你实现这个将字符串进行指定行数变换的函数：
string convert(string s, int numRows);
示例 1:
```C++
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

###Solution1
按照每一行的规则进行一行一行插入，比如第一行和最后一行的数据差值是2 × numRows - 2，其余行的差别和都是numRows的倍数
Coding:
```C++
class Solution {
public:
    string Solution::convert(string s, int numRows) {
        string result;
        int group = numRows * 2 - 2;
        int len = s.size();
        int j;

        if (len == 0)
            return result;
        if (numRows == 1) {
            return s;
        }

        for (int i = 0; i < numRows; i++) {
            int n = 0;
            int val = 0;
            j = i;
            while (j < len) {
                if (j == 0 || j % group == 0 || (j % group + 1) % numRows == 0) {
                    result.push_back(s[j]);
                    j = j + group;
                }
                else {
                    result.push_back(s[j]);
                    val = (2 * n + 1) * group;
                    if (val > j && val - j < len) {
                            result.push_back(s[val - j]);
                    }
                    j = j + group;
                }
                n++;

            }
        }
        return result;
    }
}
```
### Time Complexity1
```C++
O(n)，其中 n == \text{len}(s)n==len(s)。每个索引被访问一次。
```

###Solution2: 按行排序(LeeCode)
思路

通过从左向右迭代字符串，我们可以轻松地确定字符位于 Z 字形图案中的哪一行。

算法

我们可以使用min(numRows,len(s)) 个列表来表示 Z 字形图案中的非空行。

从左到右迭代 ss，将每个字符添加到合适的行。可以使用当前行和当前方向这两个变量对合适的行进行跟踪。

只有当我们向上移动到最上面的行或向下移动到最下面的行时，当前方向才会发生改变。
```C++
Class Solution {
public:
	string convert(string s, int numRows) {
		 if (numRows == 1) return s;

        vector<string> rows(min(numRows, int(s.size())));
        int curRow = 0;
        bool goingDown = false;

        for (char c : s) {
            rows[curRow] += c;
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        string ret;
        for (string row : rows) ret += row;
        return ret;
	}
}
```
### Time Complexity2
```C++
时间复杂度：O(n)，其中 n == \text{len}(s)n==len(s)
空间复杂度：O(n)
```

### Solution3 : 按行访问 (Leecode)
思路

按照与逐行读取 Z 字形图案相同的顺序访问字符串。

算法

首先访问 行 0 中的所有字符，接着访问 行 1，然后 行 2，依此类推...

对于所有整数 kk，

行 0 中的字符位于索引 k (2⋅numRows−2) 处;
行 numRows−1 中的字符位于索引 k (2⋅numRows−2)+numRows−1 处;
内部的 行 ii 中的字符位于索引k(2⋅numRows−2)+i 以及(k+1)(2⋅numRows−2)−i 处;
```C++
class Solution {
public:
    string convert(string s, int numRows) {

        if (numRows == 1) return s;

        string ret;
        int n = s.size();
        int cycleLen = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                ret += s[j + i];
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret += s[j + cycleLen - i];
            }
        }
        return ret;
    }
};
```

### Time Complexity3
```C++
时间复杂度：O(n)，其中 n == \text{len}(s)n==len(s)。每个索引被访问一次。
空间复杂度：O(n)。对于 C++ 实现，如果返回字符串不被视为额外空间，则复杂度为 O(1)O(1)。
```