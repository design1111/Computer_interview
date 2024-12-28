# 一、字符串



## [6. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
- `1 <= numRows <= 1000`

题解：

这道题的规律是什么呢，第一行字母依次为（i+行数），第二行为（第2个字母+行数-1），第三行为（第2个字母+行数-2），后面依次。。。最后一行为（第3个字母+行数）。还是会错误，先这样吧。





## [14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

**提示：**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` 仅由小写英文字母组成

题解：这道题先找到字符串数组中最小的字符串作为判断的一个标准，然后依次用这个标准和每个字符串进行对比，有不匹配的就删除。

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(!strs.size())
            return "";
        string s=strs[0];
        for(int i=1;i<strs.size();i++)
        {
            if(strs[i-1].size()<strs[i].size() )
                s = strs[i-1];
        }
        // cout<<s<<endl;
        for(int i=0;i<strs.size();i++)
        {
            for(int j=0;j<s.size();j++)
            {
                if(strs[i][j] != s[j])
                    s.erase(j,s.size());
            }
        }
        return s;
    }
};
```



## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

**提示：**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

题解：非常精妙的解法，使用substr()函数来截取一部分字符串进行判断。

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n=haystack.size(),m=needle.size();
        for(int i=0;i+m<=n;i++)
        {
            if(haystack.substr(i,m) == needle)
                return i;
        }
        return -1;
    }
};
```



## [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 **最后一个** 单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

 

**示例 1：**

```
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为5。
```

**示例 2：**

```
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为4。
```

**示例 3：**

```
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为6的“joyboy”。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅有英文字母和空格 `' '` 组成
- `s` 中至少存在一个单词

```
class Solution {
public:
    int lengthOfLastWord(string s) {
        vector<string>str;
        s += " ";
        string s1="";
        for(char ss:s)
        {
            if(ss != ' ')
                s1 += ss;
            if(ss == ' ' && s1 != "")
            {
                str.emplace_back(s1);
                s1="";
            } 
        }
        return str.back().size();
    }
};
```





## [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入: s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。
```

**示例 2：**

```
输入：s = "race a car"
输出：false
解释："raceacar" 不是回文串。
```

**示例 3：**

```
输入：s = " "
输出：true
解释：在移除非字母数字字符之后，s 是一个空字符串 "" 。
由于空字符串正着反着读都一样，所以是回文串。
```

方法1为自己写大小写的判定和转换；

方法2为使用API进行大小写转换

（1）方法1，双指针方法，大小写自己判断

```
//方法1：自己写大小写转换，以及特殊字符判定
class Solution {
public:
    bool isPalindrome(string s) {
        
        string sgood;
        for (char ch : s)
        {
            sgood += strFun(ch);
        }
        
        string sgood1;
        for (char ch:sgood)
        {
            if (ch != ' ')
                sgood1 += ch;
        }
        /*cout << sgood1;
        return 1;*/
        int n = sgood1.size();
        int left = 0, right = n - 1;
        while (left < right)
        {
            if (sgood[left] != sgood[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
    char strFun(char ch)
    {
        if ((ch >= '0' && ch <= '9') || (ch >= 'a' && ch <= 'z'))
            return ch;
        else if (ch >= 'A' && ch <= 'Z')
            return ch + 'a' - 'A';
        else
            return ' ';
    }
};
```

```
//方法2：使用API进行大小写转换，双指针法
class Solution1 {
public:
    bool isPalindrome(string s) {
        string sgood;
        for (char ch : s)
        {
            if (isalnum(ch))
                sgood += tolower(ch);
        }
        int n = sgood.size();
        int left = 0, right = n - 1;
        while (left < right)
        {
            if (sgood[left] != sgood[right])
                return false;

            left++;
            right--;
        }
        return true;
    }
};
```



```
#include<iostream>
#include<ctype.h>
using namespace std;

int main()
{
    Solution mySolution;
    cout<<mySolution.isPalindrome("A man, a plan, a canal: Panama");
}
```

（3）方法三：将字符串翻转，然后两个字符串判断是否相等。

```
class Solution {
public:
    bool isPalindrome(string s) {
        string str,str1;
        for(char ss:s)
        {
            if(ss>='a' && ss<='z')
                str += ss;
            if(ss>='A' && ss<='Z')
                str += strFun(ss);
            if(ss >= '0' && ss <= '9')
                str +=ss;
        }
        str1=str;
        reverse(str1.begin(),str1.end());
        if(str1 == str)
            return true;
        return false;
    }
    char strFun(char ch)
    {
        if ((ch >= '0' && ch <= '9') || (ch >= 'a' && ch <= 'z'))
            return ch;
        else if (ch >= 'A' && ch <= 'Z')
            return ch + 'a' - 'A';
        else
            return ' ';
    }
};
```



## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词

**进阶：**如果字符串在你使用的编程语言中是一种可变数据类型，请尝试使用 `O(1)` 额外空间复杂度的 **原地** 解法。

题解：

（1）这里的笨办法，先建立一个字符串数组存入这些单词，然后使用reverse()函数进行数组翻转，再将翻转的字符串数组存入字符串中，比较麻烦。

```
class Solution {
public:
    string reverseWords(string s) {
        s += " ";
        string str;
        vector<string>s1;
        for(char ss:s)
        {
            if(ss != ' ')
                str += ss;
            if(ss == ' ' && str != "")
            {
                s1.emplace_back(str);
                str = "";
            }
        }
        str="";
        reverse(s1.begin(),s1.end());
        for(int i=0;i<s1.size();i++)
        {
            str += s1[i];
            str +=" ";
        }
        str.erase(str.size()-1,str.size());
        return str;
    }
};
```





## [168. Excel表列名称](https://leetcode.cn/problems/excel-sheet-column-title/)

给你一个整数 `columnNumber` ，返回它在 Excel 表中相对应的列名称。

例如：

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

 

**示例 1：**

```
输入：columnNumber = 1
输出："A"
```

**示例 2：**

```
输入：columnNumber = 28
输出："AB"
```

**示例 3：**

```
输入：columnNumber = 701
输出："ZY"
```

**示例 4：**

```
输入：columnNumber = 2147483647
输出："FXSHRXW"
```

 题解：十进制转26进制，26个字母对应下标为（0~25）；所以(columnNumber - 1) % 26；

```
#include<iostream>

using namespace std;
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string str;
        while (columnNumber > 0)
        {

            str += char((columnNumber - 1) % 26 + 'A');
            columnNumber = (columnNumber - ((columnNumber - 1) % 26)) / 26;

        }
        reverse(str.begin(), str.end());
        return str;
    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.convertToTitle(28);
}
```

## [171. Excel 表列序号](https://leetcode.cn/problems/excel-sheet-column-number/)

给你一个字符串 `columnTitle` ，表示 Excel 表格中的列名称。返回 *该列名称对应的列序号* 。

例如：

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

 

**示例 1:**

```
输入: columnTitle = "A"
输出: 1
```

**示例 2:**

```
输入: columnTitle = "AB"
输出: 28
```

**示例 3:**

```
输入: columnTitle = "ZY"
输出: 701
```

 

**提示：**

- `1 <= columnTitle.length <= 7`
- `columnTitle` 仅由大写英文组成
- `columnTitle` 在范围 `["A", "FXSHRXW"]` 内

```
#include<iostream>
using namespace std;

class Solution {
public:
    int titleToNumber(string columnTitle) {
        reverse(columnTitle.begin(), columnTitle.end());
        int i = 0,num=0;
        for (char ch : columnTitle)
        {
            num = num + int(ch - 'A' + 1) * pow(26,i);
            i++;
        }
        return num;
    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.titleToNumber("FXSHRXW");
}
```



## [205. 同构字符串](https://leetcode.cn/problems/isomorphic-strings/)

给定两个字符串 `s` 和 `t` ，判断它们是否是同构的。

如果 `s` 中的字符可以按某种映射关系替换得到 `t` ，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

**示例 1:**

```
输入：s = "egg", t = "add"
输出：true
```

**示例 2：**

```
输入：s = "foo", t = "bar"
输出：false
```

**示例 3：**

```
输入：s = "paper", t = "title"
输出：true
```

 

**提示：**

- `1 <= s.length <= 5 * 104`
- `t.length == s.length`
- `s` 和 `t` 由任意有效的 ASCII 字符组成

题解：可以先生成两个哈希链表，然后依次对比两个链表。

```
//官方给出的方法，简单但是不好理解
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> s2t;
        unordered_map<char, char> t2s;
        int len = s.length();
        for (int i = 0; i < len; ++i) {
            char x = s[i], y = t[i];
            if ((s2t.count(x) && s2t[x] != y) || (t2s.count(y) && t2s[y] != x)) {
                return false;
            }
            s2t[x] = y;
            t2s[y] = x;
        }
        return true;
    }
};
```

```
//方法2：先生成两个链表，然后判断两个链表长度是否相等，相等之后再判断两个链表
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> s2t;
        unordered_map<char, char> t2s;
        if (s.size() != t.size())
            return false;
        for (int i = 0; i < s.size(); i++)
        {
            char ss = s[i], tt = t[i];
            s2t[ss] = tt;
            t2s[tt] = ss;
        }
        cout << s2t.size()<<endl;
        if (s2t.size() != t2s.size())
            return false;
        for (int i = 0; i < s.size(); i++)
        {
            if ((s2t[s[i]] != t[i])||(t2s[t[i]] != s[i]))
                return false;
        }
        return true;
    }
};
```

```
#include<iostream>
#include<unordered_map>
using namespace std;
int main()
{
    Solution mySolution;
    cout<<mySolution.isIsomorphic("egge", "adda");
}
```



## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `t` 是否是 `s` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `s` 和 `t` 互为字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

 

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母

题解：只要将两个字符串排序好，然后进行比较就行。这里注意字符串的sort()用法。

```
#include<iostream>
#include<algorithm>
using namespace std;
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size())
            return false;
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());
        if (s == t)
            return true;
        else return false;

    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.isAnagram("rat", "car");
}
```



## [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例 2：**

```
输入：root = [1]
输出：["1"]
```

 

**提示：**

- 树中节点的数目在范围 `[1, 100]` 内
- `-100 <= Node.val <= 100`

题解：这里有“广度优先搜索”和“深度优先搜索”两种方式。暂时不会写整个程序



```
//广度优先搜索方法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> paths;
        if(root == nullptr)
            return paths;
        
        queue<TreeNode*> node_queue;
        queue<string> path_queue;

        node_queue.push(root);
        path_queue.push(to_string(root->val));

        while(!node_queue.empty())
        {
            TreeNode* node = node_queue.front();
            string path = path_queue.front();
            node_queue.pop();
            path_queue.pop();

            if(node->left == nullptr && node->right == nullptr)
            {
                paths.push_back(path);
            }
            else
            {
                if(node->left != nullptr)
                {
                    node_queue.push(node->left);
                    path_queue.push(path + "->"+ to_string(node->left->val));
                }
                if(node->right != nullptr)
                {
                    node_queue.push(node->right);
                    path_queue.push(path+"->"+to_string(node->right->val));
                }
            }
        }
        return paths;
    }
};
```



## [290. 单词规律](https://leetcode.cn/problems/word-pattern/)

给定一种规律 `pattern` 和一个字符串 `s` ，判断 `s` 是否遵循相同的规律。

这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `s` 中的每个非空单词之间存在着双向连接的对应规律。

**示例1:**

```
输入: pattern = "abba", s = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", s = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", s = "dog cat cat dog"
输出: false
```

 

**提示:**

- `1 <= pattern.length <= 300`
- `pattern` 只包含小写英文字母
- `1 <= s.length <= 3000`
- `s` 只包含小写英文字母和 `' '`
- `s` **不包含** 任何前导或尾随对空格
- `s` 中每个单词都被 **单个空格** 分隔

题解：用两个哈希列表进行判断

```
#include<iostream>
#include<unordered_map>

using namespace std;
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        unordered_map<char, string> pa2s;
        unordered_map<string, char> s2pa;
        vector<string> ss;
        int j = 0;
        //首先判断 pattern="he",s="unit"情况
        //再次判断pattern="a",s="a"情况
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == ' ')
                j++;
        }
        if ((j+1) != pattern.size())
            return false;
        j = 0;
        string str = "";
        
        for (int i = 0; i < s.size(); i++)
        {
            
            if (s[i] != ' ')
                str += s[i];
            else
            {
                s2pa[str] = pattern[j];
                pa2s[pattern[j]] = str;
                ss.push_back(str);
                ++j;
                str = "";
            }
            
            if ((i + 1) == s.size())
            {
                s2pa[str] = pattern[j];
                pa2s[pattern[j]] = str;
                ss.push_back(str);
            }
            
        }
        if (pa2s.size() != s2pa.size())
            return false;

        for (int i = 0; i < ss.size(); i++)
        {
            //cout << ss[i] << endl;
            if ((pa2s[pattern[i]] != ss[i]) || (s2pa[ss[i]] != pattern[i]))
                return false;
        }
        return true;
    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.wordPattern("abba", "dog cat cat dog");
}
```



## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

 

**示例 1：**

```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

 

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符

题解：直接使用reverse函数进行反转。

```
class Solution {
public:
    void reverseString(vector<char>& s) {
        return reverse(s.begin(),s.end());
    }
};
```



## [345. 反转字符串中的元音字母](https://leetcode.cn/problems/reverse-vowels-of-a-string/)

给你一个字符串 `s` ，仅反转字符串中的所有元音字母，并返回结果字符串。

元音字母包括 `'a'`、`'e'`、`'i'`、`'o'`、`'u'`，且可能以大小写两种形式出现不止一次。

**示例 1：**

```
输入：s = "hello"
输出："holle"
```

**示例 2：**

```
输入：s = "leetcode"
输出："leotcede"
```

**提示：**

- `1 <= s.length <= 3 * 105`
- `s` 由 **可打印的 ASCII** 字符组成

题解：这里的反转意思是，元音字符串为“abcd”，反转之后为“dcba”。

```
class Solution {
public:
    string reverseVowels(string s) {
        string str;
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u' ||
                s[i] == 'A' || s[i] == 'E' || s[i] == 'I' || s[i] == 'O' || s[i] == 'U')
            {
                str += s[i];
            }
        }

        string ss;
        int j = 0;
        reverse(str.begin(), str.end());
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u' ||
                s[i] == 'A' || s[i] == 'E' || s[i] == 'I' || s[i] == 'O' || s[i] == 'U')
            {
                ss += str[j];
                ++j;
            }
            else
                ss += s[i];

        }
        return ss;
    }
};
```



## [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

 

**提示：**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` 和 `magazine` 由小写英文字母组成

题解：建立两个哈希链表，<char,int>类型，然后int个数进行比较，当`ransomNote`  某个元素的个数小于`magazine `的元素个数时，返回false;

```
//思想正确，但是非常耗时间的方法
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int>ran;
        unordered_map<char, int>mag;
        
        if (ransomNote.size() > magazine.size())
            return false;
        //先统计两个字符串中，每个元素的个数    
        for (int i = 0; i < ransomNote.size(); i++)
        {
            int num = 1;
            for (int j = i + 1; j < ransomNote.size(); j++)
            {
                if (ransomNote[i] == ransomNote[j])
                    num++;
            }
            if (ran.count(ransomNote[i]) ==0)
                ran[ransomNote[i]] =num;
        }
        
        for (int i = 0; i < magazine.size(); i++)
        {
            int num = 1;
            for (int j = i + 1; j < magazine.size(); j++)
            {
                if (magazine[i] == magazine[j])
                    num++;
            }
            if (mag.count(magazine[i]) == 0)
                mag[magazine[i]] = num;

        }
 		//对两个元素中字符串个数进行比较，当不足时返回false;
        for (int i = 0; i < ransomNote.size(); i++)
        {

            if (ran[ransomNote[i]] > mag[ransomNote[i]])
                return false;
        }
        return true;
    }
};
```

```
//还是建立两个链表的思想，但是可以ran[ransomNote[i]] ++这种方式统计个数，减去了一层循环，还可以把两个字符串元素个数的比较使用迭代器进行比较，暂时不会。
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int>ran;
        unordered_map<char, int>mag;
        if (ransomNote.size() > magazine.size())
            return false;
        for (int i = 0; i < ransomNote.size(); i++)
        {
             ran[ransomNote[i]] ++;
        }
        
        for (int i = 0; i < magazine.size(); i++)
        {
            
             mag[magazine[i]] ++;

        }
 
        for (int i = 0; i < ransomNote.size(); i++)
        {

            if (ran[ransomNote[i]] > mag[ransomNote[i]])
                return false;
        }
        return true;
    }
};
```



## [387. 字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/)

给定一个字符串 `s` ，找到 *它的第一个不重复的字符，并返回它的索引* 。如果不存在，则返回 `-1` 。

**示例 1：**

```
输入: s = "leetcode"
输出: 0
```

**示例 2:**

```
输入: s = "loveleetcode"
输出: 2
```

**示例 3:**

```
输入: s = "aabb"
输出: -1
```

 

**提示:**

- `1 <= s.length <= 105`
- `s` 只包含小写字母



题解：首先建立一个<char,int>链表，统计每个元素的个数；然后循环s字符串，当出现个数为1的元素返回该元素的位置

```cpp
#include<iostream>
#include<unordered_map>
using namespace std;
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int>ss;
        for (int i = 0; i < s.size(); i++)
        {
            ss[s[i]]++;
        }
        
        for (int i = 0; i < s.size(); i++)
        {
            if (ss[s[i]] == 1)
                return i;
        }
        return -1;

    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.firstUniqChar("loveleetcode");
}
```



## [389. 找不同](https://leetcode.cn/problems/find-the-difference/)

给定两个字符串 `s` 和 `t` ，它们只包含小写字母。

字符串 `t` 由字符串 `s` 随机重排，然后在随机位置添加一个字母。

请找出在 `t` 中被添加的字母。

**示例 1：**

```
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```

**示例 2：**

```
输入：s = "", t = "y"
输出："y"
```

 

**提示：**

- `0 <= s.length <= 1000`
- `t.length == s.length + 1`
- `s` 和 `t` 只包含小写字母

题解：用两个链表统计个数，当其中一个的元素个数多1个时，就是添加的字母

```
class Solution {
public:
    char findTheDifference(string s, string t) {
        unordered_map<char, int>ss;
        unordered_map<char, int>tt;
        for (int i = 0; i < s.size(); i++)
        {
            ss[s[i]]++;
        }
        for (int i = 0; i < t.size(); i++)
        {
            tt[t[i]]++;
        }
        
        for (int i = 0; i < t.size(); i++)
        {
            if (ss[t[i]] < tt[t[i]])
                return t[i];
        }
        
        return 0;
    }
};
```



## [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？ 

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

**提示：**

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- 两个字符串都只由小写字符组成。

题解：判断字符串s是否为字符串t的子序列，这里找出字符串s第一个元素出现在字符串t中出现的位置x1，然后s中第二个元素在字符串t位置的x1之后出现的位置x2；依次这样确定，当出现的次数和s的长度相等时，就为子序列。

注意，break只是跳出内循环，外循环继续。

```
//自己写的，太啰嗦
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.size()>t.size())
            return false;
        int sss=0;
        int ttt=0;
        for(int i=0;i<s.size();i++)
        {
            for(int j=ttt;j<t.size();j++)
            {  
                ttt++;
                if(s[i] == t[j])
                {
                    sss++;
                    break;
                }
            }
        }
        if(sss == s.size())
        return true;
        else
        return false;
    }
};
```

```
//官方给的，简洁明了
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int n = s.length(), m = t.length();
        int i = 0, j = 0;
        while (i < n && j < m) {
            if (s[i] == t[j]) {
                i++;
            }
            j++;
        }
        return i == n;
    }
};

```



## [409. 最长回文串](https://leetcode.cn/problems/longest-palindrome/)

给定一个包含大写字母和小写字母的字符串 `s` ，返回 *通过这些字母构造成的 **最长的回文串*** 。

在构造过程中，请注意 **区分大小写** 。比如 `"Aa"` 不能当做一个回文字符串。

**示例 1:**

```
输入:s = "abccccdd"
输出:7
解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```

**示例 2:**

```
输入:s = "a"
输出:1
```

**示例 3：**

```
输入:s = "aaaaaccc"
输出:7
```

 

**提示:**

- `1 <= s.length <= 2000`
- `s` 只由小写 **和/或** 大写英文字母组成

题解：这里"ada"和“aa”两种都是回文串，所以字符串中某个元素个数为偶数个就全部是，奇数个数的-1个，最后判断一下是否有奇数的，总数再+1即可。

```
class Solution {
public:
    int longestPalindrome(string s) {
        if(s.size()==1)
            return 1;
        unordered_map<char,int>ss;
        for(int i=0;i<s.size();i++)
        {
            ss[s[i]]++;
        }
        int num=0;
        for(auto iter=ss.begin();iter != ss.end(); iter++)
        {
            if((iter->second)%2==0)
                num += iter->second;
            else if((iter->second)>2)
                num += (iter->second)-1;
        }

        for(auto iter=ss.begin();iter != ss.end(); iter++)
        {
            if((iter->second)%2==1)
            {
                num ++;
                break;
            }
        }
        return num;
    }
};
```



## [412. Fizz Buzz](https://leetcode.cn/problems/fizz-buzz/)

给你一个整数 `n` ，找出从 `1` 到 `n` 各个整数的 Fizz Buzz 表示，并用字符串数组 `answer`（**下标从 1 开始**）返回结果，其中：

- `answer[i] == "FizzBuzz"` 如果 `i` 同时是 `3` 和 `5` 的倍数。
- `answer[i] == "Fizz"` 如果 `i` 是 `3` 的倍数。
- `answer[i] == "Buzz"` 如果 `i` 是 `5` 的倍数。
- `answer[i] == i` （以字符串形式）如果上述条件全不满足。

 

**示例 1：**

```
输入：n = 3
输出：["1","2","Fizz"]
```

**示例 2：**

```
输入：n = 5
输出：["1","2","Fizz","4","Buzz"]
```

**示例 3：**

```
输入：n = 15
输出：["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]
```

 

**提示：**

- `1 <= n <= 104`

题解：注意int类型转换字符串类型to_string()函数的用法（#include<string>头文件）

```
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string>str;

        for(int i=1;i<=n;i++)
        {
            if((i%3==0)&&(i%5==0) )
                str.push_back("FizzBuzz");
            else if(i%3==0)
                str.push_back("Fizz");
            else if(i%5==0)
                str.push_back("Buzz");
            else
                str.push_back(to_string(i));
        }
        return str;
    }
};
```



## [415. 字符串相加](https://leetcode.cn/problems/add-strings/)

给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 `BigInteger`）， 也不能直接将输入的字符串转换为整数形式。

 

**示例 1：**

```
输入：num1 = "11", num2 = "123"
输出："134"
```

**示例 2：**

```
输入：num1 = "456", num2 = "77"
输出："533"
```

**示例 3：**

```
输入：num1 = "0", num2 = "0"
输出："0"
```

**提示：**

- `1 <= num1.length, num2.length <= 104`
- `num1` 和`num2` 都只包含数字 `0-9`
- `num1` 和`num2` 都不包含任何前导零

题解：这道题不难，但是不使用指针或者数组，直接使用int类型会超出范围。

```
//超出范围的做法，当num1="9333852702227987"; num2="85731737104263"时，该做法超出范围。
class Solution {
public:
    string addStrings(string num1, string num2) {
        long long int num11 = 0, num22 = 0;
        for (int i = num1.size()-1; i >=0; i--)
        {
            num11 += (num1[i] - '0') * pow(10, num1.size() - 1 - i);
        }
        
        for (int i = num2.size() - 1; i >= 0; i--)
        {
            num22 += (num2[i] - '0') * pow(10, num2.size() - 1 - i);
        }
        return to_string(num11+num22);
    }
};
```

```
//正确做法，先通过两个数组来存储字符串，然后进行处理
#include<iostream>
#include<string>
#include<vector>
using namespace std;
class Solution {
public:
    string addStrings(string num1, string num2) {
        vector<int>num11;
        vector<int>num22;
        
        for (int i = 0; i < num1.size(); i++)
        {
            num11.push_back( num1[i] - '0');
        }

        for (int i = 0; i < num2.size(); i++)
        {
            num22.push_back( num2[i] - '0');
        }
        string num = "";
        int ii = num1.size() - 1, jj = num2.size() - 1;
        int num_ = 0;
        while (ii >= 0 || jj >= 0)
        {
            if (ii >= 0 && jj >= 0)
            {
                if ((num1[ii] - '0' + num2[jj] - '0' + num_) > 9)
                {
                    num += to_string(num1[ii] - '0' + num2[jj] - '0' + num_ - 10);
                    num_ = 1;
                }
                else
                {
                    num += to_string(num1[ii] - '0' + num2[jj] - '0' + num_);
                    num_ = 0;
                }
                
            }
            else if (ii >= 0)
            {
                if ((num1[ii] - '0' + num_) > 9)
                {
                    num += to_string(num1[ii] - '0' + num_ - 10);
                    num_ = 1;
                }
                else
                {
                    num += to_string(num1[ii] - '0' + num_);
                    num_ = 0;
                }
            }
            else if (jj >= 0)
            {
                if ((num2[jj] - '0' + num_) > 9)
                {
                    num += to_string(num2[jj] - '0' + num_ - 10);
                    num_ = 1;
                }
                else
                {
                    num += to_string(num2[jj] - '0' + num_);
                    num_ = 0;
                }
            }
            ii--;
            jj--;
        }
        cout << num << endl;
        if (num_ == 1)
            num += "1";
        reverse(num.begin(), num.end());
        return num;
            
    }
};
int main()
{
    Solution mySolution;
    cout << mySolution.addStrings("11", "123");
}
```



## [434. 字符串中的单词数](https://leetcode.cn/problems/number-of-segments-in-a-string/)

统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。

请注意，你可以假定字符串里不包括任何不可打印的字符。

**示例:**

```
输入: "Hello, my name is John"
输出: 5
解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。
```

题解：这里的i==0是用来判断“”、“   ”这两种情况的，首先判断这是不是第一个字符，如果是只要不是空字符，就num+1;（特例“  ”）

```
class Solution {
public:
    int countSegments(string s) {
        int num=0;
        for(int i=0;i<s.size();i++)
        {
            if(((i ==0) || (s[i-1]==' ')) && (s[i]!=' '))
                num++;
        }
        return num;
    }
};
```



## [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

**示例 1:**

```
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

**示例 2:**

```
输入: s = "aba"
输出: false
```

**示例 3:**

```
输入: s = "abcabcabcabc"
输出: true
解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成。)
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 由小写英文字母组成

题解：（两种特例，特例1- “aa”，“aaa”）,这里可以分为s.size()个，这里还需要改进。

```
#include<iostream>
using namespace std;
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        
        //这里判断i=2,3...n
        for (int i = 2; i < s.size()-1 ; i++) //这里可以分为i份
        {
            if (s.size() % i != 0)
            {
                continue;
            }
            int n = s.size() / i;
            int flag = 0;
            int flag_break = 0;  //为了跳出两层循环的
            for (int j = 1; j <= i; j++)
            {
                if (flag_break == 1)
                {
                    break;
                }
                for (int t = 0; t < n; t++)
                {
                    if (s[t] != s[t + n*j])
                    {
                        flag_break = 1;
                        break;                 
                    }
                    flag++;
                }
            }
            
            if (flag == n*(i-1))
                return true;
        }
        if (s.size() == 1)
            return false;
        int flag = 0;
        for (int i = 0; i < s.size(); i++)
        {
            if (s[0] != s[i])
            {
                break;
            }
            flag++;
        }
        if (flag == s.size())
            return true;

        return false;

    }
};
int main()
{
    Solution mySolution;
    cout<<mySolution.repeatedSubstringPattern("aaa");
}
```



## [482. 密钥格式化](https://leetcode.cn/problems/license-key-formatting/)



给定一个许可密钥字符串 `s`，仅由字母、数字字符和破折号组成。字符串由 `n` 个破折号分成 `n + 1` 组。你也会得到一个整数 `k` 。

我们想要重新格式化字符串 `s`，使每一组包含 `k` 个字符，除了第一组，它可以比 `k` 短，但仍然必须包含至少一个字符。此外，两组之间必须插入破折号，并且应该将所有小写字母转换为大写字母。

返回 *重新格式化的许可密钥* 。

 

**示例 1：**

```
输入：S = "5F3Z-2e-9-w", k = 4
输出："5F3Z-2E9W"
解释：字符串 S 被分成了两个部分，每部分 4 个字符；
     注意，两个额外的破折号需要删掉。
```

**示例 2：**

```
输入：S = "2-5g-3-J", k = 2
输出："2-5G-3J"
解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。
```

 

**提示:**

- `1 <= s.length <= 105`
- `s` 只包含字母、数字和破折号 `'-'`.
- `1 <= k <= 104`

题解：注意四大转换格式

（1）大写字母转小写字母

（2）小写字母转大写字母

（3）数字转字符串

（4）字符串转数字



先把字符串提取且为大写的str；如果是整数倍的直接后面添加“-”符号，否则，先把多出来的前一部分写进去，后面的继续按照整数倍进行处理

```
class Solution {
public:
    string licenseKeyFormatting(string s, int k) {
        int num=0;
        string ss;
        for(int i=0;i<s.size();i++)
        {
            if(s[i] != '-')
            {
                num++;
                if(s[i]>='a' && s[i]<='z')
                    ss += char(s[i]+'A'-'a');
                else
                    ss +=s[i];
            }
                
        }
        string str;
        if(ss.size() % k ==0)
        {
            for(int i=0;i<ss.size();i++)
            {
                if(((i+1)%k == 0) &&((i+1) != ss.size()))
                {
                    str += ss[i];
                    str+="-";
                }
                else
                    str +=ss[i];
            }
        }
        else
        {
            int num=ss.size()%k;

            for(int i=0;i<num;i++)
            {
                str += ss[i];
            }
            for(int i=num;i<ss.size();i++)
            {
                if(((i-num)%k==0)&&((i-num) != ss.size()))
                {
                    
                    str += "-";
                    str += ss[i];
                }
                else
                    str +=ss[i];
            }
        }
        return str;

    }
};
```



## [500. 键盘行](https://leetcode.cn/problems/keyboard-row/)

给你一个字符串数组 `words` ，只返回可以使用在 **美式键盘** 同一行的字母打印出来的单词。键盘如下图所示。

**美式键盘** 中：

- 第一行由字符 `"qwertyuiop"` 组成。
- 第二行由字符 `"asdfghjkl"` 组成。
- 第三行由字符 `"zxcvbnm"` 组成。

![American keyboard](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/keyboard.png)

 

**示例 1：**

```
输入：words = ["Hello","Alaska","Dad","Peace"]
输出：["Alaska","Dad"]
```

**示例 2：**

```
输入：words = ["omk"]
输出：[]
```

**示例 3：**

```
输入：words = ["adsdf","sfd"]
输出：["adsdf","sfd"]
```

 

**提示：**

- `1 <= words.length <= 20`
- `1 <= words[i].length <= 100`
- `words[i]` 由英文字母（小写和大写字母）组成

题解：这里可以判断每个单词的第一个字母和后面的字母是否为同一行。本身需要判断的字母如果少可以用if语句进行判断，但是键盘的每一行都比较长，这里可以使用字符串进行对比。

```
#include <iostream>
#include<vector>
using namespace std;
class Solution {
public:
    bool QQ(char ch)
    {
        string Q = "qwertyuiopQWERTYUIOP";
        for (int i = 0; i < Q.size(); i++)
            if (ch == Q[i])
                return true;
        return false;
    }
    bool AA(char ch)
    {
        string A = "asdfghjklASDFGHJKL";
        for (int i = 0; i < A.size(); i++)
            if (ch == A[i])
                return true;
        return false;
    }
    bool ZZ(char ch)
    {
        string Z = "zxcvbnmZXCVBNM";
        for (int i = 0; i < Z.size(); i++)
            if (ch == Z[i])
                return true;
        return false;
    }
    bool abc(string word)
    {
        if (QQ(word[0]) == 1)
        {
            for (int i = 1; i < word.size(); i++)
                if (QQ(word[i]) == 0)
                    return false;
        }
        else if (AA(word[0]) == 1)
        {
            for (int i = 1; i < word.size(); i++)
                if (AA(word[i]) == 0)
                    return false;
        }
        else if (ZZ(word[0]) == 1)
        {
            for (int i = 1; i < word.size(); i++)
                if (ZZ(word[i]) == 0)
                    return false;
        }
        return true;

    }
    vector<string> findWords(vector<string>& words) {
        vector<string>str;

        
        for (int i = 0; i < words.size(); i++)
        {
            int flag = 0;
            if (abc(words[i]))
            {
                str.push_back(words[i]);
                
            }
        }
        return str;

    }
};
int main()
{
    vector<string>str;
    str.push_back("Hello");
    str.push_back("Alaska");
    str.push_back("Dad");
    str.push_back("Peace");
    Solution mySolution;
    cout<<mySolution.findWords(str)[1];
}
```

## [520. 检测大写字母](https://leetcode.cn/problems/detect-capital/)

我们定义，在以下情况时，单词的大写用法是正确的：

- 全部字母都是大写，比如 `"USA"` 。
- 单词中所有字母都不是大写，比如 `"leetcode"` 。
- 如果单词不只含有一个字母，只有首字母大写， 比如 `"Google"` 。

给你一个字符串 `word` 。如果大写用法正确，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：word = "USA"
输出：true
```

**示例 2：**

```
输入：word = "FlaG"
输出：false
```

 

**提示：**

- `1 <= word.length <= 100`
- `word` 由小写和大写英文字母组成

题解：如果第一个字母为小写，后面也必须为小写； 如果第一个字母为大写，后面两种情况，要么为大写，要么为小写

```
class Solution {
public:
    bool detectCapitalUse(string word) {
        for (int i = 0; i < word.size(); i++)
        {
            if (word[0] >= 'a' && word[0] <= 'z')
            {
                if (word[i] < 'a' || word[i] > 'z')
                    return false;
            }
            else
            {
                if (word[1] >= 'a' && word[1] <= 'z')
                {
                    if (i > 0)
                        if (word[i] < 'a' || word[i] > 'z')
                            return false;
                }
                else if (word[1] >= 'A' && word[1] <= 'Z')
                {
                    if (i > 0)
                        if (word[i] < 'A' || word[i] > 'Z')
                            return false;
                }
            }
        }
        return true;
    }
};
```



## [521. 最长特殊序列 Ⅰ](https://leetcode.cn/problems/longest-uncommon-subsequence-i/)

给你两个字符串 `a` 和 `b`，请返回 *这两个字符串中 **最长的特殊序列*** 的长度。如果不存在，则返回 `-1` 。

**「最长特殊序列」** 定义如下：该序列为 **某字符串独有的最长子序列（即不能是其他字符串的子序列）** 。

字符串 `s` 的子序列是在从 `s` 中删除任意数量的字符后可以获得的字符串。

- 例如，`"abc"` 是 `"aebdc"` 的子序列，因为删除 `"a***e***b***d\***c"` 中斜体加粗的字符可以得到 `"abc"` 。 `"aebdc"` 的子序列还包括 `"aebdc"` 、 `"aeb"` 和 `""` (空字符串)。

 

**示例 1：**

```
输入: a = "aba", b = "cdc"
输出: 3
解释: 最长特殊序列可为 "aba" (或 "cdc")，两者均为自身的子序列且不是对方的子序列。
```

**示例 2：**

```
输入：a = "aaa", b = "bbb"
输出：3
解释: 最长特殊序列是 "aaa" 和 "bbb" 。
```

**示例 3：**

```
输入：a = "aaa", b = "aaa"
输出：-1
解释: 字符串 a 的每个子序列也是字符串 b 的每个子序列。同样，字符串 b 的每个子序列也是字符串 a 的子序列。
```

 

**提示：**

- `1 <= a.length, b.length <= 100`
- `a` 和 `b` 由小写英文字母组成

```
class Solution {
public:
    int findLUSlength(string a, string b) {
        if(a != b)
        {
            return max(a.size(),b.size());
        }
        return -1;

    }
};
```



## [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

 

**示例 1：**

```
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

**示例 2：**

```
输入：s = "abcd", k = 2
输出："bacd"
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由小写英文组成
- `1 <= k <= 104`

题解：s=“abcd”,k=3时，结果为“cbad”。

```
class Solution {
public:
    string reverseStr(string s, int k) {

        for (int i = 0; i < (s.size() - s.size() % (2 * k)); i += (2 * k))
        {

            reverse(s.begin() + i, s.begin() + i+ k);
        }
        cout << s << endl;
        if (( s.size() % (2 * k)) >=  k)
            reverse(s.end() - (s.size() % (2 * k)), s.end()- (s.size() % (2 * k)) + k);
        else if ((s.size() % (2 * k)) > 0)
            reverse(s.end() - (s.size() % (2 * k)), s.end());

        return s;
    }
};
```



## [551. 学生出勤记录 I](https://leetcode.cn/problems/student-attendance-record-i/)

给你一个字符串 `s` 表示一个学生的出勤记录，其中的每个字符用来标记当天的出勤情况（缺勤、迟到、到场）。记录中只含下面三种字符：

- `'A'`：Absent，缺勤
- `'L'`：Late，迟到
- `'P'`：Present，到场

如果学生能够 **同时** 满足下面两个条件，则可以获得出勤奖励：

- 按 **总出勤** 计，学生缺勤（`'A'`）**严格** 少于两天。
- 学生 **不会** 存在 **连续** 3 天或 **连续** 3 天以上的迟到（`'L'`）记录。

如果学生可以获得出勤奖励，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：s = "PPALLP"
输出：true
解释：学生缺勤次数少于 2 次，且不存在 3 天或以上的连续迟到记录。
```

**示例 2：**

```
输入：s = "PPALLL"
输出：false
解释：学生最后三天连续迟到，所以不满足出勤奖励的条件。
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s[i]` 为 `'A'`、`'L'` 或 `'P'`



```
class Solution {
public:
    bool checkRecord(string s) {
        int AA=0;
        int LL=0;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]=='A')
                AA++;
            if(AA>=2)
                return false;
            if(s[i] == 'L')
                LL++;
        }
        if(LL<3)
            return true;
        else
        {
            for(int i=0;i<s.size()-2;i++)
            {
                if(s[i]=='L' && s[i+1]=='L' &&s[i+2]=='L')
                    return false;
            }
        }
        
        
        return true;

    }
};
```



## [557. 反转字符串中的单词 III](https://leetcode.cn/problems/reverse-words-in-a-string-iii/)

给定一个字符串 `s` ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

**示例 1：**

```
输入：s = "Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```

**示例 2:**

```
输入： s = "God Ding"
输出："doG gniD"
```

 

***\**\*\*\*提示：\*\*\*\*\****

- `1 <= s.length <= 5 * 104`
- `s` 包含可打印的 **ASCII** 字符。
- `s` 不包含任何开头或结尾空格。
- `s` 里 **至少** 有一个词。
- `s` 中的所有单词都用一个空格隔开。

题解：注意提取一个字符串某段的内容substr函数的应用。

```
class Solution {
public:
    string reverseWords(string s) {
        string str;
        string ss;
        for(int i=0;i<s.size();i++)
        {
            if(s[i] != ' ')
                str += s[i];
            else
            {
                reverse(str.begin(),str.end());
                ss += str +" ";
                str = "";
            }
        }

        reverse(str.begin(),str.end());
        ss += str;
        return ss.substr(0,ss.size());

    }
};
```



## [606. 根据二叉树创建字符串](https://leetcode.cn/problems/construct-string-from-binary-tree/)

给你二叉树的根节点 `root` ，请你采用前序遍历的方式，将二叉树转化为一个由括号和整数组成的字符串，返回构造出的字符串。

空节点使用一对空括号对 `"()"` 表示，转化后需要省略所有不影响字符串与原始二叉树之间的一对一映射关系的空括号对。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/05/03/cons1-tree.jpg)

```
输入：root = [1,2,3,4]
输出："1(2(4))(3)"
解释：初步转化后得到 "1(2(4)())(3()())" ，但省略所有不必要的空括号对后，字符串应该是"1(2(4))(3)" 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/05/03/cons2-tree.jpg)

```
输入：root = [1,2,3,null,4]
输出："1(2()(4))(3)"
解释：和第一个示例类似，但是无法省略第一个空括号对，否则会破坏输入与输出一一映射的关系。
```

 

**提示：**

- 树中节点的数目范围是 `[1, 104]`
- `-1000 <= Node.val <= 1000`

题解：

```
// 方法1：递归方法
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    string tree2str(TreeNode* root) {
        if(root == nullptr)
            return "";
        
        if(root->left == nullptr && root->right == nullptr)
            return to_string(root->val);
        
        if(root->right == nullptr)
            return to_string(root->val) +"("+ tree2str(root->left) +")";
        
        return to_string(root->val)+"("+tree2str(root->left)+")("+tree2str(root->right)+")";

    }
};
```



## [657. 机器人能否返回原点](https://leetcode.cn/problems/robot-return-to-origin/)

在二维平面上，有一个机器人从原点 `(0, 0)` 开始。给出它的移动顺序，判断这个机器人在完成移动后是否在 **`(0, 0)` 处结束**。

移动顺序由字符串 `moves` 表示。字符 `move[i]` 表示其第 `i` 次移动。机器人的有效动作有 `R`（右），`L`（左），`U`（上）和 `D`（下）。

如果机器人在完成所有动作后返回原点，则返回 `true`。否则，返回 `false`。

**注意：**机器人“面朝”的方向无关紧要。 `“R”` 将始终使机器人向右移动一次，`“L”` 将始终向左移动等。此外，假设每次移动机器人的移动幅度相同。

 

**示例 1:**

```
输入: moves = "UD"
输出: true
解释：机器人向上移动一次，然后向下移动一次。所有动作都具有相同的幅度，因此它最终回到它开始的原点。因此，我们返回 true。
```

**示例 2:**

```
输入: moves = "LL"
输出: false
解释：机器人向左移动两次。它最终位于原点的左侧，距原点有两次 “移动” 的距离。我们返回 false，因为它在移动结束时没有返回原点。
```

 

**提示:**

- `1 <= moves.length <= 2 * 104`
- `moves` 只包含字符 `'U'`, `'D'`, `'L'` 和 `'R'`

题解：这里想要移动之后还能返回原点，则需要有几次向上移动，就有几次向下移动，左右移动次数相等，上下移动次数相等。所以总移动次数肯定为偶数；然后判断上下，左右移动的次数是否相等。

```
class Solution {
public:
    bool judgeCircle(string moves) {
        if(moves.size()%2 != 0)
            return false;
        int UU=0,DD=0,RR=0,LL=0;
        for(int i=0;i<moves.size();i++)
        {
            if(moves[i]=='U')
                UU++;
            else if(moves[i]=='D')
                DD++;
            else if(moves[i]=='R')
                RR++;
            else if(moves[i]=='L')
                LL++;
        }
        if((UU != DD) || (RR != LL))
            return false;
            
        return true;

    }
};
```



## [680. 验证回文串 II](https://leetcode.cn/problems/valid-palindrome-ii/)

给你一个字符串 `s`，**最多** 可以从中删除一个字符。

请你判断 `s` 是否能成为回文字符串：如果能，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：s = "aba"
输出：true
```

**示例 2：**

```
输入：s = "abca"
输出：true
解释：你可以删除字符 'c' 。
```

**示例 3：**

```
输入：s = "abc"
输出：false
```

 

**提示：**

- `1 <= s.length <= 105`
- `s` 由小写英文字母组成

题解：方法一：这种方法会超出时间限制，只是作为一个参考，依次删除字符串的字母，然后判断剩下的字符串是否是回文。

方法二：

```
class Solution {
public:
    bool abc(string s)
    {
        int left = 0, right = s.size() - 1;
        while (left < right)
        {
            if (s[left] != s[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
    bool validPalindrome(string s) {

        for (int i = 0; i < s.size(); i++)
        {
            string str = s;
            str.erase(i, 1);
            if (abc(str))
                return true;

        }
        return false;

    }
};
```

```
//正确做法
class Solution {
public:
    bool checkPalindrome(string s,int left,int right)
    {
        while (left < right)
        {
            if (s[left] != s[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
    bool validPalindrome(string s) {

        int left = 0, right = s.size() - 1;
        int flag = 1;
        while (left < right)
        {
            if (s[left] == s[right])
            {
                left++;
                right--;
            }
            else
            {
                return checkPalindrome(s, left+1, right) || checkPalindrome(s, left, right - 1);
            }
            
        }
        return true;

    }
};
```

## [709. 转换成小写字母](https://leetcode.cn/problems/to-lower-case/)

给你一个字符串 `s` ，将该字符串中的大写字母转换成相同的小写字母，返回新的字符串。

**示例 1：**

```
输入：s = "Hello"
输出："hello"
```

**示例 2：**

```
输入：s = "here"
输出："here"
```

**示例 3：**

```
输入：s = "LOVELY"
输出："lovely"
```

**提示：**

- `1 <= s.length <= 100`
- `s` 由 ASCII 字符集中的可打印字符组成

```
class Solution {
public:
    string toLowerCase(string s) {
        string str;
        for(int i=0;i<s.size();i++)
        {
            if( s[i]>='A' && s[i]<='Z')
                str += char(s[i]+'a'-'A');
            else
                str +=s[i];
        }
        return str;
    }
};
```

## [771. 宝石与石头](https://leetcode.cn/problems/jewels-and-stones/)

 给你一个字符串 `jewels` 代表石头中宝石的类型，另有一个字符串 `stones` 代表你拥有的石头。 `stones` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

字母区分大小写，因此 `"a"` 和 `"A"` 是不同类型的石头。

**示例 1：**

```
输入：jewels = "aA", stones = "aAAbbbb"
输出：3
```

**示例 2：**

```
输入：jewels = "z", stones = "ZZ"
输出：0
```

**提示：**

- `1 <= jewels.length, stones.length <= 50`
- `jewels` 和 `stones` 仅由英文字母组成
- `jewels` 中的所有字符都是 **唯一的**

题解：这里有两种方式，第一种采用暴力法进行搜索；第二种方式为

```
//方法一：采用暴力法进行遍历
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        int num=0;
        for(int i=0;i<jewels.size();i++)
        {
            for(int j=0;j<stones.size();j++)
            {
                if(jewels[i] == stones[j])
                num++;
            }
        }
        return num;

    }
};
```

```
//方法二：该方法中使用unordered_set进行处理
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        int jewelsCount = 0;
        unordered_set<char> jewelsSet;
        int jewelsLength = jewels.length(), stonesLength = stones.length();
        for (int i = 0; i < jewelsLength; i++) {
            char jewel = jewels[i];
            jewelsSet.insert(jewel);
        }
        for (int i = 0; i < stonesLength; i++) {
            char stone = stones[i];
            if (jewelsSet.count(stone)) {
                jewelsCount++;
            }
        }
        return jewelsCount;
    }
};
```

## [796. 旋转字符串](https://leetcode.cn/problems/rotate-string/)

给定两个字符串, `s` 和 `goal`。如果在若干次旋转操作之后，`s` 能变成 `goal` ，那么返回 `true` 。

`s` 的 **旋转操作** 就是将 `s` 最左边的字符移动到最右边。 

- 例如, 若 `s = 'abcde'`，在旋转一次之后结果就是`'bcdea'` 。

 

**示例 1:**

```
输入: s = "abcde", goal = "cdeab"
输出: true
```

**示例 2:**

```
输入: s = "abcde", goal = "abced"
输出: false
```

**提示:**

- `1 <= s.length, goal.length <= 100`
- `s` 和 `goal` 由小写英文字母组成

题解：其实每次都是将字符串的第一个字母搬移到最后面的位置。

```
class Solution {
public:
    bool rotateString(string s, string goal) {
        if (s.size() != goal.size())
            return false;
        string str=s;
        for (int i = 0; i < s.size(); i++)
        {
            
            char ch1 = str[0];
            str = str.substr(1, str.size());
            str += ch1;
            cout << str << endl;
            if (str == goal)
                return true;
        }
        return false;
    }
};
```



## [804. 唯一摩尔斯密码词](https://leetcode.cn/problems/unique-morse-code-words/)

国际摩尔斯密码定义一种标准编码方式，将每个字母对应于一个由一系列点和短线组成的字符串， 比如:

- `'a'` 对应 `".-"` ，
- `'b'` 对应 `"-..."` ，
- `'c'` 对应 `"-.-."` ，以此类推。

为了方便，所有 `26` 个英文字母的摩尔斯密码表如下：

```
[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]
```

给你一个字符串数组 `words` ，每个单词可以写成每个字母对应摩尔斯密码的组合。

- 例如，`"cab"` 可以写成 `"-.-..--..."` ，(即 `"-.-."` + `".-"` + `"-..."` 字符串的结合)。我们将这样一个连接过程称作 **单词翻译** 。

对 `words` 中所有单词进行单词翻译，返回不同 **单词翻译** 的数量。

 

**示例 1：**

```
输入: words = ["gin", "zen", "gig", "msg"]
输出: 2
解释: 
各单词翻译如下:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."

共有 2 种不同翻译, "--...-." 和 "--...--.".
```

**示例 2：**

```
输入：words = ["a"]
输出：1
```

 

**提示：**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 12`
- `words[i]` 由小写英文字母组成

题解：需要理解char类型转为int类型的大小，然后使用无序链表进行统计就可以。一个unordered_map个数也可以通过size()函数统计出来。

```
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        string aa[]={".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};
        unordered_map<string,int>str;
        for(int i=0;i<words.size();i++)
        {
            string ss;
            for(int j=0;j<words[i].size();j++)
            {
                ss += aa[int(words[i][j]-'a')];
            }
            str[ss]++;
        }
        return str.size();

    }
};
```

## [806. 写字符串需要的行数](https://leetcode.cn/problems/number-of-lines-to-write-string/)

我们要把给定的字符串 `S` 从左到右写到每一行上，每一行的最大宽度为100个单位，如果我们在写某个字母的时候会使这行超过了100 个单位，那么我们应该把这个字母写到下一行。我们给定了一个数组 `widths` ，这个数组 widths[0] 代表 'a' 需要的单位， widths[1] 代表 'b' 需要的单位，...， widths[25] 代表 'z' 需要的单位。

现在回答两个问题：至少多少行能放下`S`，以及最后一行使用的宽度是多少个单位？将你的答案作为长度为2的整数列表返回。

```
示例 1:
输入: 
widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10]
S = "abcdefghijklmnopqrstuvwxyz"
输出: [3, 60]
解释: 
所有的字符拥有相同的占用单位10。所以书写所有的26个字母，
我们需要2个整行和占用60个单位的一行。
示例 2:
输入: 
widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10]
S = "bbbcccdddaaa"
输出: [2, 4]
解释: 
除去字母'a'所有的字符都是相同的单位10，并且字符串 "bbbcccdddaa" 将会覆盖 9 * 10 + 2 * 4 = 98 个单位.
最后一个字母 'a' 将会被写到第二行，因为第一行只剩下2个单位了。
所以，这个答案是2行，第二行有4个单位宽度。
```

 

**注:**

- 字符串 `S` 的长度在 [1, 1000] 的范围。
- `S` 只包含小写字母。
- `widths` 是长度为 `26`的数组。
- `widths[i]` 值的范围在 `[2, 10]`。

题解：注意这里要考虑其中的单次宽度不能够横跨两行。其中ceil函数为向上取整，比如ceil(1.3)=2;

```
class Solution {
public:
    vector<int> numberOfLines(vector<int>& widths, string s) {
        vector<int> num;
        int sum = 0;
        int hang=0;
        for (int i = 0; i < s.size(); i++)
        {
            sum += widths[int(s[i] - 'a')];
            if(sum >100)
            {
                hang++;
                i--;
                sum=0;
            }
        }
        if(sum>0)
            hang++;
        num.push_back(hang);
        num.push_back(sum);
        return num;

    }
};
```



## [819. 最常见的单词](https://leetcode.cn/problems/most-common-word/)

给定一个段落 (paragraph) 和一个禁用单词列表 (banned)。返回出现次数最多，同时不在禁用列表中的单词。

题目保证至少有一个词不在禁用列表中，而且答案唯一。

禁用列表中的单词用小写字母表示，不含标点符号。段落中的单词不区分大小写。答案都是小写字母。

 

**示例：**

```
输入: 
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
输出: "ball"
解释: 
"hit" 出现了3次，但它是一个禁用的单词。
"ball" 出现了2次 (同时没有其他单词出现2次)，所以它是段落里出现次数最多的，且不在禁用列表中的单词。 
注意，所有这些单词在段落里不区分大小写，标点符号需要忽略（即使是紧挨着单词也忽略， 比如 "ball,"）， 
"hit"不是最终的答案，虽然它出现次数更多，但它在禁用单词列表中。
```

 

**提示：**

- `1 <= 段落长度 <= 1000`
- `0 <= 禁用单词个数 <= 100`
- `1 <= 禁用单词长度 <= 10`
- 答案是唯一的, 且都是小写字母 (即使在 `paragraph` 里是大写的，即使是一些特定的名词，答案都是小写的。)
- `paragraph` 只包含字母、空格和下列标点符号`!?',;.`
- 不存在没有连字符或者带有连字符的单词。
- 单词里只包含字母，不会出现省略号或者其他标点符号。

题解：

```
class Solution {
public:
    string mostCommonWord(string paragraph, vector<string>& banned) {

        
        paragraph += " ";
        unordered_map<string, int>PaNum;
        string str;
        for (int i = 0; i < paragraph.size(); i++)
        {
            if (paragraph[i] >= 'A' && paragraph[i] <= 'Z')
                str += char(paragraph[i] + 'a' - 'A');
            else if (paragraph[i] >= 'a' && paragraph[i] <= 'z')
                str += paragraph[i];
            else if(str != "")
            {
                //cout << str << endl;
                if(!count(banned.begin(),banned.end(),str))
                    PaNum[str]++;
                str = "";
            }
        }
        int max_par = -1;
        string ss;
        for (auto item = PaNum.begin(); item != PaNum.end(); item++)
        {
            //cout << item->first << " " << item->second << endl;
            if (item->second > max_par)
            {
                max_par = item->second;
                ss = item->first;
            }
        }
        return ss;

    }
};
```



## [821. 字符的最短距离](https://leetcode.cn/problems/shortest-distance-to-a-character/)

给你一个字符串 `s` 和一个字符 `c` ，且 `c` 是 `s` 中出现过的字符。

返回一个整数数组 `answer` ，其中 `answer.length == s.length` 且 `answer[i]` 是 `s` 中从下标 `i` 到离它 **最近** 的字符 `c` 的 **距离** 。

两个下标 `i` 和 `j` 之间的 **距离** 为 `abs(i - j)` ，其中 `abs` 是绝对值函数。

 

**示例 1：**

```
输入：s = "loveleetcode", c = "e"
输出：[3,2,1,0,1,0,0,1,2,2,1,0]
解释：字符 'e' 出现在下标 3、5、6 和 11 处（下标从 0 开始计数）。
距下标 0 最近的 'e' 出现在下标 3 ，所以距离为 abs(0 - 3) = 3 。
距下标 1 最近的 'e' 出现在下标 3 ，所以距离为 abs(1 - 3) = 2 。
对于下标 4 ，出现在下标 3 和下标 5 处的 'e' 都离它最近，但距离是一样的 abs(4 - 3) == abs(4 - 5) = 1 。
距下标 8 最近的 'e' 出现在下标 6 ，所以距离为 abs(8 - 6) = 2 。
```

**示例 2：**

```
输入：s = "aaab", c = "b"
输出：[3,2,1,0]
```

 

**提示：**

- `1 <= s.length <= 104`
- `s[i]` 和 `c` 均为小写英文字母
- 题目数据保证 `c` 在 `s` 中至少出现一次

题解：采用两遍遍历的方式进行寻找

```
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        vector<int>ss;
        for(int i=0;i<s.size();i++)
        {
            int minNum=s.size();
            int num=0;
            for(int j=0;j<s.size();j++)
            {
                if(s[j]==c)
                {
                    num=abs(j-i);
                    if(num < minNum)
                        minNum=num;
                }
                if(minNum==0)
                    break;
                
            }
            ss.push_back(minNum);
        }
        return ss;

    }
};
```



## [824. 山羊拉丁文](https://leetcode.cn/problems/goat-latin/)



给你一个由若干单词组成的句子 `sentence` ，单词间由空格分隔。每个单词仅由大写和小写英文字母组成。

请你将句子转换为 *“*山羊拉丁文（*Goat Latin*）*”*（一种类似于 猪拉丁文 - Pig Latin 的虚构语言）。山羊拉丁文的规则如下：

- 如果单词以元音开头（

  ```
  'a'
  ```

  ,

   

  ```
  'e'
  ```

  ,

   

  ```
  'i'
  ```

  ,

   

  ```
  'o'
  ```

  ,

   

  ```
  'u'
  ```

  ），在单词后添加

  ```
  "ma"
  ```

  。

  - 例如，单词 `"apple"` 变为 `"applema"` 。

- 如果单词以辅音字母开头（即，非元音字母），移除第一个字符并将它放到末尾，之后再添加

  ```
  "ma"
  ```

  。

  - 例如，单词 `"goat"` 变为 `"oatgma"` 。

- 根据单词在句子中的索引，在单词最后添加与索引相同数量的字母

  ```
  'a'
  ```

  ，索引从

   

  ```
  1
  ```

   

  开始。

  - 例如，在第一个单词后添加 `"a"` ，在第二个单词后添加 `"aa"` ，以此类推。

返回将 `sentence` 转换为山羊拉丁文后的句子。

 

**示例 1：**

```
输入：sentence = "I speak Goat Latin"
输出："Imaa peaksmaaa oatGmaaaa atinLmaaaaa"
```

**示例 2：**

```
输入：sentence = "The quick brown fox jumped over the lazy dog"
输出："heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
```

 

**提示：**

- `1 <= sentence.length <= 150`
- `sentence` 由英文字母和空格组成
- `sentence` 不含前导或尾随空格
- `sentence` 中的所有单词由单个空格分隔

题解：

```
class Solution {
public:
    string toGoatLatin(string sentence) {
        string str;
        string ss;
        int num = 0;
        for (int i = 0; i <= sentence.size(); i++)
        {
            if ((i != sentence.size() ) && sentence[i] != ' ')
                str += sentence[i];
            else
            {
                //cout << str << endl;
                num++;
                if (str[0] == 'a' || str[0] == 'e' || str[0] == 'i' || str[0] == 'o' || str[0] == 'u')
                {
                    str += "ma";

                }
                else if (str[0] == 'A' || str[0] == 'E' || str[0] == 'I' || str[0] == 'O' || str[0] == 'U')
                {
                    str += "ma";
                }
                else
                {
                    str += str[0] ;
                    str += +"ma";
                    str = str.erase(0, 1);
                }
                for (int j = 0; j < num; j++)
                    str += 'a';
                //cout << str << endl;
                //cout << str << endl;
                ss += str +" ";
                //cout << ss << endl;
                
                str = "";
            }



        }
        ss = ss.erase(ss.size()-1,1);
        return ss;

    }
};
```

## [830. 较大分组的位置](https://leetcode.cn/problems/positions-of-large-groups/)

在一个由小写字母构成的字符串 `s` 中，包含由一些连续的相同字符所构成的分组。

例如，在字符串 `s = "abbxxxxzyy"` 中，就含有 `"a"`, `"bb"`, `"xxxx"`, `"z"` 和 `"yy"` 这样的一些分组。

分组可以用区间 `[start, end]` 表示，其中 `start` 和 `end` 分别表示该分组的起始和终止位置的下标。上例中的 `"xxxx"` 分组用区间表示为 `[3,6]` 。

我们称所有包含大于或等于三个连续字符的分组为 **较大分组** 。

找到每一个 **较大分组** 的区间，**按起始位置下标递增顺序排序后**，返回结果。

 

**示例 1：**

```
输入：s = "abbxxxxzzy"
输出：[[3,6]]
解释："xxxx" 是一个起始于 3 且终止于 6 的较大分组。
```

**示例 2：**

```
输入：s = "abc"
输出：[]
解释："a","b" 和 "c" 均不是符合要求的较大分组。
```

**示例 3：**

```
输入：s = "abcdddeeeeaabbbcd"
输出：[[3,5],[6,9],[12,14]]
解释：较大分组为 "ddd", "eeee" 和 "bbb"
```

**示例 4：**

```
输入：s = "aba"
输出：[]
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅含小写英文字母

题解：需要知道二维vector动态数组是怎么存储数据的。首先需要str.push_back(vector<int>());然后str[i].push_back(i - num);

```
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        s += " ";
        vector<vector<int>>str;
        int num = 0;
        int nn = 0;
        int flag = 0;
        for (int i = 0; i < s.size(); i++)
        {
            //cout << s[i];
            if (s[i] == s[i + 1])
            {
                flag = 1;
                num++;
            }
            else
                flag = 0;
            if (flag == 0 && num >= 2)
            {
                str.push_back(vector<int>());
                str[nn].push_back(i - num);
                str[nn].push_back(i);
                nn++;
            }
            if (!flag)
                num = 0;

        }
        return str;

    }
};
```



## [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

 

**示例 1：**

```
输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
```

**示例 2：**

```
输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
```

**示例 3：**

```
输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。
```

 

**提示：**

- `1 <= s.length, t.length <= 200`
- `s` 和 `t` 只含有小写字母以及字符 `'#'`

题解：这里在删除了’#‘字符之后，还需要判断字符串是否还有内容，有内容就删除一个。

```
class Solution {
public:
    bool backspaceCompare(string s, string t) {

        vector<char>ss;
        vector<char>tt;
        for (int i = 0; i < s.size(); i++)
        {
            ss.push_back(s[i]);
            if (s[i] == '#')
            {
                ss.pop_back();
                if (ss.size() > 0)
                ss.pop_back();
            }
                
        }
        for (int i = 0; i < t.size(); i++)
        {
            tt.push_back(t[i]);
            if (t[i] == '#')
            {
                tt.pop_back();
                if(tt.size()>0)
                tt.pop_back();
            }
                
        }
        if (ss.size() == tt.size())
        {
            for (int i = 0; i < ss.size(); i++)
            {
                if (ss[i] != tt[i])
                    return false;
            }
        }
        else
            return false;
            

        return true;

    }
};
```



## [859. 亲密字符串](https://leetcode.cn/problems/buddy-strings/)

给你两个字符串 `s` 和 `goal` ，只要我们可以通过交换 `s` 中的两个字母得到与 `goal` 相等的结果，就返回 `true` ；否则返回 `false` 。

交换字母的定义是：取两个下标 `i` 和 `j` （下标从 `0` 开始）且满足 `i != j` ，接着交换 `s[i]` 和 `s[j]` 处的字符。

- 例如，在 `"abcd"` 中交换下标 `0` 和下标 `2` 的元素可以生成 `"cbad"` 。

 

**示例 1：**

```
输入：s = "ab", goal = "ba"
输出：true
解释：你可以交换 s[0] = 'a' 和 s[1] = 'b' 生成 "ba"，此时 s 和 goal 相等。
```

**示例 2：**

```
输入：s = "ab", goal = "ab"
输出：false
解释：你只能交换 s[0] = 'a' 和 s[1] = 'b' 生成 "ba"，此时 s 和 goal 不相等。
```

**示例 3：**

```
输入：s = "aa", goal = "aa"
输出：true
解释：你可以交换 s[0] = 'a' 和 s[1] = 'a' 生成 "aa"，此时 s 和 goal 相等。
```

 

**提示：**

- `1 <= s.length, goal.length <= 2 * 104`
- `s` 和 `goal` 由小写英文字母组成

题解：首先，两个字符串的长度应该是相同的，统计两个字符串中不同的字符及个数，如果不同的字符个数为2，且ss[0]==gg[1]&&ss[1]==gg[0]；如果不同的字符个数为0时，分为两种情况，（1）s=“ab”,	goal="ab"，此时无交换，返回false;（2）s="aa",goal="aa"（或某个字母只要出现次数大于两次即可），此时，s[0]和s[1]进行交换就可以，返回true；

```
class Solution {
public:
    bool buddyStrings(string s, string goal) {
        if(s.size() != goal.size())
            return false;
        
        string ss;
        string gg;
        unordered_map<char,int>str;
        for(int i=0;i<s.size();i++)
        {
            if(s[i] != goal[i])
            {
                ss += s[i];
                gg += goal[i];
            }
            str[s[i]]++;
        }
        // cout<<ss.size();
        if(ss.size() ==2)
        {
            if(ss[0]==gg[1] && ss[1]==gg[0])
                return true;
        }
        else if(ss.size()==0)
        {
            for(int i=0;i<s.size();i++)
            if(str[s[i]] >1)
                return true;
        }
            
        return false;


    }
};
```



## [884. 两句话中的不常见单词](https://leetcode.cn/problems/uncommon-words-from-two-sentences/)

**句子** 是一串由空格分隔的单词。每个 **单词** 仅由小写字母组成。

如果某个单词在其中一个句子中恰好出现一次，在另一个句子中却 **没有出现** ，那么这个单词就是 **不常见的** 。

给你两个 **句子** `s1` 和 `s2` ，返回所有 **不常用单词** 的列表。返回列表中单词可以按 **任意顺序** 组织。

 

**示例 1：**

```
输入：s1 = "this apple is sweet", s2 = "this apple is sour"
输出：["sweet","sour"]
```

**示例 2：**

```
输入：s1 = "apple apple", s2 = "banana"
输出：["banana"]
```

**提示：**

- `1 <= s1.length, s2.length <= 200`
- `s1` 和 `s2` 由小写英文字母和空格组成
- `s1` 和 `s2` 都不含前导或尾随空格
- `s1` 和 `s2` 中的所有单词间均由单个空格分隔

题解：使用一个map对每个单次出现的次数统计，当出现次数为1时，就为“不常见单次”

```
class Solution {
public:
    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int>str;
        string ss;
        for (int i = 0; i <= s1.size(); i++)
        {
            if (i != s1.size() && s1[i] != ' ')
                ss += s1[i];
            else
            {
                str[ss]++;
                ss = "";
            }
        }
        for (int i = 0; i <= s2.size(); i++)
        {
            if (i != s2.size() && s2[i] != ' ')
                ss += s2[i];
            else
            {
                str[ss]++;
                ss = "";
            }
        }
        
        vector<string>words;
        for (auto item = str.begin(); item != str.end(); item++)
        {
            if (item->second == 1)
            {
                words.push_back(item->first);
            }
                
        }
        return words;

    }
};
```



## [917. 仅仅反转字母](https://leetcode.cn/problems/reverse-only-letters/)

提示



简单



197





相关企业

给你一个字符串 `s` ，根据下述规则反转字符串：

- 所有非英文字母保留在原有位置。
- 所有英文字母（小写或大写）位置反转。

返回反转后的 `s` *。*

 



**示例 1：**

```
输入：s = "ab-cd"
输出："dc-ba"
```



**示例 2：**

```
输入：s = "a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"
```



**示例 3：**

```
输入：s = "Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"
```

 

**提示**

- `1 <= s.length <= 100`
- `s` 仅由 ASCII 值在范围 `[33, 122]` 的字符组成
- `s` 不含 `'\"'` 或 `'\\'`

题解：先把字符串提取出来，然后使用reverse函数对字符串进行反转，之后根据特殊字符的位置进行添加。

```
class Solution {
public:
    string reverseOnlyLetters(string s) {
        string str;
        for (int i = 0; i < s.size(); i++)
        {
            if ((s[i] >= 'a' && s[i] <= 'z') || (s[i] >= 'A' && s[i] <= 'Z'))
                str += s[i];
        }
        reverse(str.begin(), str.end());
        cout << str << endl;
        string ss;
        int num = 0;
        for (int i = 0; i < s.size(); i++)
        {
            if ((s[i] < 'a' || s[i]>'z') && (s[i] < 'A' || s[i]>'Z'))
            {
                ss += s[i];
            }
            else
            {
                ss += str[num];
                num++;
            }
        }
        return ss;

    }
};
```



## [929. 独特的电子邮件地址](https://leetcode.cn/problems/unique-email-addresses/)



每个 **有效电子邮件地址** 都由一个 **本地名** 和一个 **域名** 组成，以 `'@'` 符号分隔。除小写字母之外，电子邮件地址还可以含有一个或多个 `'.'` 或 `'+'` 。

- 例如，在 `alice@leetcode.com`中， `alice` 是 **本地名** ，而 `leetcode.com` 是 **域名** 。

如果在电子邮件地址的 **本地名** 部分中的某些字符之间添加句点（`'.'`），则发往那里的邮件将会转发到本地名中没有点的同一地址。请注意，此规则 **不适用于域名** 。

- 例如，`"alice.z@leetcode.com”` 和 `“alicez@leetcode.com”` 会转发到同一电子邮件地址。

如果在 **本地名** 中添加加号（`'+'`），则会忽略第一个加号后面的所有内容。这允许过滤某些电子邮件。同样，此规则 **不适用于域名** 。

- 例如 `m.y+name@email.com` 将转发到 `my@email.com`。

可以同时使用这两个规则。

给你一个字符串数组 `emails`，我们会向每个 `emails[i]` 发送一封电子邮件。返回实际收到邮件的不同地址数目。

 

**示例 1：**

```
输入：emails = ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
输出：2
解释：实际收到邮件的是 "testemail@leetcode.com" 和 "testemail@lee.tcode.com"。
```

**示例 2：**

```
输入：emails = ["a@leetcode.com","b@leetcode.com","c@leetcode.com"]
输出：3
```


**提示：**

- `1 <= emails.length <= 100`
- `1 <= emails[i].length <= 100`
- `emails[i]` 由小写英文字母、`'+'`、`'.'` 和 `'@'` 组成
- 每个 `emails[i]` 都包含有且仅有一个 `'@'` 字符
- 所有本地名和域名都不为空
- 本地名不会以 `'+'` 字符作为开头

题解：要了解unordered_set<string> 的特性，str.emplace是添加元素的方式。

```
//这个版本的code特别麻烦，有更简单的。
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {

        unordered_map<string, int>ema;


        for (int i = 0; i < emails.size(); i++)
        {
            string str;
            int flag = 0;
            int flag1 = 0;
            for (int j = 0; j < emails[i].size(); j++)
            {
                if (emails[i][j] == '@')
                {
                    flag = j;
                }

                if (j >= flag && flag != 0)
                    str += emails[i][j];
                else
                {
                    if (emails[i][j] == '+')
                        flag1 = j;

                    if ((j >= flag1 && flag1 != 0))
                    {

                    }
                    else if (emails[i][j] == '.')
                    {

                    }
                    else
                        str += emails[i][j];

                }
            }
            ema[str]++;

        }
        return ema.size();

    }
    };
```

```
//官方给出的更简便的方法
class Solution {
public:
    int numUniqueEmails(vector<string> &emails) {
        unordered_set<string> emailSet;
        for (auto &email: emails) {
            string local;
            for (char c: email) {
                if (c == '+' || c == '@') {
                    break;
                }
                if (c != '.') {
                    local += c;
                }
            }
            emailSet.emplace(local + email.substr(email.find('@')));
        }
        return emailSet.size();
    }
};
```

## [942. 增减字符串匹配](https://leetcode.cn/problems/di-string-match/)



由范围 `[0,n]` 内所有整数组成的 `n + 1` 个整数的排列序列可以表示为长度为 `n` 的字符串 `s` ，其中:

- 如果 `perm[i] < perm[i + 1]` ，那么 `s[i] == 'I'` 
- 如果 `perm[i] > perm[i + 1]` ，那么 `s[i] == 'D'` 

给定一个字符串 `s` ，重构排列 `perm` 并返回它。如果有多个有效排列perm，则返回其中 **任何一个** 。

 

**示例 1：**

```
输入：s = "IDID"
输出：[0,4,1,3,2]
```

**示例 2：**

```
输入：s = "III"
输出：[0,1,2,3]
```

**示例 3：**

```
输入：s = "DDI"
输出：[3,2,0,1]
```

**提示：**

- `1 <= s.length <= 105`
- `s` 只包含字符 `"I"` 或 `"D"`

题解：注意，对一个矩阵按列读取时，应该先是有多少列，再有多少行。

```
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int num = 0;
        for (int i = 0; i < strs[0].size(); i++)
        {
            int sum = 0;
            for (int j = 0; j < strs.size(); j++)
            {
                if (sum > int(strs[j][i] - 'a'))
                {
                    num++;
                    break;
                }
                else
                    sum = int(strs[j][i] - 'a');
            }
            cout << endl;
        }
        return num;

    }
};
```



## [1021. 删除最外层的括号](https://leetcode.cn/problems/remove-outermost-parentheses/)

有效括号字符串为空 `""`、`"(" + A + ")"` 或 `A + B` ，其中 `A` 和 `B` 都是有效的括号字符串，`+` 代表字符串的连接。

- 例如，`""`，`"()"`，`"(())()"` 和 `"(()(()))"` 都是有效的括号字符串。

如果有效字符串 `s` 非空，且不存在将其拆分为 `s = A + B` 的方法，我们称其为**原语（primitive）**，其中 `A` 和 `B` 都是非空有效括号字符串。

给出一个非空有效字符串 `s`，考虑将其进行原语化分解，使得：`s = P_1 + P_2 + ... + P_k`，其中 `P_i` 是有效括号字符串原语。

对 `s` 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 `s` 。

 

**示例 1：**

```
输入：s = "(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。
```

**示例 2：**

```
输入：s = "(()())(())(()(()))"
输出："()()()()(())"
解释：
输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，
删除每个部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。
```

**示例 3：**

```
输入：s = "()()"
输出：""
解释：
输入字符串为 "()()"，原语化分解得到 "()" + "()"，
删除每个部分中的最外层括号后得到 "" + "" = ""。
```

 

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 为 `'('` 或 `')'`
- `s` 是一个有效括号字符串

题解：官方给出使用栈的方式更为方便，可以参考。

```
//官方code
class Solution {
public:
    string removeOuterParentheses(string s) {
        string res;
        stack<char> st;
        for (auto c : s) {
            if (c == ')') {
                st.pop();
            }
            if (!st.empty()) {
                res.push_back(c);
            }
            if (c == '(') {
                st.emplace(c);
            }
        }
        return res;
    }
};

```

## [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)



给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

 

**示例：**

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```

 

**提示：**

1. `1 <= S.length <= 20000`
2. `S` 仅由小写英文字母组成。

题解：这里有两种方式，但是第一种方式就是不断遍历，但是会超出时间限制；第二种方式就是可以采用栈的方式，不断删除，最后就是剩余的字符串。

```
//方法一：该方法不断遍历，但是会超出时间限制。
class Solution {
public:
    string removeDuplicates(string s) {
        string ss=s;
        string str;
        string str1=s;
        while (ss != str)
        {
            int i = 1;
            ss = str1;
            int flag = 0;
            while (i <= ss.size())
            {
                
                if ((ss[i] != ss[i - 1]))
                {
                    if (flag == 0)
                        str = "";
                    str += ss[i - 1];
                    flag++;
                    //i++;
                }
                else
                    i++;
                
                i++;
            }
            if (flag == 0)
                str = "";
            // cout << str << endl;
            str1 = str;

        }
        return str;

    }
};
```

```
//方法二：官方给出的方法，通过栈的方式进行处理。
class Solution {
public:
    string removeDuplicates(string s) {
        string stk;
        for (char ch : s) {
            if (!stk.empty() && stk.back() == ch) {
                stk.pop_back();
            } else {
                stk.push_back(ch);
            }
        }
        return stk;
    }
};
```



## [1078. Bigram 分词](https://leetcode.cn/problems/occurrences-after-bigram/)

提示



简单





78





相关企业

给出第一个词 `first` 和第二个词 `second`，考虑在某些文本 `text` 中可能以 `"first second third"` 形式出现的情况，其中 `second` 紧随 `first` 出现，`third` 紧随 `second` 出现。

对于每种这样的情况，将第三个词 "`third`" 添加到答案中，并返回答案。

 

**示例 1：**

```
输入：text = "alice is a good girl she is a good student", first = "a", second = "good"
输出：["girl","student"]
```

**示例 2：**

```
输入：text = "we will we will rock you", first = "we", second = "will"
输出：["we","rock"]
```

 

**提示：**

- `1 <= text.length <= 1000`
- `text` 由小写英文字母和空格组成
- `text` 中的所有单词之间都由 **单个空格字符** 分隔
- `1 <= first.length, second.length <= 10`
- `first` 和 `second` 由小写英文字母组成

题解：首先提取每个单词放到数组当中，然后对数组单词进行比对。这里不存在这种bug（比如text = "alice is a good girl she is a good", first = "a", second = "good"，不存在text最后一个单词不存在的情况）。

```
class Solution {
public:
    vector<string> findOcurrences(string text, string first, string second) {
        vector<string>str;
        vector<string>ss;
        string word;
        for(int i=0;i<=text.size();i++)
        {
            if((i != text.size()) && text[i] !=' ')
                word+=text[i];
            else
            {
                ss.push_back(word);
                word="";
            }
                
        }
        for(int i=0;i<ss.size()-2;i++)
        {
            if(ss[i]==first && ss[i+1]==second)
                str.push_back(ss[i+2]);
        }
        return str;

    }
};
```



## [1108. IP 地址无效化](https://leetcode.cn/problems/defanging-an-ip-address/)



给你一个有效的 [IPv4](https://baike.baidu.com/item/IPv4) 地址 `address`，返回这个 IP 地址的无效化版本。

所谓无效化 IP 地址，其实就是用 `"[.]"` 代替了每个 `"."`。

 

**示例 1：**

```
输入：address = "1.1.1.1"
输出："1[.]1[.]1[.]1"
```

**示例 2：**

```
输入：address = "255.100.50.0"
输出："255[.]100[.]50[.]0"
```

 

**提示：**

- 给出的 `address` 是一个有效的 IPv4 地址

```
class Solution {
public:
    string defangIPaddr(string address) {
        string str;
        for(int i=0;i<address.size();i++)
        {
            if(address[i]=='.')
                str +="[.]";
            else
                str+=address[i];
        }
        return str;

    }
};
```

## [1436. 旅行终点站](https://leetcode.cn/problems/destination-city/)



给你一份旅游线路图，该线路图中的旅行线路用数组 `paths` 表示，其中 `paths[i] = [cityAi, cityBi]` 表示该线路将会从 `cityAi` 直接前往 `cityBi` 。请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市*。*

题目数据保证线路图会形成一条不存在循环的线路，因此恰有一个旅行终点站。

 

**示例 1：**

```
输入：paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
输出："Sao Paulo" 
解释：从 "London" 出发，最后抵达终点站 "Sao Paulo" 。本次旅行的路线是 "London" -> "New York" -> "Lima" -> "Sao Paulo" 。
```

**示例 2：**

```
输入：paths = [["B","C"],["D","B"],["C","A"]]
输出："A"
解释：所有可能的线路是：
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
显然，旅行终点站是 "A" 。
```

**示例 3：**

```
输入：paths = [["A","Z"]]
输出："Z"
```

 

**提示：**

- `1 <= paths.length <= 100`
- `paths[i].length == 2`
- `1 <= cityAi.length, cityBi.length <= 10`
- `cityAi != cityBi`
- 所有字符串均由大小写英文字母和空格字符组成。

题解：这里不能理解为二维数组的最后一组的最后一个城市就是需要返回的，例如[["pYyNGfBYbm","wxAscRuzOl"],["kzwEQHfwce","pYyNGfBYbm"]]，这里最后的城市应该是"wxAscRuzOl"。

官方给出的思路：这里返回的地址应该为cityBi不存在cityAi中的城市名。

```
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        unordered_set<string>cityA;
        unordered_set<string>cityB;
        for(int i=0;i<paths.size();i++)
        {
            cityA.emplace(paths[i][0]);
            cityB.emplace(paths[i][1]);
        }
        for(auto item=cityB.begin();item!=cityB.end();item++)
        {
            if( !cityA.count(*item) )
                return *item;
        }
        return "";

    }
};
```



## [599. 两个列表的最小索引总和](https://leetcode.cn/problems/minimum-index-sum-of-two-lists/)

假设 Andy 和 Doris 想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设答案总是存在。

 

**示例 1:**

```
输入: list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例 2:**

```
输入:list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

**提示:**

- `1 <= list1.length, list2.length <= 1000`
- `1 <= list1[i].length, list2[i].length <= 30` 
- `list1[i]` 和 `list2[i]` 由空格 `' '` 和英文字母组成。
- `list1` 的所有字符串都是 **唯一** 的。
- `list2` 中的所有字符串都是 **唯一** 的。

题解：这里vector.clear()函数是非常的妙。

```
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_set<string>l1;
        for(int i=0;i<list1.size();i++)
            l1.emplace(list1[i]);

        vector<string>str;
        int dis=list1.size()+list2.size();
        for(int i=0;i<list2.size();i++)
        {
            
            if(l1.count(list2[i]))
            {
                for(int j=0;j<list1.size();j++)
                if(list1[j]==list2[i])
                {
                    if((i+j)<dis)
                    {
                        str.clear();
                        str.push_back(list2[i]);
                        dis=i+j;
                    }
                    else if((i+j) == dis)
                        str.push_back(list2[i]);
                    break;
                }
                
            }
        }
        return str;

        
    }
};
```

官方给出的方法，这里直接把list1的字符串和位置都存入unordered_map当中，这样就不用二次循环寻找了。

```
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string, int> index;
        for (int i = 0; i < list1.size(); i++) {
            index[list1[i]] = i;
        }

        vector<string> ret;
        int indexSum = INT_MAX;
        for (int i = 0; i < list2.size(); i++) {
            if (index.count(list2[i]) > 0) {
                int j = index[list2[i]];
                if (i + j < indexSum) {
                    ret.clear();
                    ret.push_back(list2[i]);
                    indexSum = i + j;
                } else if (i + j == indexSum) {
                    ret.push_back(list2[i]);
                }
            }
        }
        return ret;
    }
};
```



## [1002. 查找共用字符](https://leetcode.cn/problems/find-common-characters/)

给你一个字符串数组 `words` ，请你找出所有在 `words` 的每个字符串中都出现的共用字符（ **包括重复字符**），并以数组形式返回。你可以按 **任意顺序** 返回答案。

 

**示例 1：**

```
输入：words = ["bella","label","roller"]
输出：["e","l","l"]
```

**示例 2：**

```
输入：words = ["cool","lock","cook"]
输出：["c","o"]
```

 

**提示：**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 由小写英文字母组成

题解：还没想明白遇到的问题该怎么解决。



## [1154. 一年中的第几天](https://leetcode.cn/problems/day-of-the-year/)

给你一个字符串 `date` ，按 `YYYY-MM-DD` 格式表示一个 [现行公元纪年法](https://baike.baidu.com/item/公元/17855) 日期。返回该日期是当年的第几天。

 

**示例 1：**

```
输入：date = "2019-01-09"
输出：9
解释：给定日期是2019年的第九天。
```

**示例 2：**

```
输入：date = "2019-02-10"
输出：41
```

 

**提示：**

- `date.length == 10`
- `date[4] == date[7] == '-'`，其他的 `date[i]` 都是数字
- `date` 表示的范围从 1900 年 1 月 1 日至 2019 年 12 月 31 日

题解：闰年的判断方法，如果year%400==0,则是闰年，否则需要判断year%4==0 && year%100 !=0。

```
class Solution {
public:
    int dayOfYear(string date) {
        string str;
        int year = 0, month = 0, day = 0;
        int num = 0;
        string ss;
        for (int i = 0; i < 4; i++)
        {
            year += pow(10, (3 - i)) * (int(date[i] - '0'));
        }

        for (int i = 5; i < 7; i++)
        {
            month += pow(10, (6 - i)) * (int(date[i] - '0'));
        }

        for (int i = 8; i < 10; i++)
        {
            day += pow(10, (9 - i)) * (int(date[i] - '0'));
        }

        if (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0)) //闰年
        {
            num = day;
            switch (month)
            {
            case 1:num; break;
            case 2:num = num + 31; break;
            case 3:num = num + 31 + 29; break;
            case 4:num = num + 31 + 29 + 31; break;
            case 5:num = num + 31 + 29 + 31 + 30; break;
            case 6:num = num + 31 + 29 + 31 + 30 + 31; break;
            case 7:num = num + 31 + 29 + 31 + 30 + 31 + 30; break;
            case 8:num = num + 31 + 29 + 31 + 30 + 31 + 30 + 31; break;
            case 9:num = num + 31 + 29 + 31 + 30 + 31 + 30 + 31 + 31; break;
            case 10:num = num + 31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30; break;
            case 11:num = num + 31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31; break;
            case 12:num = num + 31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + 30; break;
            }

        }
        else
        {
            num = day;
            switch (month)
            {
            case 1:num; break;
            case 2:num = num + 31; break;
            case 3:num = num + 31 + 28; break;
            case 4:num = num + 31 + 28 + 31; break;
            case 5:num = num + 31 + 28 + 31 + 30; break;
            case 6:num = num + 31 + 28 + 31 + 30 + 31; break;
            case 7:num = num + 31 + 28 + 31 + 30 + 31 + 30; break;
            case 8:num = num + 31 + 28 + 31 + 30 + 31 + 30 + 31; break;
            case 9:num = num + 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31; break;
            case 10:num = num + 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30; break;
            case 11:num = num + 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31; break;
            case 12:num = num + 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + 30; break;
            }
        }
        cout << endl;
        return num;
    }
};
```

官方方法：1、字符串转数字的函数，2、数组更改内容的方法。

```
class Solution {
public:
    int dayOfYear(string date) {
        int year = stoi(date.substr(0, 4));
        int month = stoi(date.substr(5, 2));
        int day = stoi(date.substr(8, 2));

        int amount[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        if (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0)) {
            ++amount[1];
        }

        int ans = 0;
        for (int i = 0; i < month - 1; ++i) {
            ans += amount[i];
        }
        return ans + day;
    }
};
```



# 二、数组

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)



给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                if(nums[i]+nums[j] == target)
                    return {i,j};
            }
        }
        return {};
    }
};
```



## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

题解：

还是使用双指针，这里容器能容纳的水量取决于两个木板之间最短的，那就计算最短木板和距离之间的乘积。

```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int low=0,h=height.size()-1;
        int hh=h;
        int area=min(height[low],height[h]) * (hh);
        while(low<h)
        {
            if(height[low] < height[h])
                low++;
            else
                h--;
            hh--;
            if(min(height[low],height[h])*(hh)  > area)
                area=min(height[low],height[h])*(hh);
        }
        return area;
    }
};
```





## [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 `2` 写做 `II` ，即为两个并列的 1 。`12` 写做 `XII` ，即为 `X` + `II` 。 `27` 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。

 

**示例 1:**

```
输入: s = "III"
输出: 3
```

**示例 2:**

```
输入: s = "IV"
输出: 4
```

**示例 3:**

```
输入: s = "IX"
输出: 9
```

**示例 4:**

```
输入: s = "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: s = "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 

**提示：**

- `1 <= s.length <= 15`
- `s` 仅含字符 `('I', 'V', 'X', 'L', 'C', 'D', 'M')`
- 题目数据保证 `s` 是一个有效的罗马数字，且表示整数在范围 `[1, 3999]` 内
- 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
- IL 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
- 关于罗马数字的详尽书写规则，可以参考 [罗马数字 - Mathematics ](https://b2b.partcommunity.com/community/knowledge/zh_CN/detail/10753/罗马数字#knowledge_article)。

题解：这里需要用到哈希表

```
class Solution {
    private:
    unordered_map<char,int> symbolValues={
        {'I',1},
        {'V',5},
        {'X',10},
        {'L',50},
        {'C',100},
        {'D',500},
        {'M',1000},
    };
public:
    int romanToInt(string s) {
        int ans=0;
        int n=s.length();
        for(int i=0;i<n;i++)
        {
            int values=symbolValues[s[i]];
            if(i<n-1 && values<symbolValues[s[i+1]])
            {
                ans -=values;
            }
            else
            {
                ans +=values;
            }
        }
        return ans;
    }
};
```



## [12. 整数转罗马数字](https://leetcode.cn/problems/integer-to-roman/)

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给你一个整数，将其转为罗马数字。

 

**示例 1:**

```
输入: num = 3
输出: "III"
```

**示例 2:**

```
输入: num = 4
输出: "IV"
```

**示例 3:**

```
输入: num = 9
输出: "IX"
```

**示例 4:**

```
输入: num = 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

**示例 5:**

```
输入: num = 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 

**提示：**

- `1 <= num <= 3999`

题解：

（1）官方解法，更加简洁，但是不要考虑map和unordered_map键值对这种方法，这里是没法用的（）

```
const pair<int, string> valueSymbols[] = {
    {1000, "M"},
    {900,  "CM"},
    {500,  "D"},
    {400,  "CD"},
    {100,  "C"},
    {90,   "XC"},
    {50,   "L"},
    {40,   "XL"},
    {10,   "X"},
    {9,    "IX"},
    {5,    "V"},
    {4,    "IV"},
    {1,    "I"},
};

class Solution {
public:
    string intToRoman(int num) {
        string roman;
        for (const auto &[value, symbol] : valueSymbols) {
            while (num >= value) {
                num -= value;
                roman += symbol;
            }
            if (num == 0) {
                break;
            }
        }
        return roman;
    }
};
```

当然，也可以用下面这种写法

```
pair<int,string>ss[] = {
        {1000,"M"},
        {900,"CM"},
        {500,"D"},
        {400,"CD"},
        {100,"C"},
        {90,"XC"},
        {50,"L"},
        {40,"XL"},
        {10,"X"},
        {9,"IX"},
        {5,"V"},
        {4,"IV"},
        {1,"I"},
    };
class Solution {
public:
    string intToRoman(int num) {
        string s;
        for(auto p:ss)
        {
            int value=p.first;
            while(num >= value)
            {
                num -= value;
                s += p.second;
            }
            if(num == 0)
                break;
        }
        return s;
    }
};
```

（2）采用枚举的方式进行计算。

```
class Solution {
public:

string GetStr(int& num)
{
    if(num>=1000)
    {
        num -= 1000;
        return "M";
    }
    else if(num>=900)
    {
        num -= 900;
        return "CM";
    }
    else if(num>=500)
    {
        num -= 500;
        return "D";
    }
    else if(num>=400)
    {
        num -= 400;
        return "CD";
    }
    else if(num >=100)
    {
        num -= 100;
        return "C";
    }
    else if(num>=90)
    {
        num -= 90;
        return "XC";
    }
    else if(num>=50)
    {
        num -= 50;
        return "L";
    }
    else if(num >=40)
    {
        num-= 40;
        return "XL";
    }
    else if(num>=10)
    {
        num -= 10;
        return "X";
    }
    else if(num>=9)
    {
        num -= 9;
        return "IX";
    }
    else if(num>=5)
    {
        num -= 5;
        return "V";
    }
    else if(num>=4)
    {
        num -= 4;
        return "IV";
    }
    else
    {
        num-=1;
        return "I";
    }
}
    string intToRoman(int num) {
        string res;
        while(num>0)
        {
            res += GetStr(num);
        }
        return res;
    }
};
```



## [15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

题解：首先要注意二维数组的开辟。（暂时没有思路）





## [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)



给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **升序** 排列

题解：没搞懂这道题让返回什么结果。重新理解，这里不需要返回nums这个数组。只需要返回这个数组有多少个数。

（1）方法一：使用erase()函数进行处理。这里包含unordered_ser当中count()函数的用法。

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        unordered_set<int>nn;
        int count=0;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nn.count(nums[i]))
                nums.erase(nums.begin()+i,nums.begin()+i+1);
            else
            {
                nn.emplace(nums[i]);
                count++;
            }
        }
        return count;
    }
};
```

（2）方法二：使用双指针进行处理。

待续



## [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按升序排列

题解：注意这里题目给出的nums数组是升序的。这里使用快慢指针进行处理。

注意这里刚开始时，判断nums.size()<2是非常有必要的，要是数组大小小于2，code后面的部分就是错误的。

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) {
            return n;
        }
        int slow = 2, fast = 2;
        while (fast < n) {
            if (nums[slow - 2] != nums[fast]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
};
```



## [27. 移除元素](https://leetcode.cn/problems/remove-element/)



给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

题解：

方法一：使用vector中的erase()函数。

注意erase()函数在string字符串、vector、set当中的区别用法

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int count=0;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(val == nums[i])
                nums.erase(nums.begin()+i,nums.begin()+i+1);
            else
                count++;
        }
        return count;
    }
};
```

方法二：快慢指针

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        int left = 0;
        for (int right = 0; right < n; right++) {
            if (nums[right] != val) {
                nums[left] = nums[right];
                left++;
            }
        }
        return left;
    }
};
```



## [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)



给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-104 <= target <= 104`

题解：两次遍历，第一次遍历时搜索有没有target该目标，没有时进行第二次搜索，这里注意，需要单独判断第一个数和最后一个数和target的大小。

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        for (int i = 0; i < nums.size(); i++)
        {
            if (nums[i] == target)
                return i;
        }

        for (int i = 0; i < nums.size(); i++)
        {
            // cout << nums[i] << " " << nums[i + 1] << endl;
            if (nums[0] > target)
                return 0;
            else if (nums[nums.size() - 1] < target)
            {
                return nums.size();
            }
            else if (nums[i] < target && nums[i + 1] > target)
                return i+1;
        }
        return 0;

    }
};
```



## [66. 加一](https://leetcode.cn/problems/plus-one/)



给定一个由 **整数** 组成的 **非空** 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储**单个**数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

 

**示例 1：**

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

**示例 2：**

```
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
```

**示例 3：**

```
输入：digits = [0]
输出：[1]
```

 

**提示：**

- `1 <= digits.length <= 100`
- `0 <= digits[i] <= 9`

题解：

```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {

        reverse(digits.begin(),digits.end());
        vector<int>dd;
        int flag=0;
        for(int i=0;i<digits.size();i++)
        {
            if(i == 0)
            {
                dd.push_back((digits[i]+1)%10);
                if(digits[i]+1 >9)
                    flag=1;
            }
            else
            {
                dd.push_back((digits[i]+flag)%10);
                if(digits[i]+flag > 9)
                    flag=1;
                else
                    flag=0;
            }
            
        }
        if(flag == 1)
        dd.push_back(1);
        reverse(dd.begin(),dd.end());
        return dd;

    }
};
```



## [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)



给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

 

**提示：**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`



## [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)



给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

 

**示例 1:**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**示例 2:**

```
输入: numRows = 1
输出: [[1]]
```

 

**提示:**

- `1 <= numRows <= 30`

题解：了解Vector二维数组的开辟形式和存储数值的方式。

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>>nums(numRows);
        //先初始化一个vector二维数组
        for (int i = 0; i < numRows; i++)
        {
            nums[i].resize(i + 1);
        }
        if (numRows == 1)
        {
            nums[0][0] = 1;
            return nums;
        }
        

        for (int i = 0; i < numRows; i++)
        {
            for (int j = 0; j < i+1 ; j++)
            {
                if (i < 2)
                    nums[i][j]=1;
                else
                {
                    if (j == 0 || j == i)
                    {
                        nums[i][j] = 1;
                    }
                    else
                    {
                        nums[i][j] = nums[i - 1][j - 1] + nums[i - 1][j];
                    }
                }
            }


        }
        // for (int i = 0; i < numRows; i++)
        // {
        //     for (int j = 0; j < nums[i].size(); j++)
        //         cout << nums[i][j] << " ";
        //     cout << endl;
        // }
            
        return nums;

    }
};
```



## [119. 杨辉三角 II](https://leetcode.cn/problems/pascals-triangle-ii/)

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![img](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

 

**示例 1:**

```
输入: rowIndex = 3
输出: [1,3,3,1]
```

**示例 2:**

```
输入: rowIndex = 0
输出: [1]
```

**示例 3:**

```
输入: rowIndex = 1
输出: [1,1]
```

 

**提示:**

- `0 <= rowIndex <= 33`

题解：这里应该是<=rowIndex，但是自己偷懒使用了rowIndex++;

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        rowIndex++;
        vector<vector<int>>nums(rowIndex);
        for (int i = 0; i < rowIndex; i++)
        {
            nums[i].resize(i+1);
            for (int j = 0; j < i + 1; j++)
            {
                if (i < 2)
                    nums[i][j] = 1;
                else
                {
                    if (j == 0 || j == i)
                    {
                        nums[i][0] = 1;
                        nums[i][i] = 1;
                    }
                    else
                        nums[i][j] = nums[i - 1][j - 1] + nums[i - 1][j];
                }
            }
        }
        // for (int i = 0; i < nums.size(); i++)
        // {
        //     for (int j = 0; j < nums[i].size(); j++)
        //         cout << nums[i][j] << " ";
        //     cout << endl;
        // }
        return nums[rowIndex-1];

    }
};
```



## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

题解：



## [134. 加油站](https://leetcode.cn/problems/gas-station/)

在一条环路上有 `n` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 `gas` 和 `cost` ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 `-1` 。如果存在解，则 **保证** 它是 **唯一** 的。

 

**示例 1:**

```
输入: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```

**示例 2:**

```
输入: gas = [2,3,4], cost = [3,4,3]
输出: -1
解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。
```

 

**提示:**

- `gas.length == n`
- `cost.length == n`
- `1 <= n <= 105`
- `0 <= gas[i], cost[i] <= 104`

​	题解：依次遍历，这里只要gas+ > cost+ 就可以。但是依次遍历会超时。

（1）官方解法

```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        int i = 0;
        while (i < n) {
            int sumOfGas = 0, sumOfCost = 0;
            int cnt = 0;
            while (cnt < n) {
                int j = (i + cnt) % n;
                sumOfGas += gas[j];
                sumOfCost += cost[j];
                if (sumOfCost > sumOfGas) {
                    break;
                }
                cnt++;
            }
            if (cnt == n) {
                return i;
            } else {
                i = i + cnt + 1;
            }
        }
        return -1;
    }
};
```

 （2）自己解法，使用for循环进行处理。

```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n=gas.size();
        for(int i=0;i<n;i++)
        {
            int sumGas=0, sumCost=0;
            int num=0;
            for(int j=0;j<n;j++)
            {
                int t= (j+i)%n;
                cout<<t;
                sumGas += gas[t];
                sumCost += cost[t];
                if(sumCost > sumGas)
                    break;
                num++;
            }
            cout<<endl;
            if(num == n)
                return i;
            i=i+num; //这一步很关键，如果没有这一步就会超时，因为到这里前面的都没有通过，所以可以直接到下一步
        }
        return -1;
    }
};
```



## [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 

**示例 1 ：**

```
输入：nums = [2,2,1]
输出：1
```

**示例 2 ：**

```
输入：nums = [4,1,2,1,2]
输出：4
```

**示例 3 ：**

```
输入：nums = [1]
输出：1
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- 除了某个元素只出现一次以外，其余每个元素均出现两次。

题解：使用哈希表，使用unordered_map容器，先将每个数字放进容器，最后再迭代一次找出只出现一次的数。

```
//方法一：使用unordered_map容器方法
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int,int>s;
        for(int i=0;i<nums.size();i++)
        {
            s[nums[i]]++;
        }
        for(auto item=s.begin();item!=s.end();item++)
        if(item->second == 1)
            return item->first;
        return 0;

    }
};
```





## [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

 

**提示：**

- `2 <= numbers.length <= 3 * 104`
- `-1000 <= numbers[i] <= 1000`
- `numbers` 按 **非递减顺序** 排列
- `-1000 <= target <= 1000`
- **仅存在一个有效答案**

题解：这道题最容易想到的就是暴力破解，但是肯定会超时。

（1）这是一种错误的做法，因为没有考虑numbers=[1,1,1,1,2]这种情况，这道题说“仅存在一个有效答案”，可以先将所有数存到map哈希表中，然后循环时计算能和targe相匹配的结果。这样就能避免暴力循环时的超时限制。

```
//错误做法
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        map<int,int>nn;
        for(int i=0;i<numbers.size();i++)
            nn[numbers[i]]=i;
        vector<int>aa;
        for(int i=0;i<numbers.size();i++)
        {
            if(target >= numbers[i])
            if(nn.count(target-numbers[i]))
            {
                aa.emplace_back(i+1);
                aa.emplace_back(nn[target-numbers[i]]+1);
                return aa;
            }
        }
        return aa;
    }
};
```

（2）方法二，使用双指针的方式进行计算，这样时间复杂度只为O(n)。

注意：这里的数组是**非递减顺序排列** ，所以可是使用双指针进行计算。

```
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int slow=0,fast=numbers.size()-1;
        while(slow < fast)
        {
            int tt=numbers[slow]+numbers[fast];
            if(tt==target)
                return {slow+1,fast+1};
            else if(tt < target)
                slow++;
            else
                fast--;
        }
        return {-1,-1};
    }
};
```





## [169. 多数元素](https://leetcode.cn/problems/majority-element/)

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`

 

**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

题解：注意这里的题目要求最多元素指的是 **大于** `⌊ n/2 ⌋` 的元素。

（1）方法一：使用map来进行处理，统计每个元素出现的个数。

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int>s;
        for(int i=0;i<nums.size(); i++)
            s[nums[i]]++;
        for(auto item=s.begin();item !=s.end(); item++)
            if((item->second)>=(nums.size()+1)/2)
                return item->first;
        return 0;

    }
};
```

（2）方法二：使用vector当中的count()函数进行元素的统计。vector当中的count()是统计数组中有多少个元素。但是计算速度非常慢。set、map等使用count()是判断当中有没有这个元素，返回的是false和true。

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
         for(int i=0;i<nums.size();i++)
         {
             int num= count(nums.begin(),nums.end(),nums[i]);
             if(num >= ((nums.size()+1)/2))
                 return nums[i];
         }
        return 0;

    }
};
```

## [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

**示例 1:**

```
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

 

**进阶：**

- 尽可能想出更多的解决方案，至少有 **三种** 不同的方法可以解决这个问题。
- 你可以使用空间复杂度为 `O(1)` 的 **原地** 算法解决这个问题吗？

题解：

（1）方法一：再创建一个数组，然后将前一部分的数组复制过来。

（2）方法二：先将前一部分数组补到数组后面，然后删掉前面的部分。这里需要k=k%n，因为会k有可能会比n大的，代表轮换了好几组。

```
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n=nums.size();
        k=k%n;
        for(int i=0;i<(n-k);i++)
            nums.push_back(nums[i]);
        nums.erase(nums.begin(),nums.begin()+(n-k));
    }
};
```





## [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)

给你一个整数数组 `nums` 。如果任一值在数组中出现 **至少两次** ，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。

 

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：true
```

**示例 2：**

```
输入：nums = [1,2,3,4]
输出：false
```

**示例 3：**

```
输入：nums = [1,1,1,3,3,4,3,2,4,2]
输出：true
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

题解：两种方式，第一种unordered_map<int ,int>来计数；方法二：先使用排序，然后判断有没有前后相等的元素。

注意sort函数的使用方式。

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size()-1;i++)
        {
            if(nums[i] == nums[i+1])
                return true;
        }
        return false;
    }
};
```



## [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

给你一个整数数组 `nums` 和一个整数 `k` ，判断数组中是否存在两个 **不同的索引** `i` 和 `j` ，满足 `nums[i] == nums[j]` 且 `abs(i - j) <= k` 。如果存在，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：nums = [1,2,3,1], k = 3
输出：true
```

**示例 2：**

```
输入：nums = [1,0,1,1], k = 1
输出：true
```

**示例 3：**

```
输入：nums = [1,2,3,1,2,3], k = 2
输出：false
```

**提示：**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `0 <= k <= 105`

题解：这里使用unordered_set的count函数，而不用vector下的count函数，是因为后者特别耗时。

```
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_set<int>s;
        for(int i=0;i<nums.size();i++)
        {
            if(s.count(nums[i]))
            {
                for(int j=i-1;j>=0;j--)
                {
                    if(nums[j] == nums[i])
                    if(abs(i-j)<=k)
                        return true;
                }
            }

            s.emplace(nums[i]);
        }
        
        return false;

    }
};
```

方法二：官方给的使用unordered_map方法

```
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> dictionary;
        int length = nums.size();
        for (int i = 0; i < length; i++) {
            int num = nums[i];
            if (dictionary.count(num) && i - dictionary[num] <= k) {
                return true;
            }
            dictionary[num] = i;
        }
        return false;
    }
};
```



## [228. 汇总区间](https://leetcode.cn/problems/summary-ranges/)

给定一个  **无重复元素** 的 **有序** 整数数组 `nums` 。

返回 ***恰好覆盖数组中所有数字** 的 **最小有序** 区间范围列表* 。也就是说，`nums` 的每个元素都恰好被某个区间范围所覆盖，并且不存在属于某个范围但不属于 `nums` 的数字 `x` 。

列表中的每个区间范围 `[a,b]` 应该按如下格式输出：

- `"a->b"` ，如果 `a != b`
- `"a"` ，如果 `a == b`

 

**示例 1：**

```
输入：nums = [0,1,2,4,5,7]
输出：["0->2","4->5","7"]
解释：区间范围是：
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

**示例 2：**

```
输入：nums = [0,2,3,4,6,8,9]
输出：["0","2->4","6","8->9"]
解释：区间范围是：
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

**提示：**

- `0 <= nums.length <= 20`
- `-231 <= nums[i] <= 231 - 1`
- `nums` 中的所有值都 **互不相同**
- `nums` 按升序排列





## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

给你一个整数数组 `nums`，返回 *数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积* 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(*n*)` 时间复杂度内完成此题。

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内

**进阶：**你可以在 `O(1)` 的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组 **不被视为** 额外空间。）

题解：注意题目不允许使用除法，这样就没办法先计算所有数的乘积，然后再除以这个数进行保存（并且出现0时也会出错）。其次暴力求解也会超出时间限制。

官方解法：

（1）空间复杂度O(1)的方法

这种方法是先计算这个数组左边的乘积，然后再计算右边的乘积。

```
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=nums.size();
        vector<int>answer(n);
        answer[0]=1;
        for(int i=1; i<nums.size(); i++)
        {
            answer[i]=answer[i-1]*nums[i-1];
        }
        int R=1;
        for(int i=nums.size()-1; i>=0; i--)
        {
            answer[i] = answer[i]*R;
            R *= nums[i];
        }
        return answer;
    }
};
```

（2）



## [268. 丢失的数字](https://leetcode.cn/problems/missing-number/)

给定一个包含 `[0, n]` 中 `n` 个数的数组 `nums` ，找出 `[0, n]` 这个范围内没有出现在数组中的那个数。

**示例 1：**

```
输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 2：**

```
输入：nums = [0,1]
输出：2
解释：n = 2，因为有 2 个数字，所以所有的数字都在范围 [0,2] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 3：**

```
输入：nums = [9,6,4,2,3,5,7,0,1]
输出：8
解释：n = 9，因为有 9 个数字，所以所有的数字都在范围 [0,9] 内。8 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 4：**

```
输入：nums = [0]
输出：1
解释：n = 1，因为有 1 个数字，所以所有的数字都在范围 [0,1] 内。1 是丢失的数字，因为它没有出现在 nums 中。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- `nums` 中的所有数字都 **独一无二**

题解：先使用sort函数排序，再进行处理。

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        
        for(int i=0;i<nums.size();i++)
        // cout<<nums[i]<<" ";
            if(nums[i] != i)
            return i;
        return nums.size();
    }
};
```



## [274. H 指数](https://leetcode.cn/problems/h-index/)

给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数。计算并返回该研究者的 **`h` 指数**。

根据维基百科上 [h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：`h` 代表“高引用次数” ，一名科研人员的 `h` **指数** 是指他（她）至少发表了 `h` 篇论文，并且 **至少** 有 `h` 篇论文被引用次数大于等于 `h` 。如果 `h` 有多种可能的值，**`h` 指数** 是其中最大的那个。

**示例 1：**

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

**示例 2：**

```
输入：citations = [1,3,1]
输出：1
```

 

**提示：**

- `n == citations.length`
- `1 <= n <= 5000`
- `0 <= citations[i] <= 1000`

题解：

（1）我对这道题的理解是，数组内所有数相加求得平均数，然后只要数组内元素大于平均数就count+1。但显然这种理解是有问题的。

（2）官方解法，排序

```
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end());
        int h = 0, i = citations.size() - 1;
        while (i >= 0 && citations[i] > h) {
            h++;
            i--;
        }
        return h;
    }
};
```







## [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

 

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

**提示**:

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

题解：先把含有0的位置清除掉，记得从尾部开始处理，然后尾部补0；

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int num=0;
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nums[i] == 0)
            {
                nums.erase(nums.begin()+i);
                num++;
            }
        }
        for(int i=0;i<num;i++)
            nums.push_back(0);
    }
};
```

官方方法，使用双指针，发现0进行位置交换。

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size(), left = 0, right = 0;
        while (right < n) {
            if (nums[right]) {
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }
    }
};
```



## [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)

给定一个整数数组  `nums`，处理以下类型的多个查询:

1. 计算索引 `left` 和 `right` （包含 `left` 和 `right`）之间的 `nums` 元素的 **和** ，其中 `left <= right`

实现 `NumArray` 类：

- `NumArray(int[] nums)` 使用数组 `nums` 初始化对象
- `int sumRange(int i, int j)` 返回数组 `nums` 中索引 `left` 和 `right` 之间的元素的 **总和** ，包含 `left` 和 `right` 两点（也就是 `nums[left] + nums[left + 1] + ... + nums[right]` )

 

**示例 1：**

```
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

 

**提示：**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= i <= j < nums.length`
- 最多调用 `104` 次 `sumRange` 方法

题解：注意这里类构造函数带参数是怎么使用的。

```
#include<iostream>
#include<vector>
using namespace std;
class NumArray {
public:
    vector<int>sums;
    NumArray(vector<int>& nums) {
        this->sums = nums;
    }

    int sumRange(int left, int right) {
        int sum=0;
        while (left <= right)
        {
            cout << sums[left] << " ";
            sum = sum + sums[left];
            left++;
        }
        return sum;
    }
    void aa()
    {
        for (int i = 0; i < sums.size(); i++)
            cout << sums[i] << " ";
    }
};
int main()
{
    vector<int>num = { -2, 0, 3, -5, 2, -1 };
    NumArray myNumArray(num);
    cout<<myNumArray.sumRange(2,5);
}
```



## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)



简单





815





相关企业

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

题解：



## [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)



给你两个整数数组 `nums1` 和 `nums2` ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

**示例 2:**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

题解：



## [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)

实现`RandomizedSet` 类：

- `RandomizedSet()` 初始化 `RandomizedSet` 对象
- `bool insert(int val)` 当元素 `val` 不存在时，向集合中插入该项，并返回 `true` ；否则，返回 `false` 。
- `bool remove(int val)` 当元素 `val` 存在时，从集合中移除该项，并返回 `true` ；否则，返回 `false` 。
- `int getRandom()` 随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有 **相同的概率** 被返回。

你必须实现类的所有函数，并满足每个函数的 **平均** 时间复杂度为 `O(1)` 。

**示例：**

```
输入
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
输出
[null, true, false, true, 2, true, false, 2]

解释
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomizedSet.remove(2); // 返回 false ，表示集合中不存在 2 。
randomizedSet.insert(2); // 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomizedSet.getRandom(); // getRandom 应随机返回 1 或 2 。
randomizedSet.remove(1); // 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomizedSet.insert(2); // 2 已在集合中，所以返回 false 。
randomizedSet.getRandom(); // 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
```

**提示：**

- `-231 <= val <= 231 - 1`
- 最多调用 `insert`、`remove` 和 `getRandom` 函数 `2 * ``105` 次
- 在调用 `getRandom` 方法时，数据结构中 **至少存在一个** 元素。

题解：这道题很关键，可以帮助自己理解类的书写方法。

```
class RandomizedSet {
public:
    RandomizedSet() {
        srand((unsigned)time(NULL));
    }
    
    bool insert(int val) {
        if(indices.count(val))
            return false;
        int index = nums.size();
        nums.emplace_back(val);
        indices[val] = index;
        return true;
    }
    
    bool remove(int val) {
        if(!indices.count(val))
            return false;
        int index = indices[val];
        int last=nums.back();
        nums[index] = last;
        indices[last] = index;
        nums.pop_back();
        indices.erase(val);
        return true;

    }
    
    int getRandom() {
        int randomIndex = rand()%nums.size();
        return nums[randomIndex];
    }
private:
        vector<int>nums;
        unordered_map<int,int>indices;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```



## [414. 第三大的数](https://leetcode.cn/problems/third-maximum-number/)

给你一个非空数组，返回此数组中 **第三大的数** 。如果不存在，则返回数组中最大的数。

 

**示例 1：**

```
输入：[3, 2, 1]
输出：1
解释：第三大的数是 1 。
```

**示例 2：**

```
输入：[1, 2]
输出：2
解释：第三大的数不存在, 所以返回最大的数 2 。
```

**示例 3：**

```
输入：[2, 2, 3, 1]
输出：1
解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。
此例中存在两个值为 2 的数，它们都排第二。在所有不同数字中排第三大的数为 1 。
```

 

**提示：**

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

题解：先将数组排序，然后寻找有没有第三大的数。

```
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int flag=1;
        for(int i=nums.size()-2;i>=0;i--)
        {
            if(nums[i] < nums[i+1])
            {
                flag++;
                if(flag==3)
                    return nums[i];
            }
        }
        return nums[nums.size()-1];

    }
};
```



## [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**示例 1:**

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

**示例 2:**

```
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

**提示：**

- `1 <= g.length <= 3 * 104`
- `0 <= s.length <= 3 * 104`
- `1 <= g[i], s[j] <= 231 - 1`

题解：



## [463. 岛屿的周长](https://leetcode.cn/problems/island-perimeter/)

给定一个 `row x col` 的二维网格地图 `grid` ，其中：`grid[i][j] = 1` 表示陆地， `grid[i][j] = 0` 表示水域。

网格中的格子 **水平和垂直** 方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/island.png)

```
输入：grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
输出：16
解释：它的周长是上面图片中的 16 个黄色的边
```

**示例 2：**

```
输入：grid = [[1]]
输出：4
```

**示例 3：**

```
输入：grid = [[1,0]]
输出：4
```

 

**提示：**

- `row == grid.length`
- `col == grid[i].length`
- `1 <= row, col <= 100`
- `grid[i][j]` 为 `0` 或 `1`

题解：可以分为两部分，先判断上下边，即判断当前岛屿上下有没有岛屿（特例：只有一层的情况）；第二判断左右边，即判断当前岛屿左右有没有岛屿（特例：只有一列的情况）。

```
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int num=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[i].size();j++)
            {
                if(grid[i][j] == 1)
                {
                    //上下左右有没有陆地判断。
                    if(i==0)
                    {
                        num++;
                        if(grid.size()>1)
                        if(grid[i+1][j] != 1)
                            num++;
                    }
                        
                    if(i != 0 && i != grid.size()-1)
                    {
                        if(grid[i-1][j] != 1)
                            num++;
                        if(grid[i+1][j] != 1)
                            num++;
                    }

                    
                    if(i == grid.size()-1)
                    {
                        num++;
                        if(grid.size()>1)
                        if(grid[i-1][j] != 1)
                            num++;
                    }

                    if(j==0)
                    {
                        num++;
                        if(grid[i].size()>1)
                        if(grid[i][j+1] != 1)
                            num++;
                    }

                    if(j != 0 && j != grid[i].size()-1)
                    {
                        if(grid[i][j-1] != 1)
                            num++;
                        if(grid[i][j+1] != 1)
                            num++;
                    }

                    
                    if(j == grid[i].size()-1)
                    {
                        num++;
                        if(grid[i].size()>1)
                        if(grid[i][j-1] != 1)
                            num++;
                    }
                }
            }
        }
        return num;

    }
};
```



## [485. 最大连续 1 的个数](https://leetcode.cn/problems/max-consecutive-ones/)

给定一个二进制数组 `nums` ， 计算其中最大连续 `1` 的个数。



**示例 1：**

```
输入：nums = [1,1,0,1,1,1]
输出：3
解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
```

**示例 2:**

```
输入：nums = [1,0,1,1,0,1]
输出：2
```

**提示：**

- `1 <= nums.length <= 105`
- `nums[i]` 不是 `0` 就是 `1`.

题解：这里首先给nums数组后面再添加一个0，然后每次计数连续1的个数放入新数组中，最后对数组进行排序，返回数组的最后一位就是最大的。

```
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        nums.push_back(0);
        int flag=0;
        int num=0;
        vector<int>nn;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i])
                flag=1;
            else flag=0;

            if(flag)
                num++;
            if(flag==0)
            {
                nn.push_back(num);
                num=0;
            }
        }
        sort(nn.begin(),nn.end());
        return nn[nn.size()-1];

    }
};
```

官方给出的方法更简洁，只需要每次计数当前连续的数组，每次保存一个最大连续个数。

```
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int maxCount = 0, count = 0;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) {
                count++;
            } else {
                maxCount = max(maxCount, count);
                count = 0;
            }
        }
        maxCount = max(maxCount, count);
        return maxCount;
    }
};
```



## [495. 提莫攻击](https://leetcode.cn/problems/teemo-attacking/)

在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄。他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。

当提莫攻击艾希，艾希的中毒状态正好持续 `duration` 秒。

正式地讲，提莫在 `t` 发起攻击意味着艾希在时间区间 `[t, t + duration - 1]`（含 `t` 和 `t + duration - 1`）处于中毒状态。如果提莫在中毒影响结束 **前** 再次攻击，中毒状态计时器将会 **重置** ，在新的攻击之后，中毒影响将会在 `duration` 秒后结束。

给你一个 **非递减** 的整数数组 `timeSeries` ，其中 `timeSeries[i]` 表示提莫在 `timeSeries[i]` 秒时对艾希发起攻击，以及一个表示中毒持续时间的整数 `duration` 。

返回艾希处于中毒状态的 **总** 秒数。

**示例 1：**

```
输入：timeSeries = [1,4], duration = 2
输出：4
解释：提莫攻击对艾希的影响如下：
- 第 1 秒，提莫攻击艾希并使其立即中毒。中毒状态会维持 2 秒，即第 1 秒和第 2 秒。
- 第 4 秒，提莫再次攻击艾希，艾希中毒状态又持续 2 秒，即第 4 秒和第 5 秒。
艾希在第 1、2、4、5 秒处于中毒状态，所以总中毒秒数是 4 。
```

**示例 2：**

```
输入：timeSeries = [1,2], duration = 2
输出：3
解释：提莫攻击对艾希的影响如下：
- 第 1 秒，提莫攻击艾希并使其立即中毒。中毒状态会维持 2 秒，即第 1 秒和第 2 秒。
- 第 2 秒，提莫再次攻击艾希，并重置中毒计时器，艾希中毒状态需要持续 2 秒，即第 2 秒和第 3 秒。
艾希在第 1、2、3 秒处于中毒状态，所以总中毒秒数是 3 。
```

**提示：**

- `1 <= timeSeries.length <= 104`
- `0 <= timeSeries[i], duration <= 107`
- `timeSeries` 按 **非递减** 顺序排列

题解：这里只要本次攻击时间+技能释放时间 < 下次攻击时间，就+技能释放时间，否则只能+两次技能释放时间间隔。最后再加一次技能释放时间。

```
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        int num=0;
        for(int i=0;i<timeSeries.size()-1;i++)
        {
            if(timeSeries[i]-1+duration < timeSeries[i+1])
                num += duration;
            else
                num = num + (timeSeries[i+1]-timeSeries[i]);
        }
        num += duration;
        return num;
    }
};
```



## [506. 相对名次](https://leetcode.cn/problems/relative-ranks/)

给你一个长度为 `n` 的整数数组 `score` ，其中 `score[i]` 是第 `i` 位运动员在比赛中的得分。所有得分都 **互不相同** 。

运动员将根据得分 **决定名次** ，其中名次第 `1` 的运动员得分最高，名次第 `2` 的运动员得分第 `2` 高，依此类推。运动员的名次决定了他们的获奖情况：

- 名次第 `1` 的运动员获金牌 `"Gold Medal"` 。
- 名次第 `2` 的运动员获银牌 `"Silver Medal"` 。
- 名次第 `3` 的运动员获铜牌 `"Bronze Medal"` 。
- 从名次第 `4` 到第 `n` 的运动员，只能获得他们的名次编号（即，名次第 `x` 的运动员获得编号 `"x"`）。

使用长度为 `n` 的数组 `answer` 返回获奖，其中 `answer[i]` 是第 `i` 位运动员的获奖情况。

 

**示例 1：**

```
输入：score = [5,4,3,2,1]
输出：["Gold Medal","Silver Medal","Bronze Medal","4","5"]
解释：名次为 [1st, 2nd, 3rd, 4th, 5th] 。
```

**示例 2：**

```
输入：score = [10,3,8,9,4]
输出：["Gold Medal","5","Bronze Medal","Silver Medal","4"]
解释：名次为 [1st, 5th, 3rd, 2nd, 4th] 。
```

 

**提示：**

- `n == score.length`
- `1 <= n <= 104`
- `0 <= score[i] <= 106`
- `score` 中的所有值 **互不相同**

题解：先使用一个链表对score进行排序，然后利用两层遍历进行寻找，本来害怕两层遍历会超出时间限制，但是这道题没有超出。

```
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        vector<int>ss=score;
        vector<string>nums;
        sort(ss.begin(),ss.end());
        reverse(ss.begin(),ss.end());
        for(int i=0;i<score.size();i++)
        {
            for(int j=0;j<ss.size();j++)
            {
                if(score[i] == ss[j])
                {
                    if(j==0)
                        nums.push_back("Gold Medal");
                    else if(j==1)
                        nums.push_back("Silver Medal");
                    else if(j==2)
                    nums.push_back("Bronze Medal");
                    else
                        nums.push_back(to_string(j+1));
                    break;
                }
            }
        }
        return nums;
    }
};
```



## [561. 数组拆分](https://leetcode.cn/problems/array-partition/)

给定长度为 `2n` 的整数数组 `nums` ，你的任务是将这些数分成 `n` 对, 例如 `(a1, b1), (a2, b2), ..., (an, bn)` ，使得从 `1` 到 `n` 的 `min(ai, bi)` 总和最大。

返回该 **最大总和** 。

 

**示例 1：**

```
输入：nums = [1,4,3,2]
输出：4
解释：所有可能的分法（忽略元素顺序）为：
1. (1, 4), (2, 3) -> min(1, 4) + min(2, 3) = 1 + 2 = 3
2. (1, 3), (2, 4) -> min(1, 3) + min(2, 4) = 1 + 2 = 3
3. (1, 2), (3, 4) -> min(1, 2) + min(3, 4) = 1 + 3 = 4
所以最大总和为 4
```

**示例 2：**

```
输入：nums = [6,2,6,5,1,2]
输出：9
解释：最优的分法为 (2, 1), (2, 5), (6, 6). min(2, 1) + min(2, 5) + min(6, 6) = 1 + 2 + 6 = 9
```

 

**提示：**

- `1 <= n <= 104`
- `nums.length == 2 * n`
- `-104 <= nums[i] <= 104`

题解：这道题其实就是排序，然后每两个之间最小，但是总和最大。例如从1到6，只有（1，2）（3，4）（5，6）=1+3+5=9最大。

```
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int num=0;
        for(int i=0;i<nums.size();i=i+2)
        {
            num += nums[i];
        }
        return num;

    }
};
```



## [566. 重塑矩阵](https://leetcode.cn/problems/reshape-the-matrix/)

在 MATLAB 中，有一个非常有用的函数 `reshape` ，它可以将一个 `m x n` 矩阵重塑为另一个大小不同（`r x c`）的新矩阵，但保留其原始数据。

给你一个由二维数组 `mat` 表示的 `m x n` 矩阵，以及两个正整数 `r` 和 `c` ，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的 **行遍历顺序** 填充。

如果具有给定参数的 `reshape` 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg)

```
输入：mat = [[1,2],[3,4]], r = 1, c = 4
输出：[[1,2,3,4]]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg)

```
输入：mat = [[1,2],[3,4]], r = 2, c = 4
输出：[[1,2],[3,4]]
```

 

**提示：**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 100`
- `-1000 <= mat[i][j] <= 1000`
- `1 <= r, c <= 300`





## [575. 分糖果](https://leetcode.cn/problems/distribute-candies/)

Alice 有 `n` 枚糖，其中第 `i` 枚糖的类型为 `candyType[i]` 。Alice 注意到她的体重正在增长，所以前去拜访了一位医生。

医生建议 Alice 要少摄入糖分，只吃掉她所有糖的 `n / 2` 即可（`n` 是一个偶数）。Alice 非常喜欢这些糖，她想要在遵循医生建议的情况下，尽可能吃到最多不同种类的糖。

给你一个长度为 `n` 的整数数组 `candyType` ，返回： Alice *在仅吃掉 `n / 2` 枚糖的情况下，可以吃到糖的 **最多** 种类数*。

 

**示例 1：**

```
输入：candyType = [1,1,2,2,3,3]
输出：3
解释：Alice 只能吃 6 / 2 = 3 枚糖，由于只有 3 种糖，她可以每种吃一枚。
```

**示例 2：**

```
输入：candyType = [1,1,2,3]
输出：2
解释：Alice 只能吃 4 / 2 = 2 枚糖，不管她选择吃的种类是 [1,2]、[1,3] 还是 [2,3]，她只能吃到两种不同类的糖。
```

**示例 3：**

```
输入：candyType = [6,6,6,6]
输出：1
解释：Alice 只能吃 4 / 2 = 2 枚糖，尽管她能吃 2 枚，但只能吃到 1 种糖。
```

 

**提示：**

- `n == candyType.length`
- `2 <= n <= 104`
- `n` 是一个偶数
- `-105 <= candyType[i] <= 105`



## [830. 较大分组的位置](https://leetcode.cn/problems/positions-of-large-groups/)

在一个由小写字母构成的字符串 `s` 中，包含由一些连续的相同字符所构成的分组。

例如，在字符串 `s = "abbxxxxzyy"` 中，就含有 `"a"`, `"bb"`, `"xxxx"`, `"z"` 和 `"yy"` 这样的一些分组。

分组可以用区间 `[start, end]` 表示，其中 `start` 和 `end` 分别表示该分组的起始和终止位置的下标。上例中的 `"xxxx"` 分组用区间表示为 `[3,6]` 。

我们称所有包含大于或等于三个连续字符的分组为 **较大分组** 。

找到每一个 **较大分组** 的区间，**按起始位置下标递增顺序排序后**，返回结果。

 

**示例 1：**

```
输入：s = "abbxxxxzzy"
输出：[[3,6]]
解释："xxxx" 是一个起始于 3 且终止于 6 的较大分组。
```

**示例 2：**

```
输入：s = "abc"
输出：[]
解释："a","b" 和 "c" 均不是符合要求的较大分组。
```

**示例 3：**

```
输入：s = "abcdddeeeeaabbbcd"
输出：[[3,5],[6,9],[12,14]]
解释：较大分组为 "ddd", "eeee" 和 "bbb"
```

**示例 4：**

```
输入：s = "aba"
输出：[]
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅含小写英文字母

题解：需要知道二维数组存储数据的方式。

（1）str.push_back(vector<int>());
                str[nn].push_back(i - num);
                str[nn].push_back(i);

（2）ret.push_back({i - num + 1, i});

（3）先开辟数组，然后按照数组的方式存储。

```
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        s += " ";
        vector<vector<int>>str;
        int num = 0;
        int nn = 0;
        int flag = 0;
        for (int i = 0; i < s.size(); i++)
        {
            //cout << s[i];
            if (s[i] == s[i + 1])
            {
                flag = 1;
                num++;
            }
            else
                flag = 0;
            if (flag == 0 && num >= 2)
            {
                str.push_back(vector<int>());
                str[nn].push_back(i - num);
                str[nn].push_back(i);
                nn++;
            }
            if (!flag)
                num = 0;

        }
        return str;

    }
};
```



## [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

在柠檬水摊上，每一杯柠檬水的售价为 `5` 美元。顾客排队购买你的产品，（按账单 `bills` 支付的顺序）一次购买一杯。

每位顾客只买一杯柠檬水，然后向你付 `5` 美元、`10` 美元或 `20` 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 `5` 美元。

注意，一开始你手头没有任何零钱。

给你一个整数数组 `bills` ，其中 `bills[i]` 是第 `i` 位顾客付的账。如果你能给每位顾客正确找零，返回 `true` ，否则返回 `false` 。

 

**示例 1：**

```
输入：bills = [5,5,5,10,20]
输出：true
解释：
前 3 位顾客那里，我们按顺序收取 3 张 5 美元的钞票。
第 4 位顾客那里，我们收取一张 10 美元的钞票，并返还 5 美元。
第 5 位顾客那里，我们找还一张 10 美元的钞票和一张 5 美元的钞票。
由于所有客户都得到了正确的找零，所以我们输出 true。
```

**示例 2：**

```
输入：bills = [5,5,10,10,20]
输出：false
解释：
前 2 位顾客那里，我们按顺序收取 2 张 5 美元的钞票。
对于接下来的 2 位顾客，我们收取一张 10 美元的钞票，然后返还 5 美元。
对于最后一位顾客，我们无法退回 15 美元，因为我们现在只有两张 10 美元的钞票。
由于不是每位顾客都得到了正确的找零，所以答案是 false。
```

 

**提示：**

- `1 <= bills.length <= 105`
- `bills[i]` 不是 `5` 就是 `10` 或是 `20` 

```
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        if(bills[0] != 5)
                return false;
        unordered_map<int,int>bb;
        for(int i=0;i<bills.size();i++)
        {
            bb[bills[i]]++;
            if(i>0 && bills[i] !=5)
            {
                if(bills[i] == 10)
                {
                    if(bb[5]>0)
                    {
                        bb[5]--;
                        // cout<<"1--bb[5]: "<<bb[5]<<endl;
                    }
                    else
                        return false;
                }
                if(bills[i] == 20)
                {
                    if(bb[5]>0 && bb[10]>0)
                    {
                        bb[10]--;
                        bb[5]--;
                        // cout<<"2---bb[5]: "<<bb[5]<<endl;
                        // cout<<"2---bb[10]: "<<bb[10]<<endl;
                    }
                    else if(bb[5]>2)
                    {
                        bb[5] = bb[5]-3;
                        // cout<<"3--bb[5]: "<<bb[5]<<endl;
                    }
                        
                    else
                        return false;
                }
            }
        }
        return true;

    }
};
```

官方给出的更简洁的code

```
class Solution {
public:
    bool lemonadeChange(vector<int> &bills) {
        int five = 0, ten = 0;
        for (int b: bills) {
            if (b == 5) five++; // 无需找零
            else if (b == 10) five--, ten++; // 返还 5
            else if (ten) five--, ten--; // 此时 b=20，返还 10+5
            else five -= 3; // 此时 b=20，返还 5+5+5
            if (five < 0) return false; // 无法正确找零
        }
        return true;
    }
};
```



## [1122. 数组的相对排序](https://leetcode.cn/problems/relative-sort-array/)

给你两个数组，`arr1` 和 `arr2`，`arr2` 中的元素各不相同，`arr2` 中的每个元素都出现在 `arr1` 中。

对 `arr1` 中的元素进行排序，使 `arr1` 中项的相对顺序和 `arr2` 中的相对顺序相同。未在 `arr2` 中出现过的元素需要按照升序放在 `arr1` 的末尾。

 

**示例 1：**

```
输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]
```

**示例  2:**

```
输入：arr1 = [28,6,22,8,44,17], arr2 = [22,28,8,6]
输出：[22,28,8,6,17,44]
```

 

**提示：**

- `1 <= arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- `arr2` 中的元素 `arr2[i]` **各不相同** 
- `arr2` 中的每个元素 `arr2[i]` 都出现在 `arr1` 中

题解：注意两个vector链表的拼接，使用insert(s1.end(),s2.begin(),s2.end())。

```
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        vector<int>str;
        unordered_set<int>a2;
        for(int i=0;i<arr2.size();i++)
            a2.emplace(arr2[i]);
        unordered_map<int,int>a1;
        for(int i=0;i<arr1.size();i++)
        {
            if(!a2.count(arr1[i]))
                str.push_back(arr1[i]);
            else
            {
                a1[arr1[i]]++;
            }
        }
        vector<int>nn;
        for(int i=0;i<arr2.size();i++)
        {
            for(int j=0;j<a1[arr2[i]];j++)
            {
                nn.push_back(arr2[i]);
            }
        }
        sort(str.begin(),str.end());
        nn.insert(nn.end(),str.begin(),str.end());
        return nn;
    }
};
```



# 三、栈





# 四、链表

## 链表

```
#include<iostream>
using namespace std;

struct node {
	int value;
	struct node* next;
};
int main()
{
	struct node a;  //是一个空节点
	struct node b;
	struct node* list = NULL;  //是一个空链表
	list = &a;     //list链表，含有一个节点

	//再添加一个节点
	a.next = &b;
	b.next = NULL;
	
	return 0;
}
```



### 2、链表的类型

（1）有没有带头节点

（1.1）带头节点的单链表

（1.2）不带头节点的单链表

（2）有几个方向

（2.1）单链表

（2.2）双向链表

（3）有没有环

（3.1）链表

（3.2）循环链表

### 3、代码实践

#### 3.1 创建空链表

```
//创建空链表
struct node* list_creat()
{
	//malloc 申请内存失败，则返回NULL;
	struct node* list = (struct node*)malloc(sizeof(struct node));
	if (list)
	{
		list->next = NULL;
	}
	return list;
}
```

#### 3.2 创建带头节点的单链表来存储一个数组

问题：不明白的地方，这里创建了一个头节点list，但是指针p依次指向下一个位置，最后却返回一个list，这里list明明没有参与任何过程，怎么就和下一个节点链接上了呢？

```
//使用数组来创建一个链表,带头节点的
struct node* list_creat(int data[], int n)
{
	//创建头节点
	struct node* list = (struct node*)malloc(sizeof(struct node));
	if (list == NULL)
		return NULL;
	struct node* p = list;
	for (int i = 0; i < n; i++)
	{
		//创建新节点
		struct node* tmp = (struct node*)malloc(sizeof(struct node));
		//设置数据
		tmp->value = data[i];
		//链接
		p->next = tmp;
		//p指针后移
		p = p->next;
	}
	//尾部为空
	p->next = NULL;
	return list;

}
```

#### 3.3 链表的遍历

```
#include<iostream>
using namespace std;

struct node {
	int value;
	struct node* next;
};
//创建空链表
struct node* list_creat()
{
	//malloc 申请内存失败，则返回NULL;
	struct node* list = (struct node*)malloc(sizeof(struct node));
	if (list)
	{
		list->next = NULL;
	}
	return list;
}

//使用数组来创建一个链表,带头节点的
struct node* list_creat(int data[], int n)
{
	//创建头节点
	struct node* list = (struct node*)malloc(sizeof(struct node));
	if (list == NULL)
		return NULL;
	struct node* p = list;
	for (int i = 0; i < n; i++)
	{
		//创建新节点
		struct node* tmp = (struct node*)malloc(sizeof(struct node));
		//设置数据
		tmp->value = data[i];
		//链接
		p->next = tmp;
		//p指针后移
		p = p->next;
	}
	//尾部为空
	p->next = NULL;
	return list;

}
//链表的遍历
void list_visit(struct node* list)
{
	if (list == NULL)
		return;
	for (struct node* p = list->next; p; p = p->next)
	{
		cout << p->value << endl;
	}
}
int main()
{
	int data[] = { 1,2,3,4,5 };
	struct node* list = list_creat(data, 5);
	list_visit(list);
	
	return 0;
}
```

#### 3.4 删除链表中某个值

```
//删除链表中,包含value的节点
bool list_del(struct node* list, int value)
{
	if (list == NULL)
		return false;

	struct node* p;
	for (p = list; p->next; p = p->next)
	{
		if (p->next->value == value)
			break;
	}
	
	//p指向了含有value的节点
	if (p->next == NULL)
		return false;

	//此时已经找到了value，需要释放value这个节点
	struct node* tmp = p->next; //使用临时指针，指向需要删除的节点

	p->next = tmp->next;
	free(tmp);  //释放需要删除的节点内存
	return true;
}
```



## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)



将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

 

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列

题解：因为这里list1和list2都是升序链表，不需要再排序。方法一，首先创建一个头节点，使链表指向头节点，然后依次判断list1和list2节点上的值；

方法二：官方给出的递归方法，还没有理解。

```
//方法一：创建一个头节点，然后进行链接。
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode dummy = ListNode(-1);
        ListNode *prev = &dummy;

        while(list1 != nullptr && list2 != nullptr)
        {
            if(list1->val < list2->val)
            {
                prev->next=list1;
                list1=list1->next;
            }
            else
            {
                prev->next = list2;
                list2 = list2->next;
            }
            prev = prev->next;
        }
        // prev->next=nullptr;

        if(list1 == nullptr)
            prev->next = list2;
        else
            prev->next = list1;

        return dummy.next;
        
    }
};
```

```
//方法二：官方给出的递归方法
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        } else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```



## [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)



给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

 

**提示：**

- 链表中节点数目在范围 `[0, 300]` 内
- `-100 <= Node.val <= 100`
- 题目数据保证链表已经按升序 **排列**

题解：这里有两个需要注意的地方，如果是比赛，这里不释放野指针没问题，但是实际项目中是要释放野指针的。

```
// 参加比赛，不释放野指针的内存
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr)
            return head;
        ListNode *p=head;
        while( p->next != nullptr)
        {
            if(p->val == p->next->val)
            {
                p->next = p->next->next;
            }
            else
            {
                p = p->next;
            }
                
        }
        return head;

    }
};
```

```
//方法二，需要删除释放的指针，这里暂时还不会

```



## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)



给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 104]`
- `-105 <= Node.val <= 105`
- `pos` 为 `-1` 或者链表中的一个 **有效索引** 。

题解：两种方式都是官方给出的。方法一：使用哈希表来存储已经访问过的节点；方法二：采用快慢指针的方式进行判断。

```
//方法一：哈希表存储
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode*> seen;
        while(head != nullptr)
        {
            if(seen.count(head))
            {
                return true;
            }
            seen.insert(head);
            head=head->next;
        }
        return false;
        
    }
};
```

```
//方式二：采用快慢指针方式判断。
class Solution {
public:
    bool hasCycle(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return false;
        }
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (slow != fast) {
            if (fast == nullptr || fast->next == nullptr) {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        return true;
    }
};

```



## [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)



给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

 

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

题解：这个题首先需要给一个头节点，否则会出现head=[7,7,7,7]这种情况，输出结果为7，这种结果，但是建立头节点就可以了。

```
//这个方法不完整，没有考虑head=[7,7,7,7]这种情况，输出结果为7；
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(head == nullptr)
            return head;
        ListNode *p=head;
        while(head != nullptr && head->next != nullptr)
        {
            if(head->next->val == val)
            {
                ListNode *tmp = head->next;
                head->next = head->next->next;
            }
            else 
                head=head->next;
        }
        return p;

    }
};
```

```
//这种方法为在原来的链表上进行处理。但是开始时的while不明白是为什么。
//为什么开始时while而不是if，因为如果链表为[7,7,7,7],那么这里只能删掉第一个数，最后还是会返回一个结果7。
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while(head != nullptr && head->val==val)
        {
            ListNode *tmp=head;
            head=head->next;
            delete tmp;
        }

        ListNode *p=head;
        while(head != nullptr && head->next != nullptr)
        {
            if(head->next->val == val)
            {
                ListNode *tmp = head->next;
                head->next = head->next->next;
            }
            else 
                head=head->next;
        }
        return p;

    }
};
```

```
//建立一个新链表，然后再进行判断。
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        //方法1，先建立一个新表头
        ListNode *Head1 = new ListNode(0);
        Head1->next = head;
        ListNode *p=Head1;
        while( p->next != nullptr)
        {
            if(p->next->val == val)
            {
                ListNode * tmp=p->next;
                p->next = p->next->next;
                delete tmp;
            }
            else
                p=p->next;
        }
        head = Head1->next;
        delete Head1;
        return head;
    }
};
```



## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

题解：没看懂



## [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)



给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
输入：head = [1,2]
输出：false
```

 

**提示：**

- 链表中节点数目在范围`[1, 105]` 内
- `0 <= Node.val <= 9`

题解：把链表中的数存到数组当中，然后对数组进行判断是否是回文。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {

        vector<int>aa;
        while(head)
        {
            aa.push_back(head->val);
            head=head->next;
        }
        int left=0,right=aa.size()-1;
        while(left < right)
        {
            if(aa[left] != aa[right])
                return false;
            left++;
            right--;
        }
        return true;

    }
};
```



## [705. 设计哈希集合](https://leetcode.cn/problems/design-hashset/)

不使用任何内建的哈希表库设计一个哈希集合（HashSet）。

实现 `MyHashSet` 类：

- `void add(key)` 向哈希集合中插入值 `key` 。
- `bool contains(key)` 返回哈希集合中是否存在这个值 `key` 。
- `void remove(key)` 将给定值 `key` 从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

**示例：**

```
输入：
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
输出：
[null, null, null, true, false, null, true, null, false]

解释：
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // 返回 True
myHashSet.contains(3); // 返回 False ，（未找到）
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // 返回 True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // 返回 False ，（已移除）
```

 

**提示：**

- `0 <= key <= 106`
- 最多调用 `104` 次 `add`、`remove` 和 `contains`

题解：



## [706. 设计哈希映射](https://leetcode.cn/problems/design-hashmap/)

不使用任何内建的哈希表库设计一个哈希映射（HashMap）。

实现 `MyHashMap` 类：

- `MyHashMap()` 用空映射初始化对象
- `void put(int key, int value)` 向 HashMap 插入一个键值对 `(key, value)` 。如果 `key` 已经存在于映射中，则更新其对应的值 `value` 。
- `int get(int key)` 返回特定的 `key` 所映射的 `value` ；如果映射中不包含 `key` 的映射，返回 `-1` 。
- `void remove(key)` 如果映射中存在 `key` 的映射，则移除 `key` 和它所对应的 `value` 。

 

**示例：**

```
输入：
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
输出：
[null, null, null, 1, -1, null, 1, null, -1]

解释：
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // myHashMap 现在为 [[1,1]]
myHashMap.put(2, 2); // myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(1);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(3);    // 返回 -1（未找到），myHashMap 现在为 [[1,1], [2,2]]
myHashMap.put(2, 1); // myHashMap 现在为 [[1,1], [2,1]]（更新已有的值）
myHashMap.get(2);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,1]]
myHashMap.remove(2); // 删除键为 2 的数据，myHashMap 现在为 [[1,1]]
myHashMap.get(2);    // 返回 -1（未找到），myHashMap 现在为 [[1,1]]
```

 

**提示：**

- `0 <= key, value <= 106`
- 最多调用 `104` 次 `put`、`get` 和 `remove` 方法

题解：



## [876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

给你单链表的头结点 `head` ，请你找出并返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[3,4,5]
解释：链表只有一个中间结点，值为 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

```
输入：head = [1,2,3,4,5,6]
输出：[4,5,6]
解释：该链表有两个中间结点，值分别为 3 和 4 ，返回第二个结点。
```

 

**提示：**

- 链表的结点数范围是 `[1, 100]`
- `1 <= Node.val <= 100`

题解：通过第一次遍历的时候开始计数，然后在第二次遍历的时候，当计数达到这个位置时，返回这个位置。

​	方法二：采用快慢指针的方法进行处理，用两个指针 `slow` 与 `fast` 一起遍历链表。`slow` 一次走一步，`fast` 一次走两步。那么当 `fast` 到达链表的末尾时，`slow` 必然位于中间。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        int num1=0;
        ListNode *p=head;
        while(p)
        {
            num1++;
            p=p->next;
        }
        p=head;
        num1=num1/2;
        ListNode *pp;
        int num=0;
        while(p)
        {
            
            if(num == num1)
            {
                pp=head;
                return pp;
            }
            head=head->next;
            num++;
        }
        return pp;

    }
};
```

```
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```



## [1290. 二进制链表转整数](https://leetcode.cn/problems/convert-binary-number-in-a-linked-list-to-integer/)



给你一个单链表的引用结点 `head`。链表中每个结点的值不是 0 就是 1。已知此链表是一个整数数字的二进制表示形式。

请你返回该链表所表示数字的 **十进制值** 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/15/graph-1.png)

```
输入：head = [1,0,1]
输出：5
解释：二进制数 (101) 转化为十进制数 (5)
```

**示例 2：**

```
输入：head = [0]
输出：0
```

**示例 3：**

```
输入：head = [1]
输出：1
```

**示例 4：**

```
输入：head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
输出：18880
```

**示例 5：**

```
输入：head = [0,0]
输出：0
```

 

**提示：**

- 链表不为空。
- 链表的结点总数不超过 `30`。
- 每个结点的值不是 `0` 就是 `1`。

题解：先用一个数组把链表中的数存储起来，然后再进行二进制到十进制转换。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int getDecimalValue(ListNode* head) {
        vector<int>num;
        while(head)
        {
            num.push_back(head->val);
            head=head->next;
        }
        int sum=0;
        for(int i=0;i<num.size();i++)
        {
            sum = sum + num[i]*pow(2,num.size()-i-1);
        }
        return sum;

    }
};
```



## [LCR 023. 相交链表](https://leetcode.cn/problems/3u1WK4/)

给定两个单链表的头节点 `headA` 和 `headB` ，请找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

 

**提示：**

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `0 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA + 1] == listB[skipB + 1]`

题解：方法一，双指针，两层循环来进行判断A和B是否有相交，记得当两个节点相交时为A==B。

方法二：采用哈希集合的方式进行处理，首先先记录A链表，然后再去遍历B链表，当存在这个哈希值时，就为true。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode *A1=headA;
        while(A1 != nullptr)
        {
            ListNode *BB=headB;
            while(BB)
            {
                if(A1 != BB)
                    BB=BB->next;
                else
                    return A1;
            }
            A1 = A1->next;
            
        }
        return nullptr;
        
    }
};
```

```
//方法二：采用哈希集合的方式进行处理，首先先记录A链表，然后再去遍历B链表，当存在这个哈希值时，就为true。
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        //方法一：采用哈希值的方式
        unordered_set<ListNode*> visited;
        ListNode *AA=headA;
        ListNode *BB=headB;
        while(AA)
        {
            visited.emplace(AA);
            AA=AA->next;
        }
        while(BB)
        {
            if(visited.count(BB))
            return BB;
            BB=BB->next;
        }
        return nullptr;	
    }
};
```



## [剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)



输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

```
0 <= 链表长度 <= 10000
```

题解：先用链表存储，再对链表使用reverse函数进行翻转。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int>list;
        while(head)
        {
            list.push_back(head->val);
            head=head->next;
        }
        reverse(list.begin(),list.end());
        return list;

    }
};
```



## [剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)



给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

**注意：**此题对比原题有改动

**示例 1:**

```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

**示例 2:**

```
输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

 

**说明：**

- 题目保证链表中节点的值互不相同
- 若使用 C 或 C++ 语言，你不需要 `free` 或 `delete` 被删除的节点

题解：两种思路，方法一：先while判断第一个节点是否是value，是就删除，然后再判断。

方法二：先添加一个头节点，然后再去判断。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        while(head != nullptr && head->val == val)
        {
            head=head->next;
            
        }
        ListNode *p=head;
        
        while(head)
        {
            if(head->next != nullptr)
            {
                if(head->next->val == val)
                {
                    ListNode *tmp = head->next;
                    head->next=head->next->next;
                    delete tmp;
                }
            }
            
            head=head->next;
        }
        return p;

    }
};
```



## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。

 

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

题解：两种思路，方法一：两次遍历进行处理。方法二：使用快慢指针，快指针比慢指针快k步。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode *p=head;
        int num=0;
        while(p)
        {
            num++;
            p=p->next;
        }
        p=head;
        int i=0;
        while(p)
        {

            if(i == (num-k))
            {
                return p;
            }
            p=p->next;
            i++;
        }
        return head;

    }
};
```

```
//快慢指针方法
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* fast = head;
        ListNode* slow = head;

        while (fast && k > 0) {
            fast = fast->next;
            k--;
        }
        while (fast) {
            fast = fast->next;
            slow = slow->next;
        }

        return slow;
    }
};
```



## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)



给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

题解：注意第22题和第19题两种的不同，本题是返回删除该节点之后的链表，22题为后面的链表。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        
        ListNode *p=head;
        int num=0;
        while(p)
        {
            num++;
            p=p->next;
        }
        p=head;

        ListNode *pp=new ListNode(0);
        pp->next = head;
        ListNode *ppp=pp;



        int num1=0;
        while(pp)
        {
            if(num1 == (num-n))
            {
                pp->next=pp->next->next;
            }
            else
            {
                pp=pp->next;
            }
            num1++;
        }
        return ppp->next;
    }
};
```



## [面试题 02.01. 移除重复节点](https://leetcode.cn/problems/remove-duplicate-node-lcci/)

编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。

**示例1:**

```
 输入：[1, 2, 3, 3, 2, 1]
 输出：[1, 2, 3]
```

**示例2:**

```
 输入：[1, 1, 1, 1, 2]
 输出：[1, 2]
```

**提示：**

1. 链表长度在[0, 20000]范围内。
2. 链表元素在[0, 20000]范围内。

**进阶：**

如果不得使用临时缓冲区，该怎么解决？



题解：



## [面试题 17.12. BiNode](https://leetcode.cn/problems/binode-lcci/)



二叉树数据结构`TreeNode`可用来表示单向链表（其中`left`置空，`right`为下一个链表节点）。实现一个方法，把二叉搜索树转换为单向链表，要求依然符合二叉搜索树的性质，转换操作应是原址的，也就是在原始的二叉搜索树上直接修改。

返回转换后的单向链表的头节点。

**注意：**本题相对原题稍作改动

 

**示例：**

```
输入： [4,2,5,1,3,null,6,0]
输出： [0,null,1,null,2,null,3,null,4,null,5,null,6]
```

**提示：**

- 节点数量不会超过 100000。





# 五、动态规划



## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)



假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 

**提示：**

- `1 <= n <= 45`

题解：**关键问题，需要写出状态转移方程**

我们用 f(x) 表示爬到第 x 级台阶的方案数，考虑最后一步可能跨了一级台阶，也可能跨了两级台阶，所以我们可以列出如下式子：

f(x)=f(x−1)+f(x−2)

它意味着爬到第 x 级台阶的方案数是爬到第 x−1 级台阶的方案数和爬到第 x−2 级台阶的方案数的和。很好理解，因为每次只能爬 1 级或 2 级，所以 f(x) 只能从 f(x−1) 和 f(x−2) 转移过来，而这里要统计方案总数，我们就需要对这两项的贡献求和。



方法二：动态规划

**动态规划思路分析** 一般看到题目**有多少种方案**，这种题意思不需要你列出来具体的多少种方案只需要计算有多少种方案，基本上就可以尝试用动态规划算法来解决问题。想好用动态规划就想想动态规划五步曲。


**算法实现步骤：**

- 确定dp数组以及下标的含义
  - 定义 dp[i]为爬上第 i 级台阶有多少种方案；
- 确定状态转移方程
  - 因为每次只可以爬 1 或者 2 个台阶所以，爬上当前台阶的方案应该是前面两个状态的方案的和即，dp[i]=dp[i−1]+dp[i−2]。
- 初始化状态
  - i=0 级开始爬的，所以从第 0 级爬到第 0 级我们可以看作只有一种方案，即 dp(0)=1；
  - i=1 代表从第 0 级到第 1 级也只有种方案，一即爬一级，dp(1)=1。
- 遍历顺序
  - 由状态转移方程知道 dp[i] 是从 dp[i−1] 和 dp[i−2] 转移过来所以从前往后遍历。
- 返回值
  - 因为一共计算 n 阶楼梯有多少方案，所以返回 dp[n]。

```
class Solution {
public:
    /* 动态规划五部曲：
     * 1.确定dp[i]的下标以及dp值的含义： 爬到第i层楼梯，有dp[i]种方法；
     * 2.确定动态规划的递推公式：dp[i] = dp[i-1] + dp[i-2];
     * 3.dp数组的初始化：因为提示中，1<=n<=45 所以初始化值，dp[1] = 1, dp[2] = 2;
     * 4.确定遍历顺序：分析递推公式可知当前值依赖前两个值来确定，所以递推顺序应该是从前往后；
     * 5.打印dp数组看自己写的对不对；
    */
    int climbStairs(int n) {
        if (n <= 1) return n;
        /* 定义dp数组 */
        vector<int> dp(n+1);
        /* 初始化dp数组 */
        dp[1] = 1;
        dp[2] = 2;
        /* 从前往后遍历 */
        for(int i = 3; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};
```





## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

题解：方法一，时间复杂度为（n*n），双层遍历非常的耗费时间，有时会超时。

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int max1 = 0;
        int max = 0;
        vector<int>nums;
        for (int i = 0; i < prices.size() - 1; i++)
        {


            for (int j = i; j < prices.size(); j++)
            {
                if (prices[i] < prices[j])
                {
                    max1 = prices[j] - prices[i];
                    if (max1 > max)
                        max = max1;
                }

            }
        }

        return max;

    }
};
```

方法二，官方解法：

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int cost = prices[0]; //成本
        int profit = 0;        //利润

        for (int price:prices)
        {
            cost=min(cost,price);
            profit=max(profit,price-cost);
        }
        return profit;
    }
};
```



## [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

 

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

 

**提示：**

- `1 <= prices.length <= 3 * 104`
- `0 <= prices[i] <= 104`

题解：

（1）方法一，采用贪心算法。

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        int profit=0;
        for(int i=1;i<n;i++)
            profit += max(0,prices[i]-prices[i-1]);
        return profit;
    }
};
```

（2）方法二，动态规划（还没搞明白）





## [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)



**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定 `n` ，请计算 `F(n)` 。

**示例 1：**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

**示例 2：**

```
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

**示例 3：**

```
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

 

**提示：**

- `0 <= n <= 30`

题解：

```
class Solution {
public:

    int fib(int n) {
        int num = 0;
        vector<int>nums;
        nums.push_back(0);
        nums.push_back(1);
        if(n>2)
        for (int i = 1; i < n; i++)
            nums.push_back(nums[i]+nums[i-1]);

        if (n==0)
            nums.pop_back();
        return nums.back();
    }
};
```

方法二：官方方法，三个变量分别存储上一次的结果

```
class Solution {
public:
    int fib(int n) {
        if (n < 2) {
            return n;
        }
        int p = 0, q = 0, r = 1;
        for (int i = 2; i <= n; ++i) {
            p = q; 
            q = r; 
            r = p + q;
        }
        return r;
    }
};
```



## [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)



给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

**示例 1：**

```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```

**示例 2：**

```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
总花费为 6 。
```

**提示：**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`

题解：采用动态规划解法，这里状态转移方程为*dp*[*i*]=min(*dp*[*i*−1]+*cost*[*i*−1],*dp*[*i*−2]+*cost*[*i*−2])；0个台阶时为0，1个台阶时也为0；

```
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector<int>dp(n+1);
        dp[0]=0;
        dp[1]=0;
        for(int i=2;i<=cost.size();i++)
        {
            dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
};
```



## [1025. 除数博弈](https://leetcode.cn/problems/divisor-game/)



爱丽丝和鲍勃一起玩游戏，他们轮流行动。爱丽丝先手开局。

最初，黑板上有一个数字 `n` 。在每个玩家的回合，玩家需要执行以下操作：

- 选出任一 `x`，满足 `0 < x < n` 且 `n % x == 0` 。
- 用 `n - x` 替换黑板上的数字 `n` 。

如果玩家无法执行这些操作，就会输掉游戏。

*只有在爱丽丝在游戏中取得胜利时才返回 `true` 。假设两个玩家都以最佳状态参与游戏。*

**示例 1：**

```
输入：n = 2
输出：true
解释：爱丽丝选择 1，鲍勃无法进行操作。
```

**示例 2：**

```
输入：n = 3
输出：false
解释：爱丽丝选择 1，鲍勃也选择 1，然后爱丽丝无法进行操作。
```

 

**提示：**

- `1 <= n <= 1000`

题解：





## [1137. 第 N 个泰波那契数](https://leetcode.cn/problems/n-th-tribonacci-number/)

泰波那契序列 Tn 定义如下： 

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 `n`，请返回第 n 个泰波那契数 Tn 的值。

 

**示例 1：**

```
输入：n = 4
输出：4
解释：
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```

**示例 2：**

```
输入：n = 25
输出：1389537
```

**提示：**

- `0 <= n <= 37`
- 答案保证是一个 32 位整数，即 `answer <= 2^31 - 1`。

题解：其实这道题就是Tn = Tn-3 + Tn-2 + Tn-1

```
class Solution {
public:
    int tribonacci(int n) {
        if(n==0)
            return 0;
        else if(n==1)
            return 1;
        else if(n==2)
            return 1;
        vector<int>nn(n+1);
        nn[0]=0;
        nn[1]=1;
        nn[2]=1;
        for(int i=3;i<=n;i++)
        {
            nn[i]=nn[i-3]+nn[i-2]+nn[i-1];
        }
        
        return nn.back();

    }
};
```



## [1646. 获取生成数组中的最大值](https://leetcode.cn/problems/get-maximum-in-generated-array/)

给你一个整数 `n` 。按下述规则生成一个长度为 `n + 1` 的数组 `nums` ：

- `nums[0] = 0`
- `nums[1] = 1`
- 当 `2 <= 2 * i <= n` 时，`nums[2 * i] = nums[i]`
- 当 `2 <= 2 * i + 1 <= n` 时，`nums[2 * i + 1] = nums[i] + nums[i + 1]`

返回生成数组 `nums` 中的 **最大** 值。

 

**示例 1：**

```
输入：n = 7
输出：3
解释：根据规则：
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
因此，nums = [0,1,1,2,1,3,2,3]，最大值 3
```

**示例 2：**

```
输入：n = 2
输出：1
解释：根据规则，nums[0]、nums[1] 和 nums[2] 之中的最大值是 1
```

**示例 3：**

```
输入：n = 3
输出：2
解释：根据规则，nums[0]、nums[1]、nums[2] 和 nums[3] 之中的最大值是 2
```

**提示：**

- `0 <= n <= 100`

题解：

```
class Solution {
public:
    int getMaximumGenerated(int n) {
        if(n==0)
            return 0;
        else if(n==1)
            return 1;
        int maxnum=0;
        vector<int>nn(n+1);
        nn[0]=0;
        nn[1]=1;
        for(int i=2;i<=n;i++)
        {
            if(i%2==0)
                nn[i]=nn[i/2];
            else
                nn[i]=nn[i/2]+nn[i/2+1];
            if(nn[i]>maxnum)
                maxnum=nn[i];
        }
        return maxnum;

    }
};
```



## [面试题 08.01. 三步问题](https://leetcode.cn/problems/three-steps-problem-lcci/)

三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。

**示例1:**

```
 输入：n = 3 
 输出：4
 说明: 有四种走法
```

**示例2:**

```
 输入：n = 5
 输出：13
```

**提示:**

1. n范围在[1, 1000000]之间

题解：这里首先需要需要计算出整个转移状态方程。

楼梯有：n=1，2，3，4。。。

结果有：1，2，4，7  可以发现这里f(i4)=f(i1)+f(i2)+f(i3)。所以就很好计算了。

注意这里需要取模，是因为整个结果可能很大，所以需要取模计算。如果不想每次都取模那么就将类型定义为long int。或者两个数字相加之后先取一次模再与第三个数相加。

```
vector<long int>dp(n+1,0);   //定义为long int类型
```

```
dp[i] = ((dp[i - 1] + dp[i - 2]) % 1000000007 + dp[i - 3]) % 1000000007; //定义为int类型数组，但是两个数先相加取模再相加第三个数。
```

```
class Solution {
public:
    int waysToStep(int n) {
        if(n<3)
            return n;
        if(n==3)
            return 4;
        vector<long int>dp(n+1,0);
        dp[1]=1;
        dp[2]=2;
        dp[3]=4;
        for(int i=4;i<=n;i++)
        {
            dp[i]=(dp[i-1]%1000000007+dp[i-2]%1000000007+dp[i-3]%1000000007)%1000000007;
        }
            
        return dp.back();
    }
};
```



## [面试题 16.17. 连续数列](https://leetcode.cn/problems/contiguous-sequence-lcci/)

给定一个整数数组，找出总和最大的连续数列，并返回总和。

**示例：**

```
输入： [-2,1,-3,4,-1,2,1,-5,4]
输出： 6
解释： 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**进阶：**

如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

题解：

![image-20231008201201372](C:\Users\lei\AppData\Roaming\Typora\typora-user-images\image-20231008201201372.png)

初始数组：-2，1，-3，4，-1，2，1，-5，4

相加后结果：-2，1，-2，4，3，5，6，1，5

这里结果为6的最大，则选择【4，-1，2，1】

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre=0;
        int maxnum=nums[0];
        for(int x:nums)
        {
            pre=max(pre+x,x);
            maxnum=max(maxnum,pre);
        }
        return maxnum;
    }
};
```





# 五 动态规划（中等题）



## [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

题解：使用动态规划的方法，需要知道状态转移方程。（官方方法）



```
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n=0,m=0;
        n=grid.size();
        m=grid[0].size();
        if(n==0)
            return 0;
        vector<vector<int>>dp(n,vector<int>(m));
        dp[0][0]=grid[0][0];
        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++)
            {
                if(i>0 && j==0)
                    dp[i][0]=dp[i-1][0]+grid[i][0];
                else if(i==0 && j>0)
                    dp[i][j]=dp[0][j-1]+grid[i][j];
                else if(i>0 && j>0)
                    dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        return dp[n-1][m-1];

    }
};
```



## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

**提示：**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

题解：我们只要

（1）贪心算法

```
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxreach=0;
        for(int i=0;i<nums.size();i++)
        {
            if(i>maxreach)
                return false;
            maxreach=max(i+nums[i],maxreach);
        }
        return true;
    }
};
```



## [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `nums[n-1]`

题解：这里题目是保证了可以跳到最后的，这个不需要我们判断。

这里还是按照跳跃游戏1的方式进行判断，但是最后一步是不用判断的。

```
class Solution {
public:
    int jump(vector<int>& nums) {
        int maxnum=0;
        int num=0;
        int end=0;
        for(int i=0;i<nums.size()-1;i++)
        {           
            maxnum=max(i+nums[i],maxnum);
            if(i==end)
            {
                end=maxnum;
                num++;
            } 
        }
        return num;
    }
};
```



## [62. 不同路径](https://leetcode.cn/problems/unique-paths/)



一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

 

**提示：**

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 109`

题解：

动态规划，转移状态方程：dp[i] [j]=dp[i-1] [j] + dp[i] [j-1]；另外最上边和最左侧都只有一种走法，所以dp都为1。

![image-20231008224525556](C:\Users\lei\AppData\Roaming\Typora\typora-user-images\image-20231008224525556.png)

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>>dp(m,vector<int>(n));
        for(int i=0;i<m;i++)
            dp[i][0]=1;
        for(int j=0;j<n;j++)
            dp[0][j]=1;
        for(int i=1;i<m;i++)
            for(int j=1;j<n;j++)
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
        return dp[m-1][n-1];
    }
};
```



## [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

 

**提示：**

- `m == obstacleGrid.length`
- `n == obstacleGrid[i].length`
- `1 <= m, n <= 100`
- `obstacleGrid[i][j]` 为 `0` 或 `1`

题解：

**思路分析** 用什么方法应该不用多解释了，一般路径问题求解有多少中方案肯定用动态规划的解法，但是要注意本题中加了干扰相当于有些路径是不能走的就是那些坐标值为1的路是不能走的。 算法

- 确定dp数组以及下标的含义
  - dp(i,j) 表示从坐标 (0,0) 到坐标 (i,j) 的路径总数
  - u(i,j) 表示坐标 (i,j) 是否有障碍物，如果坐标 (i,j) 有障碍物，u(i,j)=1，否则 u(i, j) = 0。
  - 注意一下范围 i 在[0, m), j 在[0, n);
- 确定状态转移方程
  - 因为机器人每次只能向下或者向右移动一步，所以从坐标 (0,0) 到坐标 (i,j) 的路径总数的值只取决于从坐标 (0,0) 到坐标 (i−1,j) 的路径总数和从坐标 (0,0) 到坐标(i,j−1) 的路径总数，即 dp(i,j) 只能通过 dp(i−1,j) 和 dp(i,j−1) 转移得到。
  - 当坐标 (i,j) 本身有障碍的时候，任何路径都到到不了 dp(i,j)，此时 dp(i, j) = 0；
    - u(i,j) = 1 时dp[i] [j] = 0;
    - u(i,j) != 1 时dp[i][j] = dp[i-1] [j] + dp[i] [j-1];;
- 初始化状态
  - 如果 i = 0 相当于在没有障碍物的时候只有一行的路径所以dp[0] [j] = 1;
  - 如果 j = 0 相当于在没有障碍物的时候只有一列的路径所以dp[i] [0] = 1;
- 遍历顺序
  - 由状态转移方程知道dp[i] 是从 dp[i−1,j] 和 dp[i,j−1] 转移过来所以从前往后遍历。
- 返回值
  - 最终的答案即为dp(m−1,n−1)。

```
class Solution {
public:
    /**
     * 1. 确定dp数组下标含义 dp[i][j] 从(0,0)到(i,j)可能的路径种类;
     * 2. 递推公式 dp[i][j] = dp[i-1][j] + dp[i][j-1] 但是需要加限制条件就是没有障碍物的时候
     *    if(obstacleGrid[i][j] == 0) dp[i][j] = dp[i-1][j] + dp[i][j-1];
     * 3. 初始化 当obstacleGrid[i][j] == 0时，dp[i][0]=1 dp[0][i]=1 初始化横竖就可;
     * 4. 遍历顺序 一行一行遍历;
     * 5. 推导结果;
     */
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        /* 计算数组大小 */
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        /* 定义dp数组 */
        vector<vector<int>> dp(m,vector<int>(n,0));
        /* 初始化dp数组 */
        for(int i = 0; i < m && obstacleGrid[i][0] == 0; i++)
            dp[i][0] = 1; 
        for(int i = 0; i < n && obstacleGrid[0][i] == 0; i++)
            dp[0][i] = 1; 
        /* 一行一行遍历 */
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                /* 去除障碍物 */
                if(obstacleGrid[i][j] == 1) continue;
                
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```



# 六、图论



## [997. 找到小镇的法官](https://leetcode.cn/problems/find-the-town-judge/)

小镇里有 `n` 个人，按从 `1` 到 `n` 的顺序编号。传言称，这些人中有一个暗地里是小镇法官。

如果小镇法官真的存在，那么：

1. 小镇法官不会信任任何人。
2. 每个人（除了小镇法官）都信任这位小镇法官。
3. 只有一个人同时满足属性 **1** 和属性 **2** 。

给你一个数组 `trust` ，其中 `trust[i] = [ai, bi]` 表示编号为 `ai` 的人信任编号为 `bi` 的人。

如果小镇法官存在并且可以确定他的身份，请返回该法官的编号；否则，返回 `-1` 。

**示例 1：**

```
输入：n = 2, trust = [[1,2]]
输出：2
```

**示例 2：**

```
输入：n = 3, trust = [[1,3],[2,3]]
输出：3
```

**示例 3：**

```
输入：n = 3, trust = [[1,3],[2,3],[3,1]]
输出：-1
```

**提示：**

- `1 <= n <= 1000`
- `0 <= trust.length <= 104`
- `trust[i].length == 2`
- `trust` 中的所有`trust[i] = [ai, bi]` **互不相同**
- `ai != bi`
- `1 <= ai, bi <= n`











## [1791. 找出星型图的中心节点](https://leetcode.cn/problems/find-center-of-star-graph/)

有一个无向的 **星型** 图，由 `n` 个编号从 `1` 到 `n` 的节点组成。星型图有一个 **中心** 节点，并且恰有 `n - 1` 条边将中心节点与其他每个节点连接起来。

给你一个二维整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示在节点 `ui` 和 `vi` 之间存在一条边。请你找出并返回 `edges` 所表示星型图的中心节点。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/14/star_graph.png)

```
输入：edges = [[1,2],[2,3],[4,2]]
输出：2
解释：如上图所示，节点 2 与其他每个节点都相连，所以节点 2 是中心节点。
```

**示例 2：**

```
输入：edges = [[1,2],[5,1],[1,3],[1,4]]
输出：1
```

 

**提示：**

- `3 <= n <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- 题目数据给出的 `edges` 表示一个有效的星型图

题解：

方法一：使用map哈希表来统计每个数组中出现次数最多的那个数，就是中点（因为题目保证了是有中心点的）。

```
class Solution {
public:
    int findCenter(vector<vector<int>>& edges) {
        map<int,int>nn;
        for(int i=0;i<edges.size();i++)
        {
            nn[edges[i][0]]++;
            nn[edges[i][1]]++;
        }
        int num,maxnum=0;
        for(auto item=nn.begin();item != nn.end(); item++)
        {
            if(item->second > maxnum)
            {
                num=item->first;
                maxnum=item->second;
            }
        }
        return num;
    }
};
```



## [1971. 寻找图中是否存在路径](https://leetcode.cn/problems/find-if-path-exists-in-graph/)

有一个具有 `n` 个顶点的 **双向** 图，其中每个顶点标记从 `0` 到 `n - 1`（包含 `0` 和 `n - 1`）。图中的边用一个二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由 **最多一条** 边连接，并且没有顶点存在与自身相连的边。

请你确定是否存在从顶点 `source` 开始，到顶点 `destination` 结束的 **有效路径** 。

给你数组 `edges` 和整数 `n`、`source` 和 `destination`，如果从 `source` 到 `destination` 存在 **有效路径** ，则返回 `true`，否则返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```
输入：n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
输出：true
解释：存在由顶点 0 到顶点 2 的路径:
- 0 → 1 → 2 
- 0 → 2
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

 

**提示：**

- `1 <= n <= 2 * 105`
- `0 <= edges.length <= 2 * 105`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- 不存在重复边
- 不存在指向顶点自身的边

题解：

本来是想使用键值对map来进行判断的，后面才意识到map只能存储一个【0，1】【0，2】，那么只会保存一个，并且也得保证有【0，1】也得有【1，0】的存在。



# 七、二叉树



## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)



给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

 

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

 

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

题解：

（1）递归法：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void inorder(TreeNode* root, vector<int>&res)
    {
        if(!root)
            return;
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int>res;
        inorder(root, res);
        return res;
    }
};
```

（2）迭代法：



## [100. 相同的树](https://leetcode.cn/problems/same-tree/)

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

```
输入：p = [1,2], q = [1,null,2]
输出：false
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

 

**提示：**

- 两棵树上的节点数目都在范围 `[0, 100]` 内
- `-104 <= Node.val <= 104`







# 数据库

关键词

### 1、distinct



### 2、 order by

[SQL ORDER BY 关键字 | 菜鸟教程 (runoob.com)](https://www.runoob.com/sql/sql-orderby.html)

### 3、cross join

[[SQL快速入门-38\] SQL CROSS JOIN：交叉连接 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/604191058)

### 4、group by

[SQL GROUP BY 语句 (w3school.com.cn)](https://www.w3school.com.cn/sql/sql_groupby.asp)

### 5、datediff函数

[DATEDIFF() 函数—— 计算时间差_datediff计算小时差-CSDN博客](https://blog.csdn.net/qq_34498806/article/details/85614472)

### 6、round（）函数

[SQL ROUND() 函数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/sql/sql-func-round.html)

### 7、if语句

[sql中的 IF 条件语句的用法_sql写if语句-CSDN博客](https://blog.csdn.net/qq_36850813/article/details/80449860)

### 8、where in 和 Not in



### 9、like字符匹配

[查看学校名称中含北京的用户_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/95d9922b1e2a49de80daa491889969ee?tpId=199&tqId=1971219&ru=%2Fexam%2Fintelligent&qru=%2Fta%2Fsql-quick-study%2Fquestion-ranking&sourceUrl=%2Fexam%2Fintelligent%3FquestionJobId%3D10%26tagId%3D146746)

### 10、group by 之后的having 

[MySQL查询中having语句的使用场景和用法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/107495600)

[SQL HAVING 语句 (w3school.com.cn)](https://www.w3school.com.cn/sql/sql_having.asp)

HAVING子句用于对分组后的结果再进行过滤，它的功能有点像WHERE子句，但它用于组而不是单个记录。
**在HAVING子句中可以使用统计函数，但在WHERE子句中则不能。
HAVING通常与GROUP BY子句一起使用。**

### 11、join 或者 left join...

- JOIN: 如果表中有至少一个匹配，则返回行
- LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行
- RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行
- FULL JOIN: 只要其中一个表中存在匹配，就返回行

### 12、union all

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

[SQL UNION 和 UNION ALL 操作符 (w3school.com.cn)](https://www.w3school.com.cn/sql/sql_union.asp)

### 13、case函数

```
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

[SQL CASE 语句 (w3schools.cn)](https://www.w3schools.cn/sql/sql_case.asp)



### 14、substring_index(str,delim,count)

![图片说明](https://uploadfiles.nowcoder.com/images/20210928/522911501_1632792502086/4EF7B5DAE69AD54DE81C49D58020D7A4)

### 15、











## [175. 组合两个表](https://leetcode.cn/problems/combine-two-tables/)

------

表: `Person`

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
personId 是该表的主键（具有唯一值的列）。
该表包含一些人的 ID 和他们的姓和名的信息。
```

表: `Address`

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
addressId 是该表的主键（具有唯一值的列）。
该表的每一行都包含一个 ID = PersonId 的人的城市和州的信息。
```

编写解决方案，报告 `Person` 表中每个人的姓、名、城市和州。如果 `personId` 的地址不在 `Address` 表中，则报告为 `null` 。

以 **任意顺序** 返回结果表。

结果格式如下所示。

**示例 1:**

```
输入: 
Person表:
+----------+----------+-----------+
| personId | lastName | firstName |
+----------+----------+-----------+
| 1        | Wang     | Allen     |
| 2        | Alice    | Bob       |
+----------+----------+-----------+
Address表:
+-----------+----------+---------------+------------+
| addressId | personId | city          | state      |
+-----------+----------+---------------+------------+
| 1         | 2        | New York City | New York   |
| 2         | 3        | Leetcode      | California |
+-----------+----------+---------------+------------+
输出: 
+-----------+----------+---------------+----------+
| firstName | lastName | city          | state    |
+-----------+----------+---------------+----------+
| Allen     | Wang     | Null          | Null     |
| Bob       | Alice    | New York City | New York |
+-----------+----------+---------------+----------+
解释: 
地址表中没有 personId = 1 的地址，所以它们的城市和州返回 null。
addressId = 1 包含了 personId = 2 的地址信息。
```

题解：

```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId;
```



## [1757. 可回收且低脂的产品](https://leetcode.cn/problems/recyclable-and-low-fat-products/)

表：`Products`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
product_id 是该表的主键（具有唯一值的列）。
low_fats 是枚举类型，取值为以下两种 ('Y', 'N')，其中 'Y' 表示该产品是低脂产品，'N' 表示不是低脂产品。
recyclable 是枚举类型，取值为以下两种 ('Y', 'N')，其中 'Y' 表示该产品可回收，而 'N' 表示不可回收。
```

 

编写解决方案找出既是低脂又是可回收的产品编号。

返回结果 **无顺序要求** 。

返回结果格式如下例所示：

**示例 1：**

```
输入：
Products 表：
+-------------+----------+------------+
| product_id  | low_fats | recyclable |
+-------------+----------+------------+
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |
+-------------+----------+------------+
输出：
+-------------+
| product_id  |
+-------------+
| 1           |
| 3           |
+-------------+
解释：
只有产品 id 为 1 和 3 的产品，既是低脂又是可回收的产品。
```

题解：

```
# Write your MySQL query statement below
select product_id from Products where low_fats='Y' AND recyclable='Y'
```

## [584. 寻找用户推荐人](https://leetcode.cn/problems/find-customer-referee/)

表: `Customer`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
在 SQL 中，id 是该表的主键列。
该表的每一行表示一个客户的 id、姓名以及推荐他们的客户的 id。
```

找出那些 **没有被** `id = 2` 的客户 **推荐** 的客户的姓名。

以 **任意顺序** 返回结果表。

结果格式如下所示。

 

**示例 1：**

```
输入： 
Customer 表:
+----+------+------------+
| id | name | referee_id |
+----+------+------------+
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |
+----+------+------------+
输出：
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```

题解：

```
# Write your MySQL query statement below
select name from Customer where referee_id != 2 OR referee_id is NULL;
```



## [595. 大的国家](https://leetcode.cn/problems/big-countries/)

`World` 表：

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
name 是该表的主键（具有唯一值的列）。
这张表的每一行提供：国家名称、所属大陆、面积、人口和 GDP 值。
```

如果一个国家满足下述两个条件之一，则认为该国是 **大国** ：

- 面积至少为 300 万平方公里（即，`3000000 km2`），或者
- 人口至少为 2500 万（即 `25000000`）

编写解决方案找出 **大国** 的国家名称、人口和面积。

按 **任意顺序** 返回结果表。

返回结果格式如下例所示。

 

**示例：**

```
输入：
World 表：
+-------------+-----------+---------+------------+--------------+
| name        | continent | area    | population | gdp          |
+-------------+-----------+---------+------------+--------------+
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |
+-------------+-----------+---------+------------+--------------+
输出：
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan | 25500100   | 652230  |
| Algeria     | 37100000   | 2381741 |
+-------------+------------+---------+
```

题解：

```
# Write your MySQL query statement below
select name,population,area from World where area >= 3000000 OR population >= 25000000;
```

## [1148. 文章浏览 I](https://leetcode.cn/problems/article-views-i/)

`Views` 表：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
此表可能会存在重复行。（换句话说，在 SQL 中这个表没有主键）
此表的每一行都表示某人在某天浏览了某位作者的某篇文章。
请注意，同一人的 author_id 和 viewer_id 是相同的。
```

 

请查询出所有浏览过自己文章的作者

结果按照 `id` 升序排列。

查询结果的格式如下所示：

**示例 1：**

```
输入：
Views 表：
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+

输出：
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
```

题解：这里有三个问题，（1）需要将输出不论是author_id还是viewer_id改为id的名称（As id）。

（2）这里author_id=4有两次记录，但是输出时只需要一次就行，需要DISTINCT 关键字来从表 Views 中检索唯一元素。

（3）需要将id列按照升序进行排列，使用ORDER BY 关键字实现。

```
# Write your MySQL query statement below
select DISTINCT author_id As id from Views where author_id=viewer_id  ORDER BY id;
```

## [1683. 无效的推文](https://leetcode.cn/problems/invalid-tweets/)

表：`Tweets`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
在 SQL 中，tweet_id 是这个表的主键。
这个表包含某社交媒体 App 中所有的推文。
```

 

查询所有无效推文的编号（ID）。当推文内容中的字符数**严格大于** `15` 时，该推文是无效的。

以**任意顺序**返回结果表。

查询结果格式如下所示：

**示例 1：**

```
输入：
Tweets 表：
+----------+----------------------------------+
| tweet_id | content                          |
+----------+----------------------------------+
| 1        | Vote for Biden                   |
| 2        | Let us make America great again! |
+----------+----------------------------------+

输出：
+----------+
| tweet_id |
+----------+
| 2        |
+----------+
解释：
推文 1 的长度 length = 14。该推文是有效的。
推文 2 的长度 length = 32。该推文是无效的。
```

题解：这里用于计算字符串中字符数的最佳函数是 [CHAR_LENGTH(str)](https://leetcode.cn/link/?target=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F5.7%2Fen%2Fstring-functions.html%23function_char-length)，它返回字符串 `str` 的长度。

另一个常用的函数 LENGTH(str) 在这个问题中也适用，因为列 content 只包含英文字符，没有特殊字符。否则，LENGTH() 可能会返回不同的结果，因为该函数返回字符串 str 的字节数，某些字符包含多于 1 个字节。

以字符 '¥' 为例：CHAR_LENGTH() 返回结果为 1，而 LENGTH() 返回结果为 2，因为该字符串包含 2 个字节。

```
# Write your MySQL query statement below
select tweet_id from tweets where CHAR_LENGTH(content)>15;
```



## [1378. 使用唯一标识码替换员工ID](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/)

`Employees` 表：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
在 SQL 中，id 是这张表的主键。
这张表的每一行分别代表了某公司其中一位员工的名字和 ID 。
```

`EmployeeUNI` 表：

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
在 SQL 中，(id, unique_id) 是这张表的主键。
这张表的每一行包含了该公司某位员工的 ID 和他的唯一标识码（unique ID）。
```

 

展示每位用户的 **唯一标识码（unique ID ）**；如果某位员工没有唯一标识码，使用 null 填充即可。

你可以以 **任意** 顺序返回结果表。

返回结果的格式如下例所示。

 

**示例 1：**

```
输入：
Employees 表:
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
EmployeeUNI 表:
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
输出：
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
解释：
Alice and Bob 没有唯一标识码, 因此我们使用 null 替代。
Meir 的唯一标识码是 2 。
Winston 的唯一标识码是 3 。
Jonathan 唯一标识码是 1 。
```

题解：

（1）我们首先执行LEFT JOIN操作，将两个表的数据基于 id 列进行组合。同样，我们使用 LEFT JOIN 来确保将所有 Employees 表中的行都包含在结果中，即使在 EmployeeUNI 表中没有匹配的行。

```
SELECT 
    * 
FROM
    Employees 
LEFT JOIN 
    EmployeeUNI 
ON 
    Employees.id = EmployeeUNI.id;
```

id	name	id	unique_id
1	Alice	null	null
7	Bob	null	null
11	Meir	11	2
90	Winston	90	3
3	Jonathan	3	1
（2）由于我们想要从组合表中检索列 unique_id 和 name ，所以我们将从 EmployeeUNI 表选择 unique_id 列，从 Employees 表选择 name 列。完整代码如下：

```
SELECT 
    EmployeeUNI.unique_id, Employees.name
FROM 
    Employees
LEFT JOIN 
    EmployeeUNI 
ON 
    Employees.id = EmployeeUNI.id;
```



## [1068. 产品销售分析 I](https://leetcode.cn/problems/product-sales-analysis-i/)

销售表 `Sales`：

```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) 是销售表 Sales 的主键（具有唯一值的列的组合）。
product_id 是关联到产品表 Product 的外键（reference 列）。
该表的每一行显示 product_id 在某一年的销售情况。
注意: price 表示每单位价格。
```

产品表 `Product`：

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id 是表的主键（具有唯一值的列）。
该表的每一行表示每种产品的产品名称。
```

 

编写解决方案，以获取 `Sales` 表中所有 `sale_id` 对应的 `product_name` 以及该产品的所有 `year` 和 `price` 。

返回结果表 **无顺序要求** 。

结果格式示例如下。

 

**示例 1：**

```
输入：
Sales 表：
+---------+------------+------+----------+-------+
| sale_id | product_id | year | quantity | price |
+---------+------------+------+----------+-------+ 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |
+---------+------------+------+----------+-------+
Product 表：
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |
+------------+--------------+
输出：
+--------------+-------+-------+
| product_name | year  | price |
+--------------+-------+-------+
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |
+--------------+-------+-------+
```

题解：

正确答案：

```
# Write your MySQL query statement below
select product_name,year,price from Sales LEFT JOIN Product ON Sales.product_id=product.product_id;
```

这里需要注意，不能是下面的写法，这样会报错。

```
# Write your MySQL query statement below
select product_name,year,price from Product LEFT JOIN Sales ON Sales.product_id=product.product_id;
```



## [1581. 进店却未进行过交易的顾客](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/)

表：`Visits`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
visit_id 是该表中具有唯一值的列。
该表包含有关光临过购物中心的顾客的信息。
```

表：`Transactions`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
transaction_id 是该表中具有唯一值的列。
此表包含 visit_id 期间进行的交易的信息。
```

 

有一些顾客可能光顾了购物中心但没有进行交易。请你编写一个解决方案，来查找这些顾客的 ID ，以及他们只光顾不交易的次数。

返回以 **任何顺序** 排序的结果表。

返回结果格式如下例所示。

**示例 1：**

```
输入:
Visits
+----------+-------------+
| visit_id | customer_id |
+----------+-------------+
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |
+----------+-------------+
Transactions
+----------------+----------+--------+
| transaction_id | visit_id | amount |
+----------------+----------+--------+
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |
+----------------+----------+--------+
输出:
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------+
解释:
ID = 23 的顾客曾经逛过一次购物中心，并在 ID = 12 的访问期间进行了一笔交易。
ID = 9 的顾客曾经逛过一次购物中心，并在 ID = 13 的访问期间进行了一笔交易。
ID = 30 的顾客曾经去过购物中心，并且没有进行任何交易。
ID = 54 的顾客三度造访了购物中心。在 2 次访问中，他们没有进行任何交易，在 1 次访问中，他们进行了 3 次交易。
ID = 96 的顾客曾经去过购物中心，并且没有进行任何交易。
如我们所见，ID 为 30 和 96 的顾客一次没有进行任何交易就去了购物中心。顾客 54 也两次访问了购物中心并且没有进行任何交易。
```

题解：



```
# Write your MySQL query statement below
select customer_id,count(customer_id) as count_no_trans from Visits LEFT JOIN Transactions ON Visits.visit_id=Transactions.visit_id where amount IS NULL GROUP BY customer_id; 
```

```
# Write your MySQL query statement below
SELECT 
    customer_id, COUNT(customer_id) count_no_trans
FROM 
    visits v
LEFT JOIN 
    transactions t ON v.visit_id = t.visit_id
WHERE amount IS NULL
GROUP BY customer_id;
```



## [197. 上升的温度](https://leetcode.cn/problems/rising-temperature/)

表： `Weather`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id 是该表具有唯一值的列。
该表包含特定日期的温度信息
```

 

编写解决方案，找出与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 **无顺序要求** 。

结果格式如下例子所示。

 

**示例 1：**

```
输入：
Weather 表：
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
输出：
+----+
| id |
+----+
| 2  |
| 4  |
+----+
解释：
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（20 -> 30）
```

题解：

datediff函数的用法，DATEDIFF(datepart,startdate,enddate)

select DATEDIFF(year,'2010-12-31','2019-01-01')    返回 9

select DATEDIFF(Month,'2018-01-01','2019-01-01')    返回 12

select DATEDIFF(week,'2018-12-01','2018-12-31')     返回 5

```
# Write your MySQL query statement below
select w1.id from Weather w1,Weather w2 
where datediff(w1.recordDate,w2.recordDate)=1 AND w1.Temperature>w2.Temperature; 
```



## [1661. 每台机器的进程平均运行时间](https://leetcode.cn/problems/average-time-of-process-per-machine/)

表: `Activity`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
该表展示了一家工厂网站的用户活动。
(machine_id, process_id, activity_type) 是当前表的主键（具有唯一值的列的组合）。
machine_id 是一台机器的ID号。
process_id 是运行在各机器上的进程ID号。
activity_type 是枚举类型 ('start', 'end')。
timestamp 是浮点类型,代表当前时间(以秒为单位)。
'start' 代表该进程在这台机器上的开始运行时间戳 , 'end' 代表该进程在这台机器上的终止运行时间戳。
同一台机器，同一个进程都有一对开始时间戳和结束时间戳，而且开始时间戳永远在结束时间戳前面。
```

 

现在有一个工厂网站由几台机器运行，每台机器上运行着 **相同数量的进程** 。编写解决方案，计算每台机器各自完成一个进程任务的平均耗时。

完成一个进程任务的时间指进程的`'end' 时间戳` 减去 `'start' 时间戳`。平均耗时通过计算每台机器上所有进程任务的总耗费时间除以机器上的总进程数量获得。

结果表必须包含`machine_id（机器ID）` 和对应的 **average time（平均耗时）** 别名 `processing_time`，且**四舍五入保留3位小数。**

以 **任意顺序** 返回表。

具体参考例子如下。

 

**示例 1:**

```
输入：
Activity table:
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
输出：
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------+
解释：
一共有3台机器,每台机器运行着两个进程.
机器 0 的平均耗时: ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
机器 1 的平均耗时: ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
机器 2 的平均耗时: ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456
```

题解：

方法一：将表分成两个表a1，a2，然后进行整理成a3，利用a3显示出整个结果。

```
select a3.machine_id,ROUND(avg(a3.processTime),3) AS processing_time from 
(select a1.machine_id,(a2.timestamp-a1.timestamp) AS processTime
from Activity a1, Activity a2
where a1.machine_id=a2.machine_id AND a1.process_id=a2.process_id AND a1.activity_type='start'
AND a2.activity_type='end') AS a3
GROUP BY a3.machine_id;
```

上面的方法比较抽象，可以理解下面的

```
select a1.machine_id machine_id, 
    round(sum(a2.timestamp-a1.timestamp)/count(*), 3) processing_time 
from Activity a1, Activity a2
where a1.machine_id = a2.machine_id and a1.process_id = a2.process_id
    and a1.activity_type = 'start' and a2.activity_type = 'end'
group by machine_id
```

方法二：是利用if语句进行判断

```
select
    machine_id,
    round(avg(if(activity_type='start', -timestamp, timestamp))*2, 3) processing_time 
from Activity 
group by machine_id
```



## [577. 员工奖金](https://leetcode.cn/problems/employee-bonus/)

表：`Employee` 

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
empId 是该表中具有唯一值的列。
该表的每一行都表示员工的姓名和 id，以及他们的工资和经理的 id。
```

 

表：`Bonus`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
empId 是该表具有唯一值的列。
empId 是 Employee 表中 empId 的外键(reference 列)。
该表的每一行都包含一个员工的 id 和他们各自的奖金。
```

 

编写解决方案，报告每个奖金 **少于** `1000` 的员工的姓名和奖金数额。

以 **任意顺序** 返回结果表。

结果格式如下所示。

 

**示例 1：**

```
输入：
Employee table:
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
Bonus table:
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
输出：
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
```

题解：

```
select name,bonus from Employee LEFT JOIN Bonus ON Employee.empID=Bonus.empID where bonus<1000 OR bonus IS NULL;
```





## [1280. 学生们参加各科测试的次数](https://leetcode.cn/problems/students-and-examinations/)

学生表: `Students`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
在 SQL 中，主键为 student_id（学生ID）。
该表内的每一行都记录有学校一名学生的信息。
```

科目表: `Subjects`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
在 SQL 中，主键为 subject_name（科目名称）。
每一行记录学校的一门科目名称。
```

 

考试表: `Examinations`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
这个表可能包含重复数据（换句话说，在 SQL 中，这个表没有主键）。
学生表里的一个学生修读科目表里的每一门科目。
这张考试表的每一行记录就表示学生表里的某个学生参加了一次科目表里某门科目的测试。
```

 

查询出每个学生参加每一门科目测试的次数，结果按 `student_id` 和 `subject_name` 排序。

查询结构格式如下所示。

 

**示例 1：**

```
输入：
Students table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
Subjects table:
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
Examinations table:
+------------+--------------+
| student_id | subject_name |
+------------+--------------+
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
+------------+--------------+
输出：
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
解释：
结果表需包含所有学生和所有科目（即便测试次数为0）：
Alice 参加了 3 次数学测试, 2 次物理测试，以及 1 次编程测试；
Bob 参加了 1 次数学测试, 1 次编程测试，没有参加物理测试；
Alex 啥测试都没参加；
John  参加了数学、物理、编程测试各 1 次。
```

题解：

1.先连结两个表：对表Students和表Subjects求笛卡尔积，得到4*3行的表

```
SELECT *
FROM Students a CROSS JOIN Subjects b
```

| student_id | student_name | subject_name |
| ---------- | ------------ | ------------ |
| 1          | Alice        | Programming  |
| 1          | Alice        | Physics      |
| 1          | Alice        | Math         |
| 2          | Bob          | Programming  |
| 2          | Bob          | Physics      |
| 2          | Bob          | Math         |
| 13         | John         | Programming  |
| 13         | John         | Physics      |
| 13         | John         | Math         |
| 6          | Alex         | Programming  |
| 6          | Alex         | Physics      |
| 6          | Alex         | Math         |

2.再连结三个表：对刚刚得到的新表和表Examinations进行左连结（LEFT JOIN）

```
SELECT *
FROM Students a CROSS JOIN Subjects b
    LEFT JOIN Examinations e ON a.student_id = e.student_id AND b.subject_name = e.subject_name
```

| student_id | student_name | subject_name | student_id | subject_name |
| ---------- | ------------ | ------------ | ---------- | ------------ |
| 1          | Alice        | Programming  | 1          | Programming  |
| 1          | Alice        | Physics      | 1          | Physics      |
| 1          | Alice        | Physics      | 1          | Physics      |
| 1          | Alice        | Math         | 1          | Math         |
| 1          | Alice        | Math         | 1          | Math         |
| 1          | Alice        | Math         | 1          | Math         |
| 2          | Bob          | Programming  | 2          | Programming  |
| 2          | Bob          | Physics      | null       | null         |
| 2          | Bob          | Math         | 2          | Math         |
| 13         | John         | Programming  | 13         | Programming  |
| 13         | John         | Physics      | 13         | Physics      |
| 13         | John         | Math         | 13         | Math  ...    |

3.查询

```
SELECT a.student_id, a.student_name, b.subject_name, COUNT(e.subject_name) AS attended_exams
FROM Students a CROSS JOIN Subjects b
    LEFT JOIN Examinations e ON a.student_id = e.student_id AND b.subject_name = e.subject_name
GROUP BY a.student_id, b.subject_name
ORDER BY a.student_id, b.subject_name
```

这里不能用COUNT(*)，要用COUNT(e.subject_name)，因为e.subject_name中才有NULL





## **SQL15** **查看学校名称中含北京的用户**

**描述**

题目：现在运营想查看所有大学中带有北京的用户的信息，请你取出相应数据。

示例：用户信息表：user_profile

| id   | device_id | gender | age  | university   | gpa  |
| ---- | --------- | ------ | ---- | ------------ | ---- |
| 1    | 2138      | male   | 21   | 北京大学     | 3.4  |
| 2    | 3214      | male   |      | 复旦大学     | 4.0  |
| 3    | 6543      | female | 20   | 北京大学     | 3.2  |
| 4    | 2315      | female | 23   | 浙江大学     | 3.6  |
| 5    | 5432      | male   | 25   | 山东大学     | 3.8  |
| 6    | 2131      | male   | 28   | 北京师范大学 | 3.3  |

根据示例，你的查询应返回如下结果：

| device_id | age  | university   |
| --------- | ---- | ------------ |
| 2138      | 21   | 北京大学     |
| 6543      | 20   | 北京大学     |
| 2131      | 28   | 北京师范大学 |



**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8);
INSERT INTO user_profile VALUES(6,2131,'male',28,'北京师范大学',3.3);
```

复制

输出：

```
2138|21|北京大学
6543|20|北京大学
2131|28|北京师范大学
```

题解：

**字符匹配**

一般形式为：

列名 [NOT ] LIKE

匹配串中可包含如下四种通配符：
_：匹配任意一个字符；
%：匹配0个或多个字符；
[ ]：匹配[ ]中的任意一个字符(若要比较的字符是连续的，则可以用连字符“-”表 达 )；
[^ ]：不匹配[ ]中的任意一个字符。

例23．查询学生表中姓‘张’的学生的详细信息。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `‘张%’
```

例24．查询姓“张”且名字是3个字的学生姓名。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'张__’
```

如果把姓名列的类型改为nchar(20)，在SQL Server 2012中执行没有结果。原因是姓名列的类型是char(20)，当姓名少于20个汉字时，系统在存储这些数据时自动在后边补空格，空格作为一个字符，也参加LIKE的比较。可以用rtrim()去掉右空格。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `rtrim(姓名) ``LIKE` `'张__'
```

例25.查询学生表中姓‘张’、姓‘李’和姓‘刘’的学生的情况。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'[张李刘]%’
```

例26.查询学生表表中名字的第2个字为“小”或“大”的学生的姓名和学号。

```
SELECT` `姓名,学号 ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'_[小大]%'
```

例27.查询学生表中所有不姓“刘”的学生。

```
SELECT` `姓名 ``FROM` `学生 ``WHERE` `姓名 ``NOT` `LIKE` `'刘%’
```

例28.从学生表表中查询学号的最后一位不是2、3、5的学生信息。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `学号 ``LIKE` `'%[^235]'
```

题解：

```
select device_id,age,university from user_profile where university LIKE '%北京%';
```



## SQL16 查找GPA最高值

**描述**

题目：运营想要知道复旦大学学生gpa最高值是多少，请你取出相应数据

示例：某user_profile表如下:

| id   | device_id | gender | age  | university | gpa  |
| ---- | --------- | ------ | ---- | ---------- | ---- |
| 1    | 2234      | male   | 21   | 北京大学   | 3.2  |
| 2    | 2235      | male   | NULL | 复旦大学   | 3.8  |
| 3    | 2236      | female | 20   | 复旦大学   | 3.5  |
| 4    | 2237      | female | 23   | 浙江大学   | 3.3  |
| 5    | 2238      | male   | 25   | 复旦大学   | 3.1  |
| 6    | 2239      | male   | 25   | 北京大学   | 3.6  |
| 7    | 2240      | male   | NULL | 清华大学   | 3.3  |
| 8    | 2241      | female | NULL | 北京大学   | 3.7  |

根据输入，你的查询应返回以下结果，结果保留到小数点后面1位(1位之后的四舍五入):

| gpa  |
| ---- |
| 3.8  |



**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float);
INSERT INTO user_profile VALUES(1,2234,'male',21,'北京大学',3.2);
INSERT INTO user_profile VALUES(2,2235,'male',null,'复旦大学',3.8);
INSERT INTO user_profile VALUES(3,2236,'female',20,'复旦大学',3.5);
INSERT INTO user_profile VALUES(4,2237,'female',23,'浙江大学',3.3);
INSERT INTO user_profile VALUES(5,2238,'male',25,'复旦大学',3.1);
INSERT INTO user_profile VALUES(6,2239,'male',25,'北京大学',3.6);
INSERT INTO user_profile VALUES(7,2240,'male',null,'清华大学',3.3);
INSERT INTO user_profile VALUES(8,2241,'female',null,'北京大学',3.7);
```

复制

输出：

```
3.8
```

题解：

```
select max(gpa) from user_profile where university='复旦大学';
```



## SQL17 计算男生人数以及平均GPA

**描述**

题目：现在运营想要看一下男性用户有多少人以及他们的平均gpa是多少，用以辅助设计相关活动，请你取出相应数据。

示例：user_profile

| id   | device_id | gender | age  | university   | gpa  |
| ---- | --------- | ------ | ---- | ------------ | ---- |
| 1    | 2138      | male   | 21   | 北京大学     | 3.4  |
| 2    | 3214      | male   |      | 复旦大学     | 4.0  |
| 3    | 6543      | female | 20   | 北京大学     | 3.2  |
| 4    | 2315      | female | 23   | 浙江大学     | 3.6  |
| 5    | 5432      | male   | 25   | 山东大学     | 3.8  |
| 6    | 2131      | male   | 28   | 北京师范大学 | 3.3  |

根据输入，你的查询应返回以下结果，结果保留到小数点后面1位(1位之后的四舍五入)：

| male_num | avg_gpa |
| -------- | ------- |
| 4        | 3.6     |

**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8);
INSERT INTO user_profile VALUES(6,2131,'male',28,'北京师范大学',3.3);
```

复制

输出：

```
4|3.6
```

题解：

```
# 错误做法：
select count(gender='male') as male_num,avg(gpa) as avg_gpa from user_profile;
```

结果为 male_num:6，avg_gpa:3.5

```
# 正确做法
select count(gender) as male_num,ROUND(avg(gpa),1) as avg_gpa from user_profile where gender='male';
```



## SQL18 分组计算练习题

**描述**

题目：现在运营想要对每个学校不同性别的用户活跃情况和发帖数量进行分析，请分别计算出每个学校每种性别的用户数、30天内平均活跃天数和平均发帖数量。

用户信息表：user_profile

30天内活跃天数字段（active_days_within_30）

发帖数量字段（question_cnt）

回答数量字段（answer_cnt）

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4.0  | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4在过去的30天里面活跃了7天，发帖数量为2，回答数量为12

。。。

最后一行表示:id为7的用户的常用信息为使用的设备id为4321，性别为男，年龄26岁，复旦大学，gpa为3.6在过去的30天里面活跃了9天，发帖数量为6，回答数量为52

你的查询返回结果需要对**性别和学校分组**，示例如下，结果保留1位小数，1位小数之后的四舍五入：

| gender | university | user_num | avg_active_day | avg_question_cnt |
| ------ | ---------- | -------- | -------------- | ---------------- |
| male   | 北京大学   | 1        | 7.0            | 2.0              |
| male   | 复旦大学   | 2        | 12.0           | 5.5              |
| female | 北京大学   | 1        | 12.0           | 3.0              |
| female | 浙江大学   | 1        | 5.0            | 1.0              |
| male   | 山东大学   | 2        | 17.5           | 11.0             |



解释:

第一行表示：北京大学的男性用户个数为1，平均活跃天数为7天，平均发帖量为2

。。。

最后一行表示：山东大学的男性用户个数为2，平均活跃天数为17.5天，平均发帖量为11

**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` float,
`question_cnt` float,
`answer_cnt` float
);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
```

复制

输出：

```
male|北京大学|1|7.0|2.0
male|复旦大学|2|12.0|5.5
female|北京大学|1|12.0|3.0
female|浙江大学|1|5.0|1.0
male|山东大学|2|17.5|11.0
```

题解：这里关于user_num的计算，需要理解清楚。

```
select gender,university,
    count(id) as user_num, 
    ROUND(avg(active_days_within_30),1) as avg_active_day,
    ROUND(avg(question_cnt),1) as avg_question_cnt 
from user_profile GROUP BY gender,university;
```



## SQL19 分组过滤练习题

**描述**

题目：现在运营想查看每个学校用户的平均发贴和回帖情况，寻找低活跃度学校进行重点运营，请取出平均发贴数低于5的学校或平均回帖数小于20的学校。

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4.0  | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | female | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4在过去的30天里面活跃了7天，发帖数量为2，回答数量为12
。。。
最后一行表示:id为7的用户的常用信息为使用的设备id为4321，性别为男，年龄26岁，复旦大学，gpa为3.6在过去的30天里面活跃了9天，发帖数量为6，回答数量为52



根据示例，你的查询应返回以下结果，请你保留3位小数(系统后台也会自动校正)，3位之后四舍五入：

| university | avg_question_cnt | avg_answer_cnt |
| ---------- | ---------------- | -------------- |
| 北京大学   | 2.5000           | 21.000         |
| 浙江大学   | 1.000            | 2.000          |

解释: 平均发贴数低于5的学校或平均回帖数小于20的学校有2个

属于北京大学的用户的平均发帖量为2.500，平均回答数量为21.000

属于浙江大学的用户的平均发帖量为1.000，平均回答数量为2.000

**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` float,
`answer_cnt` float
);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
```

复制

输出：

```
university|avg_question_cnt|avg_answer_cnt
北京大学|2.500|21.000
浙江大学|1.000|2.000
```

题解：这里需要了解where和having的区别，having是聚合函数作为筛选结果。

第一步：

```
select university,
    avg(question_cnt) as avg_question_cnt,
    avg(answer_cnt) as avg_answer_cnt
from user_profile
GROUP BY university
```

结果为：

| university | avg_question_cnt | avg_answer_cnt |
| ---------- | ---------------- | -------------- |
| 北京大学   | 2.500            | 21.000         |
| 复旦大学   | 5.500            | 38.500         |
| 浙江大学   | 1.000            | 2.000          |
| 山东大学   | 11.000           | 41.500         |

```
select university,
    avg(question_cnt) as avg_question_cnt,
    avg(answer_cnt) as avg_answer_cnt
from user_profile
GROUP BY university
having avg_question_cnt<5 OR avg_answer_cnt<20
```



## SQL20 分组排序练习题

**描述**

题目：现在运营想要查看不同大学的用户平均发帖情况，并期望结果按照平均发帖情况进行升序排列，请你取出相应数据。

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4.0  | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | female | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4在过去的30天里面活跃了7天，发帖数量为2，回答数量为12
。。。
最后一行表示:id为7的用户的常用信息为使用的设备id为4321，性别为男，年龄26岁，复旦大学，gpa为3.6在过去的30天里面活跃了9天，发帖数量为6，回答数量为52

根据示例，你的查询应返回以下结果：

| university | avg_question_cnt |
| ---------- | ---------------- |
| 浙江大学   | 1.0000           |
| 北京大学   | 2.5000           |
| 复旦大学   | 5.5000           |
| 山东大学   | 11.0000          |

**示例1**

输入：

```
drop table if exists user_profile;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
```

复制

输出：

```
浙江大学|1.0000
北京大学|2.5000
复旦大学|5.5000
山东大学|11.0000
```

题解：

```
select university,
    avg(question_cnt) as avg_question_cnt
from user_profile 
GROUP BY university
ORDER BY avg_question_cnt;
```



## SQL21 浙江大学用户题目回答情况

**描述**

题目：现在运营想要查看所有来自浙江大学的用户题目回答明细情况，请你取出相应数据

示例 ：question_practice_detail

| id   | device_id | question_id | result |
| ---- | --------- | ----------- | ------ |
| 1    | 2138      | 111         | wrong  |
| 2    | 3214      | 112         | wrong  |
| 3    | 3214      | 113         | wrong  |
| 4    | 6543      | 114         | right  |
| 5    | 2315      | 115         | right  |
| 6    | 2315      | 116         | right  |
| 7    | 2315      | 117         | wrong  |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，在question_id为111的题目上，回答错误

....

最后一行表示:id为7的用户的常用信息为使用的设备id为2135，在question_id为117的题目上，回答错误

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4.0  | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | female | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4在过去的30天里面活跃了7天，发帖数量为2，回答数量为12
。。。
最后一行表示:id为7的用户的常用信息为使用的设备id为4321，性别为男，年龄26岁，复旦大学，gpa为3.6在过去的30天里面活跃了9天，发帖数量为6，回答数量为52

根据示例，你的查询应返回以下结果，查询结果根据question_id升序排序：

![img](https://uploadfiles.nowcoder.com/images/20210825/999991344_1629872498861/D9E601E7A15464E123E07993B5B38512)

解释:

根据题目的数据只有1个浙江大学的用户，那么把浙江大学这个用户所有答题数据查询出来就行

**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);
INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(8,5432,118,'wrong');
INSERT INTO question_practice_detail VALUES(9,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(10,2131,114,'right');
INSERT INTO question_practice_detail VALUES(11,5432,113,'wrong');
```

复制

输出：

```
2315|115|right
2315|116|right
2315|117|wrong
```

题解：

错误做法

```
正确做法：
select device_id,question_id,result 
from  user_profile 
JOIN question_practice_detail 
ON question_practice_detail.device_id=user_profile.device_id 
AND user_profile.university='浙江大学';
```



```
# 正确做法：
select us.device_id,qe.question_id,qe.result 
from  user_profile us
JOIN question_practice_detail qe
ON qe.device_id=us.device_id 
AND us.university='浙江大学';
```



## SQL22 统计每个学校的答过题的用户的平均答题数

**描述**

运营想要了解每个学校答过题的用户平均答题数量情况，请你取出数据。

**用户信息表 user_profile，其中device_id指终端编号（认为每个用户有唯一的一个终端），gender指性别，age指年龄，university指用户所在的学校，gpa是该用户平均学分绩点，active_days_within_30是30天内的活跃天数。**

| device_id | gender | age  | university | gpa  | active_days_within_30 |
| --------- | ------ | ---- | ---------- | ---- | --------------------- |
| 2138      | male   | 21   | 北京大学   | 3.4  | 7                     |
| 3214      | male   | NULL | 复旦大学   | 4    | 15                    |
| 6543      | female | 20   | 北京大学   | 3.2  | 12                    |
| 2315      | female | 23   | 浙江大学   | 3.6  | 5                     |
| 5432      | male   | 25   | 山东大学   | 3.8  | 20                    |
| 2131      | male   | 28   | 山东大学   | 3.3  | 15                    |
| 4321      | male   | 28   | 复旦大学   | 3.6  | 9                     |



第一行表示:用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4，在过去的30天里面活跃了7天

最后一行表示:用户的常用信息为使用的设备id为4321，性别为男，年龄28岁，复旦大学，gpa为3.6，在过去的30天里面活跃了9天

**答题情况明细表** **question_practice_detail，其中question_id是题目编号，result是答题结果。**

| device_id | question_id | result |
| --------- | ----------- | ------ |
| 2138      | 111         | wrong  |
| 3214      | 112         | wrong  |
| 3214      | 113         | wrong  |
| 6543      | 111         | right  |
| 2315      | 115         | right  |
| 2315      | 116         | right  |
| 2315      | 117         | wrong  |
| 5432      | 118         | wrong  |
| 5432      | 112         | wrong  |
| 2131      | 114         | right  |
| 5432      | 113         | wrong  |



第一行表示用户的常用信息为使用的设备id为2138，在question_id为111的题目上，回答错误

....

最后一行表示用户的常用信息为使用的设备id为5432，在question_id为113的题目上，回答错误

请你写SQL查找每个学校用户的平均答题数目(说明：某学校用户平均答题数量计算方式为该学校用户答题总次数除以答过题的不同用户个数)根据示例，你的查询应返回以下结果（结果保留4位小数），注意：**结果按照university升序排序！！！**

| university | avg_answer_cnt |
| ---------- | -------------- |
| 北京大学   | 1.0000         |
| 复旦大学   | 2.0000         |
| 山东大学   | 2.0000         |
| 浙江大学   | 3.0000         |

解释:

第一行：北京大学总共有2个用户，2138和6543，2个用户在**question_practice_detail里面答了2题，平均答题数目为2/2=1.0000**

**....**

**最后一行:浙江大学总共有1个用户，2315，这个用户在\**question_practice_detail里面答了3题，平均答题数目为3/1=3.0000\****

**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
CREATE TABLE `user_profile` (
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int
);
CREATE TABLE `question_practice_detail` (
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(2138,'male',21,'北京大学',3.4,7);
INSERT INTO user_profile VALUES(3214,'male',null,'复旦大学',4.0,15);
INSERT INTO user_profile VALUES(6543,'female',20,'北京大学',3.2,12);
INSERT INTO user_profile VALUES(2315,'female',23,'浙江大学',3.6,5);
INSERT INTO user_profile VALUES(5432,'male',25,'山东大学',3.8,20);
INSERT INTO user_profile VALUES(2131,'male',28,'山东大学',3.3,15);
INSERT INTO user_profile VALUES(4321,'male',28,'复旦大学',3.6,9);
INSERT INTO question_practice_detail VALUES(2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(6543,111,'right');
INSERT INTO question_practice_detail VALUES(2315,115,'right');
INSERT INTO question_practice_detail VALUES(2315,116,'right');
INSERT INTO question_practice_detail VALUES(2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(5432,118,'wrong');
INSERT INTO question_practice_detail VALUES(5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(2131,114,'right');
INSERT INTO question_practice_detail VALUES(5432,113,'wrong');
```

复制

输出：

```
北京大学|1.0000
复旦大学|2.0000
山东大学|2.0000
浙江大学|3.0000
```

题解：

```
select a.university,ROUND(count(b.question_id)/count(distinct b.device_id),4) AS avg_answer_cnt
from user_profile a
JOIN question_practice_detail b
ON a.device_id=b.device_id
GROUP BY a.university;
# ORDER BY avg_answer_cnt;不需要最后这句
```



## SQL23 统计每个学校各难度的用户平均刷题数

**描述**

题目：运营想要计算一些**参加了答题**的不同学校、不同难度的用户平均答题量，请你写SQL取出相应数据

用户信息表：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   | NULL | 复旦大学   | 4    | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 28   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4，在过去的30天里面活跃了7天，发帖数量为2，回答数量为12

最后一行表示:id为7的用户的常用信息为使用的设备id为4321，性别为男，年龄28岁，复旦大学，gpa为3.6，在过去的30天里面活跃了9天，发帖数量为6，回答数量为52

题库练习明细表：question_practice_detail

| id   | device_id | question_id | result |
| ---- | --------- | ----------- | ------ |
| 1    | 2138      | 111         | wrong  |
| 2    | 3214      | 112         | wrong  |
| 3    | 3214      | 113         | wrong  |
| 4    | 6534      | 111         | right  |
| 5    | 2315      | 115         | right  |
| 6    | 2315      | 116         | right  |
| 7    | 2315      | 117         | wrong  |
| 8    | 5432      | 117         | wrong  |
| 9    | 5432      | 112         | wrong  |
| 10   | 2131      | 113         | right  |
| 11   | 5432      | 113         | wrong  |
| 12   | 2315      | 115         | right  |
| 13   | 2315      | 116         | right  |
| 14   | 2315      | 117         | wrong  |
| 15   | 5432      | 117         | wrong  |
| 16   | 5432      | 112         | wrong  |
| 17   | 2131      | 113         | right  |
| 18   | 5432      | 113         | wrong  |
| 19   | 2315      | 117         | wrong  |
| 20   | 5432      | 117         | wrong  |
| 21   | 5432      | 112         | wrong  |
| 22   | 2131      | 113         | right  |
| 23   | 5432      | 113         | wrong  |



第一行表示:id为1的用户的常用信息为使用的设备id为2138，在question_id为111的题目上，回答错误

......

最后一行表示:id为23的用户的常用信息为使用的设备id为5432，在question_id为113的题目上，回答错误

表：question_detail

| id   | question_id | difficult_level |
| ---- | ----------- | --------------- |
| 1    | 111         | hard            |
| 2    | 112         | medium          |
| 3    | 113         | easy            |
| 4    | 115         | easy            |
| 5    | 116         | medium          |
| 6    | 117         | easy            |

第一行表示: 题目id为111的难度为hard

....

第一行表示: 题目id为117的难度为easy

请你写一个SQL查询，计算不同学校、不同难度的用户平均答题量，根据示例，你的查询应返回以下结果(结果在小数点位数保留4位，4位之后四舍五入)：

| university | difficult_level | avg_answer_cnt |
| ---------- | --------------- | -------------- |
| 北京大学   | hard            | 1.0000         |
| 复旦大学   | easy            | 1.0000         |
| 复旦大学   | medium          | 1.0000         |
| 山东大学   | easy            | 4.5000         |
| 山东大学   | medium          | 3.0000         |
| 浙江大学   | easy            | 5.0000         |
| 浙江大学   | medium          | 2.0000         |

解释：

第一行：北京大学有设备id为2138，6543这2个用户，这2个用户在question_practice_detail表下都只有一条答题记录，且答题题目是111，从question_detail可以知道这个题目是hard，故 北京大学的用户答题为hard的题目平均答题为2/2=1.0000

第二行，第三行：复旦大学有设备id为3214，4321这2个用户，但是在question_practice_detail表只有1个用户(device_id=3214有答题，device_id=4321没有答题，不计入后续计算)有2条答题记录，且答题题目是112，113各1个，从question_detail可以知道题目难度分别是medium和easy，故 复旦大学的用户答题为easy, medium的题目平均答题量都为1(easy=1或medium=1) /1 (device_id=3214)=1.0000

第四行，第五行：山东大学有设备id为5432和2131这2个用户，这2个用户总共在question_practice_detail表下有12条答题记录，且答题题目是112，113，117，且数目分别为3，6，3，从question_detail可以知道题目难度分别为medium,easy,easy，所以，easy共有9个，故easy的题目平均答题量= 9(easy=9)/2 (device_id=3214 or device_id=5432) =4.5000，medium共有3个，medium的答题只有device_id=5432的用户，故medium的题目平均答题量= 3(medium=9)/1 ( device_id=5432) =3.0000

.....

**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
drop table if  exists `question_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);
CREATE TABLE `question_detail` (
`id` int NOT NULL,
`question_id`int NOT NULL,
`difficult_level` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(8,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(9,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(10,2131,113,'right');
INSERT INTO question_practice_detail VALUES(11,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(12,2315,115,'right');
INSERT INTO question_practice_detail VALUES(13,2315,116,'right');
INSERT INTO question_practice_detail VALUES(14,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(15,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(16,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(17,2131,113,'right');
INSERT INTO question_practice_detail VALUES(18,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(19,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(20,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(21,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(22,2131,113,'right');
INSERT INTO question_practice_detail VALUES(23,5432,113,'wrong');
INSERT INTO question_detail VALUES(1,111,'hard');
INSERT INTO question_detail VALUES(2,112,'medium');
INSERT INTO question_detail VALUES(3,113,'easy');
INSERT INTO question_detail VALUES(4,115,'easy');
INSERT INTO question_detail VALUES(5,116,'medium');
INSERT INTO question_detail VALUES(6,117,'easy');
```

复制

输出：

```
北京大学|hard|1.0000
复旦大学|easy|1.0000
复旦大学|medium|1.0000
山东大学|easy|4.5000
山东大学|medium|3.0000
浙江大学|easy|5.0000
浙江大学|medium|2.0000
```

**题解**：这里有三个表需要查找，所以需要一次建立连接。

- 限定条件：无；
- 每个学校：按学校分组`group by university`
- 不同难度：按难度分组`group by difficult_level`
- 平均答题数：总答题数除以总人数`count(b.question_id) / count(distinct b.device_id)`
- 来自上面信息三个表，需要联表，a与b用device_id连接，b与c用question_id连接。

```
select a.university,c.difficult_level,
    ROUND(count(b.question_id)/count(distinct b.device_id),4) AS avg_answer_cnt
from user_profile a
JOIN question_practice_detail b ON a.device_id=b.device_id
JOIN question_detail c ON b.question_id=c.question_id
GROUP BY a.university,c.difficult_level;

```

## SQL24 统计每个用户的平均刷题数

**描述**

题目：运营想要查看**参加了答题**的山东大学的用户在不同难度下的平均答题题目数，请取出相应数据

用户信息表：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   | NULL | 复旦大学   | 4    | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 28   | 复旦大学   | 3.6  | 9                     | 6            | 52         |



第一行表示:id为1的用户的常用信息为使用的设备id为2138，性别为男，年龄21岁，北京大学，gpa为3.4，在过去的30天里面活跃了7天，发帖数量为2，回答数量为12

最后一行表示:id为7的用户的常用信息为使用的设备id为432，性别为男，年龄28岁，复旦大学，gpa为3.6，在过去的30天里面活跃了9天，发帖数量为6，回答数量为52

题库练习明细表：question_practice_detail

| id   | device_id | question_id | result |
| ---- | --------- | ----------- | ------ |
| 1    | 2138      | 111         | wrong  |
| 2    | 3214      | 112         | wrong  |
| 3    | 3214      | 113         | wrong  |
| 4    | 6534      | 111         | right  |
| 5    | 2315      | 115         | right  |
| 6    | 2315      | 116         | right  |
| 7    | 2315      | 117         | wrong  |
| 8    | 5432      | 117         | wrong  |
| 9    | 5432      | 112         | wrong  |
| 10   | 2131      | 113         | right  |
| 11   | 5432      | 113         | wrong  |
| 12   | 2315      | 115         | right  |
| 13   | 2315      | 116         | right  |
| 14   | 2315      | 117         | wrong  |
| 15   | 5432      | 117         | wrong  |
| 16   | 5432      | 112         | wrong  |
| 17   | 2131      | 113         | right  |
| 18   | 5432      | 113         | wrong  |
| 19   | 2315      | 117         | wrong  |
| 20   | 5432      | 117         | wrong  |
| 21   | 5432      | 112         | wrong  |
| 22   | 2131      | 113         | right  |
| 23   | 5432      | 113         | wrong  |



第一行表示:id为1的用户的常用信息为使用的设备id为2138，在question_id为111的题目上，回答错误

......

最后一行表示:id为23的用户的常用信息为使用的设备id为5432，在question_id为113的题目上，回答错误

表：question_detail

| id   | question_id | difficult_level |
| ---- | ----------- | --------------- |
| 1    | 111         | hard            |
| 2    | 112         | medium          |
| 3    | 113         | easy            |
| 4    | 115         | easy            |
| 5    | 116         | medium          |
| 6    | 117         | easy            |



第一行表示: 题目id为111的难度为hard

....

第一行表示: 题目id为117的难度为easy

请你写一个SQL查询，计算山东、不同难度的用户平均答题量，根据示例，你的查询应返回以下结果(结果在小数点位数保留4位，4位之后四舍五入)：



| university | difficult_level | avg_answer_cnt |
| ---------- | --------------- | -------------- |
| 山东大学   | easy            | 4.5000         |
| 山东大学   | medium          | 3.0000         |



山东大学有设备id为5432和2131这2个用户，这2个用户总共在question_practice_detail表下有12条答题记录，且答题题目是112，113，117，且数目分别为3，6，3，从question_detail可以知道题目难度分别为medium,easy,easy，所以，easy共有9个，故easy的题目平均答题量= 9(easy=9)/2 (device_id=3214 or device_id=5432) =4.5000，medium共有3个，medium的答题只有device_id=5432的用户，故medium的题目平均答题量= 3(medium=9)/1 ( device_id=5432) =3.0000

**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);
CREATE TABLE `question_detail` (
`id` int NOT NULL,
`question_id`int NOT NULL,
`difficult_level` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(8,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(9,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(10,2131,113,'right');
INSERT INTO question_practice_detail VALUES(11,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(12,2315,115,'right');
INSERT INTO question_practice_detail VALUES(13,2315,116,'right');
INSERT INTO question_practice_detail VALUES(14,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(15,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(16,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(17,2131,113,'right');
INSERT INTO question_practice_detail VALUES(18,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(19,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(20,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(21,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(22,2131,113,'right');
INSERT INTO question_practice_detail VALUES(23,5432,113,'wrong');
INSERT INTO question_detail VALUES(1,111,'hard');
INSERT INTO question_detail VALUES(2,112,'medium');
INSERT INTO question_detail VALUES(3,113,'easy');
INSERT INTO question_detail VALUES(4,115,'easy');
INSERT INTO question_detail VALUES(5,116,'medium');
INSERT INTO question_detail VALUES(6,117,'easy');
```

复制

输出：

```
山东大学|easy|4.5000
山东大学|medium|3.0000
```

题解：

注意：这里where需要在group by之前

方法一：

```
select a.university,c.difficult_level,
    ROUND(count(b.question_id)/count(distinct b.device_id),4) AS avg_answer_cnt
from user_profile a
JOIN question_practice_detail b ON a.device_id=b.device_id
JOIN question_detail c ON b.question_id=c.question_id
WHERE a.university='山东大学'
GROUP BY a.university,c.difficult_level
```

方法二：

```
SELECT 
    t1.university,
    t3.difficult_level,
    COUNT(t2.question_id) / COUNT(DISTINCT(t2.device_id)) as avg_answer_cnt
from 
    user_profile as t1,
    question_practice_detail as t2,
    question_detail as t3
WHERE 
    t1.university = '山东大学'
    and t1.device_id = t2.device_id
    and t2.question_id = t3.question_id
GROUP BY
    t3.difficult_level;
```



## SQL25 查找山东大学或者性别为男生的信息

**描述**

题目：现在运营想要分别查看学校为山东大学或者性别为男性的用户的device_id、gender、age和gpa数据，请取出相应结果，结果不去重。

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4    | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

根据示例，你的查询应返回以下结果（注意输出的顺序，先输出学校为山东大学再输出性别为男生的信息）：

| device_id | gender | age  | gpa  |
| --------- | ------ | ---- | ---- |
| 5432      | male   | 25   | 3.8  |
| 2131      | male   | 28   | 3.3  |
| 2138      | male   | 21   | 3.4  |
| 3214      | male   | None | 4    |
| 5432      | male   | 25   | 3.8  |
| 2131      | male   | 28   | 3.3  |
| 4321      | male   | 28   | 3.6  |



**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);
CREATE TABLE `question_detail` (
`id` int NOT NULL,
`question_id`int NOT NULL,
`difficult_level` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(8,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(9,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(10,2131,113,'right');
INSERT INTO question_practice_detail VALUES(11,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(12,2315,115,'right');
INSERT INTO question_practice_detail VALUES(13,2315,116,'right');
INSERT INTO question_practice_detail VALUES(14,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(15,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(16,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(17,2131,113,'right');
INSERT INTO question_practice_detail VALUES(18,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(19,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(20,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(21,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(22,2131,113,'right');
INSERT INTO question_practice_detail VALUES(23,5432,113,'wrong');
INSERT INTO question_detail VALUES(1,111,'hard');
INSERT INTO question_detail VALUES(2,112,'medium');
INSERT INTO question_detail VALUES(3,113,'easy');
INSERT INTO question_detail VALUES(4,115,'easy');
INSERT INTO question_detail VALUES(5,116,'medium');
INSERT INTO question_detail VALUES(6,117,'easy');
```

复制

输出：

```
5432|male|25|3.8
2131|male|28|3.3
2138|male|21|3.4
3214|male|None|4.0
5432|male|25|3.8
2131|male|28|3.3
4321|male|28|3.6
```

题解：这里是需要将两次查询结果都显示出来，这里就需要使用union关键字。

```
select device_id,gender,age,gpa from user_profile
where university='山东大学' 
union all
select device_id,gender,age,gpa from user_profile
where  gender='male';
```



## SQL26 计算25岁以上和以下的用户数量

**描述**

题目：现在运营想要将用户划分为25岁以下和25岁及以上两个年龄段，分别查看这两个年龄段用户数量

**本题注意：age为null 也记为 25岁以下**

示例：user_profile

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4    | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

根据示例，你的查询应返回以下结果：

| age_cut    | number |
| ---------- | ------ |
| 25岁以下   | 4      |
| 25岁及以上 | 3      |



**示例1**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL
);
CREATE TABLE `question_detail` (
`id` int NOT NULL,
`question_id`int NOT NULL,
`difficult_level` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(8,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(9,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(10,2131,113,'right');
INSERT INTO question_practice_detail VALUES(11,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(12,2315,115,'right');
INSERT INTO question_practice_detail VALUES(13,2315,116,'right');
INSERT INTO question_practice_detail VALUES(14,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(15,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(16,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(17,2131,113,'right');
INSERT INTO question_practice_detail VALUES(18,5432,113,'wrong');
INSERT INTO question_practice_detail VALUES(19,2315,117,'wrong');
INSERT INTO question_practice_detail VALUES(20,5432,117,'wrong');
INSERT INTO question_practice_detail VALUES(21,5432,112,'wrong');
INSERT INTO question_practice_detail VALUES(22,2131,113,'right');
INSERT INTO question_practice_detail VALUES(23,5432,113,'wrong');
INSERT INTO question_detail VALUES(1,111,'hard');
INSERT INTO question_detail VALUES(2,112,'medium');
INSERT INTO question_detail VALUES(3,113,'easy');
INSERT INTO question_detail VALUES(4,115,'easy');
INSERT INTO question_detail VALUES(5,116,'medium');
INSERT INTO question_detail VALUES(6,117,'easy');
```

复制

输出：

```
25岁以下|4
25岁及以上|3
```



题解：首先注意case函数的使用方法

```
select  case when age<25 OR age IS NULL then '25岁以下'
             when age>=25 then '25岁及以上'
        END age_cut,count(*)number   #这里的number可有可无
from user_profile
GROUP BY age_cut
```

方法二：使用if判断语句

```
# 方法二：使用if语句
select if(age<25 OR age IS NULL,'25岁以下','25岁及以上') AS age_cut,
        count(*) as number
from user_profile
GROUP BY age_cut;
```

方法三：使用联合语句查询

```
# 方法三： 使用联合语句
select '25岁以下' age_cut,count(*) AS number
from user_profile
where age<25 OR age IS NULL
union
select '25岁及以上' age_cut,count(*) AS number
from user_profile
where age>=25
```



## SQL28 计算用户8月每天的练题数量

**描述**

题目：现在运营想要计算出**2021年8月每天用户练习题目的数量**，请取出相应数据。

**示例：question_practice_detail**

| id   | device_id | question_id | result | date       |
| ---- | --------- | ----------- | ------ | ---------- |
| 1    | 2138      | 111         | wrong  | 2021-05-03 |
| 2    | 3214      | 112         | wrong  | 2021-05-09 |
| 3    | 3214      | 113         | wrong  | 2021-06-15 |
| 4    | 6543      | 111         | right  | 2021-08-13 |
| 5    | 2315      | 115         | right  | 2021-08-13 |
| 6    | 2315      | 116         | right  | 2021-08-14 |
| 7    | 2315      | 117         | wrong  | 2021-08-15 |
| ……   |           |             |        |            |



根据示例，你的查询应返回以下结果：

| day  | question_cnt |
| ---- | ------------ |
| 13   | 5            |
| 14   | 2            |
| 15   | 3            |
| 16   | 1            |
| 18   | 1            |



**示例**

输入：

```
drop table if exists `user_profile`;
drop table if  exists `question_practice_detail`;
drop table if  exists `question_detail`;
CREATE TABLE `user_profile` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`gender` varchar(14) NOT NULL,
`age` int ,
`university` varchar(32) NOT NULL,
`gpa` float,
`active_days_within_30` int ,
`question_cnt` int ,
`answer_cnt` int 
);
CREATE TABLE `question_practice_detail` (
`id` int NOT NULL,
`device_id` int NOT NULL,
`question_id`int NOT NULL,
`result` varchar(32) NOT NULL,
`date` date NOT NULL
);
CREATE TABLE `question_detail` (
`id` int NOT NULL,
`question_id`int NOT NULL,
`difficult_level` varchar(32) NOT NULL
);

INSERT INTO user_profile VALUES(1,2138,'male',21,'北京大学',3.4,7,2,12);
INSERT INTO user_profile VALUES(2,3214,'male',null,'复旦大学',4.0,15,5,25);
INSERT INTO user_profile VALUES(3,6543,'female',20,'北京大学',3.2,12,3,30);
INSERT INTO user_profile VALUES(4,2315,'female',23,'浙江大学',3.6,5,1,2);
INSERT INTO user_profile VALUES(5,5432,'male',25,'山东大学',3.8,20,15,70);
INSERT INTO user_profile VALUES(6,2131,'male',28,'山东大学',3.3,15,7,13);
INSERT INTO user_profile VALUES(7,4321,'male',28,'复旦大学',3.6,9,6,52);
INSERT INTO question_practice_detail VALUES(1,2138,111,'wrong','2021-05-03');
INSERT INTO question_practice_detail VALUES(2,3214,112,'wrong','2021-05-09');
INSERT INTO question_practice_detail VALUES(3,3214,113,'wrong','2021-06-15');
INSERT INTO question_practice_detail VALUES(4,6543,111,'right','2021-08-13');
INSERT INTO question_practice_detail VALUES(5,2315,115,'right','2021-08-13');
INSERT INTO question_practice_detail VALUES(6,2315,116,'right','2021-08-14');
INSERT INTO question_practice_detail VALUES(7,2315,117,'wrong','2021-08-15');
INSERT INTO question_practice_detail VALUES(8,3214,112,'wrong','2021-05-09');
INSERT INTO question_practice_detail VALUES(9,3214,113,'wrong','2021-08-15');
INSERT INTO question_practice_detail VALUES(10,6543,111,'right','2021-08-13');
INSERT INTO question_practice_detail VALUES(11,2315,115,'right','2021-08-13');
INSERT INTO question_practice_detail VALUES(12,2315,116,'right','2021-08-14');
INSERT INTO question_practice_detail VALUES(13,2315,117,'wrong','2021-08-15');
INSERT INTO question_practice_detail VALUES(14,3214,112,'wrong','2021-08-16');
INSERT INTO question_practice_detail VALUES(15,3214,113,'wrong','2021-08-18');
INSERT INTO question_practice_detail VALUES(16,6543,111,'right','2021-08-13');
INSERT INTO question_detail VALUES(1,111,'hard');
INSERT INTO question_detail VALUES(2,112,'medium');
INSERT INTO question_detail VALUES(3,113,'easy');
INSERT INTO question_detail VALUES(4,115,'easy');
INSERT INTO question_detail VALUES(5,116,'medium');
INSERT INTO question_detail VALUES(6,117,'easy');
```

复制

输出：

```
13|5
14|2
15|3
16|1
18|1
```

题解：





## SQL30 统计每种性别的人数







- 题目

- 题解(92)

- 讨论(491)

- 排行

- 面经

  new

中等 通过率：43.68% 时间限制：1秒 空间限制：256M

## 描述

题目：现在运营举办了一场比赛，收到了一些参赛申请，表数据记录形式如下所示，现在运营想要统计每个性别的用户分别有多少参赛者，请取出相应结果

示例：user_submit

| device_id | profile              | blog_url            |
| --------- | -------------------- | ------------------- |
| 2138      | 180cm,75kg,27,male   | http:/url/bigboy777 |
| 3214      | 165cm,45kg,26,female | http:/url/kittycc   |
| 6543      | 178cm,65kg,25,male   | http:/url/tiger     |
| 4321      | 171cm,55kg,23,female | http:/url/uhksd     |
| 2131      | 168cm,45kg,22,female | http:/urlsydney     |

根据示例，你的查询应返回以下结果：

| gender | number |
| ------ | ------ |
| male   | 2      |
| female | 3      |

题解：

```
# 方法一：使用if语句进行判断
select if(profile like '%female%','female','male')as gender,count(*) as number
from user_submit
group by gender;
```

```
# 方法二，使用字符匹配

```













## SQL195 查找最晚入职员工的所有信息

**描述**

有一个员工employees表简况如下:

| emp_no | birth_date | first_name | last_name | gender | hire_date  |
| ------ | ---------- | ---------- | --------- | ------ | ---------- |
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
| 10002  | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
| 10003  | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
| 10004  | 1954-05-01 | Christian  | Koblick   | M      | 1986-12-01 |

请你查找employees里最晚入职员工的所有信息，以上例子输出如下:emp_nobirth_datefirst_namelast_namegenderhire_date10004
1954-05-01Christian 
Koblick  
M  
1986-12-01 

**示例1**

输入：

```
drop table if exists  `employees` ; 
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
INSERT INTO employees VALUES(10001,'1953-09-02','Georgi','Facello','M','1986-06-26');
INSERT INTO employees VALUES(10002,'1964-06-02','Bezalel','Simmel','F','1985-11-21');
INSERT INTO employees VALUES(10003,'1959-12-03','Parto','Bamford','M','1986-08-28');
INSERT INTO employees VALUES(10004,'1954-05-01','Chirstian','Koblick','M','1986-12-01');
INSERT INTO employees VALUES(10005,'1955-01-21','Kyoichi','Maliniak','M','1989-09-12');
INSERT INTO employees VALUES(10006,'1953-04-20','Anneke','Preusig','F','1989-06-02');
INSERT INTO employees VALUES(10007,'1957-05-23','Tzvetan','Zielinski','F','1989-02-10');
INSERT INTO employees VALUES(10008,'1958-02-19','Saniya','Kalloufi','M','1994-09-15');
INSERT INTO employees VALUES(10009,'1952-04-19','Sumant','Peac','F','1985-02-18');
INSERT INTO employees VALUES(10010,'1963-06-01','Duangkaew','Piveteau','F','1989-08-24');
INSERT INTO employees VALUES(10011,'1953-11-07','Mary','Sluis','F','1990-01-22');
```

复制

输出：

```
10008|1958-02-19|Saniya|Kalloufi|M|1994-09-15
```

题解：

```
select * from employees
where hire_date=(select max(hire_date) from employees)
```





