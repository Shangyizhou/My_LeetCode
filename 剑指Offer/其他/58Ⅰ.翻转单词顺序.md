## 题目

#### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)



输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

**示例 1：**

```
输入: "the sky is blue"
输出: "blue is sky the"
```

**示例 2：**

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

**示例 3：**

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

 

**说明：**

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

## 思路

- 我们先去除多余的空格
  - 去除开头的空格
  - 去除单词间多余的空格
- 反转整个字符串（双指针）
- 反转各个单词（双指针）

## 源代码

```C
class Solution {
public:
    // 双指针法反转整个句子
    void reverse(int start, int end, string& s) {
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
    // 去除空格
    void remove(string& s) {
        if (s.size() == 0) return;

        // 去除之前的空格
        int slowIndex = 0, fastIndex = 0;

        // 让快指针越过开头的空格 
        while (fastIndex < s.size() && s[fastIndex] == ' ') {
            fastIndex++;
        }

        // 删除单词之间多余的空格
        for ( ; fastIndex < s.size(); fastIndex++) {
            // _ _ab_ _ _cde 快指针会到达c
            if (fastIndex - 1 >= 0 && s[fastIndex - 1] == s[fastIndex] && s[fastIndex] == ' ') {
                continue;
            } else {
                // 过程
                // a_ab_ _ _cde -> abab_ _ _cde -> ab_b_ _ _cde
                // ab_b_ _ _cde -> ab_c_ _ _cde -> ab_cd_ _cde -> ab_cde_cde(此时慢指针是指向`_`的)
                // 或者a_ab_ _ _cde_ -> a_cde_cde_(此时慢指针指向c)
                s[slowIndex++] = s[fastIndex];
            }
        }
        // a_ab_ _ _cde_ -> a_cde_cde_(此时慢指针指向c)
        if (slowIndex - 1 > 0 && s[slowIndex - 1] == ' ') {
            // 第slowIndex - 1位置为空，即第slowIndex个元素为空，需要设定之前的长度，则为slowIndex - 1的长度
            s.resize(slowIndex - 1);
        } else {
            // ab_cde_cde(此时慢指针是指向`_`的)
            s.resize(slowIndex);
        }
    }
    string reverseWords(string s) {
        // 首先得到去除多余空格的字符串
        remove(s);
        // 然后反转整个字符串
        reverse(0, s.size() - 1, s);
        // 最后反转各个单词
        for (int i = 0; i < s.size(); i++) {
            int tmp = i;
            while (tmp < s.size() && s[tmp] != ' ') tmp++;
            reverse(i, tmp - 1, s);
            // 之后i会加1
            i = tmp;
        }
        return s;
    }
};
```

