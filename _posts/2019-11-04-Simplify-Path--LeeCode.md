---
layout:     post
title:      Simplify Path--LeeCode
subtitle:   CodeTraining Simplify Path--LeeCode
date:       2019-11-04
author:     MY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - LeeCode
    - C/C++
---

# Solution
以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

首先将string对象中的非"/"字符串截取放在sub的vector序列中，然后根据处理".", ".."的规则，将处理完后的字符串放入res的vector序列中，最后将res存入string返回。

## 问题点
1.用例需要在编程前考虑完整
2.代码的逻辑顺序提前考虑完整，不然很容易写出bug

```c++
class Solution {
public:
    string simplifyPath(string path) {
    	std::vector<string> sub, res;
        string str;
        string::size_type l = 0;
        string::size_type r = 0;
        while ((l = path.find_first_not_of('/', l)) != std::string::npos) {
            string tmp;
            r = path.find('/', l);
            if (r == std::string::npos) {
                tmp = path.substr(l);
                sub.push_back(tmp);
                break;
            }
            else {
                tmp = path.substr(l, r - l);
                l = r;
                sub.push_back(tmp);
            }
        }

        for (std::vector<string>::size_type i = 0; i < sub.size(); i++) {
            if (sub[i].compare(".") == 0) {		
                continue;
            }
            else if (sub[i].compare("..") == 0) {
                if (res.size() != 0) {
                    res.pop_back();
                }
                else {
                    continue;
                }	
            }
            else {
                res.push_back(sub[i]);
            }
        }

        for (std::vector<string>::size_type i = 0; i < res.size(); i++) {
            str += "/";
            str += res[i];
        }
        if (str.size() == 0) {
            str += '/';
        }
        return str;
    }
};
```

# TimeComplexity
O(N)

# SpaceComplexity
O(3N)