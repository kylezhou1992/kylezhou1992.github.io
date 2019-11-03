---
layout:     post
title:      Integer to Roman--LeeCode
subtitle:   CodeTraining Integer to Roman--LeeCode
date:       2019-10-19
author:     MY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - LeeCode
    - C/C++
---


# Integer to Roman
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

# Solution1
```c++
string Solution::intToRoman(int num) {
	string result;
	int rem = num;
	int digit;
	int bitFlag = 0;
	Solution s;
	while (rem > 0) {
		digit = rem % 10;
		rem = rem / 10;
		result = s.digitToRoman(digit, bitFlag) + result;
		if (rem > 0) {
			bitFlag++;
		}
	}
	return result;
}

string Solution::digitToRoman(int digit, int bit) {
	string rstr;
	switch (digit) {
		case 1:
		case 2:
		case 3:
			for (int i = 0; i < digit; i++) {
				if (bit == 0) {
					rstr += 'I';
				}
				else if (bit == 1) {
					rstr += 'X';
				}
				else if (bit == 2) {
					rstr += 'C';
				}
				else if (bit == 3) {
					rstr += 'M';
				}
			}
			break;
		case 4:
			if (bit == 0) {
				rstr += "IV";
			}
			else if (bit == 1) {
				rstr += "XL";
			}
			else if (bit == 2) {
				rstr += "CD";
			}
			break;
		case 5:
			if (bit == 0) {
				rstr += 'V';
			}
			else if (bit == 1) {
				rstr += 'L';
			}
			else if (bit == 2) {
				rstr += 'D';
			}
			break;
		case 6:
			if (bit == 0) {
				rstr += "VI";
			}
			else if (bit == 1) {
				rstr += "LX";
			}
			else if (bit == 2) {
				rstr += "DC";
			}
			break;
		case 7:
			if (bit == 0) {
				rstr += "VII";
			}
			else if (bit == 1) {
				rstr += "LXX";
			}
			else if (bit == 2) {
				rstr += "DCC";
			}
			break;
		case 8:
			if (bit == 0) {
				rstr += "VIII";
			}
			else if (bit == 1) {
				rstr += "LXXX";
			}
			else if (bit == 2) {
				rstr += "DCCC";
			}
			break;
		case 9:
			if (bit == 0) {
				rstr += "IX";
			}
			else if (bit == 1) {
				rstr += "XC";
			}
			else if (bit == 2) {
				rstr += "CM";
			}
			break;
		default:
			break;
	}
	return rstr;
}
```

### TIme Complexity
O(1)