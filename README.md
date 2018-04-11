# lintcode
题目：
Given an input string, reverse the string word by word.

For example,
Given s = "the sky is blue",
return "blue is sky the".


思路：
方法1：首先把句子看做由词组成的，例如“A B C”，因此可以将句子的所有字符前后交换，得到“C' B' A'"。显然X‘表示逆序的词X，所以第二步是将每个词中的字符串前后交换。整个过程的时间复杂度为O(n)，空间复杂度为O(1)。这种方法的缺点是没有考虑许多特殊情况，例如字符串中有连续的空格，字符串开始结尾处有空格等。

方法2：利用两个stack，一个表示单词，一个表示句子。当遇到非空格字符时放入单词stack；当遇到空格时将单词stack中的字符压入句子stack中（注意：单词此时已经逆序一次），然后仅添加一个空格。最后将句子stack依次输出，此时句子逆序。两次逆序的道理同方法1.

代码：
方法1：
[cpp] view plain copy
class Solution {    
public:    
    void reverseWords(string &s) {    
        s = removeDuplicateSpace(s);  
          
        int begin = 0;    
        int end = 0;  
            
        while(end < s.size()){    
            if(s[end] == ' '){    
                swapString(s, begin, end - 1);    
                begin = end+1;    
                end = begin;    
            }    
            else{    
                end++;    
            }    
        }    
        swapString(s, begin, end - 1);    
            
        swapString(s, 0, s.size()-1);    
    }    
        
    void swapString(string &s, int begin, int end) {    
        while(end > begin){    
            char c = s[begin];    
            s[begin] = s[end];    
            s[end] = c;    
            begin++;    
            end--;    
        }    
    }  
      
    string removeDuplicateSpace(string s) {  
        string res;  
        int begin = 0;  
        for(; begin < s.size(); begin++) {  
            if(s[begin] != ' '){  
                break;  
            }  
        }  
        int end = s.size() - 1;  
        for(; end >= 0; end--) {  
            if(s[end] != ' '){  
                break;  
            }  
        }  
        bool is_space = false;  
        for(int i = begin; i <= end; i++) {  
            if(s[i] == ' '){  
                if(!is_space){  
                    is_space = true;  
                }  
                else{  
                    continue;  
                }  
            }  
            else{  
                is_space = false;  
            }  
            res.push_back(s[i]);  
        }  
        return res;  
    }  
};   

方法2：
[cpp] view plain copy
class Solution {  
public:  
    void reverseWords(string &s) {  
        stack<int> word;  
        stack<int> sentence;  
        int i = 0;  
          
        while(i <= s.size()){  
            if(i == s.size() || s[i] == ' '){  
                if(!word.empty()){  
                    if(!sentence.empty()){  
                        sentence.push(' ');  
                    }  
                    while(!word.empty()){  
                        sentence.push(word.top());  
                        word.pop();  
                    }  
                }  
            } else{  
                word.push(s[i]);  
            }  
            i++;  
        };  
          
        s.clear();  
        while(!sentence.empty()){  
            s.push_back(sentence.top());  
            sentence.pop();  
        };  
    }  
}; 
