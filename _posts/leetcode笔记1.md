---
layout:     post
title:      lc
subtitle:   
date:       2020-02-02
author:     gy
# header-img: img/post-bg-cook.jpg
catalog: true
tags:
    # - leetcode
---
# 前言
这个博客有点操蛋啊。
## methods in java
```
Stack<Integer> stack = new Stack<>();
```
## 1021. Remove Outermost Parentheses
1. 不要考虑括号是否合法，实际上来讲判断合法和去除外部括号是两个并列的行为，分开来写更清晰简单，也更合乎逻辑。
2. 然后解决问题时候只需分类，然后用count来判断是第几个括号就可以了，感觉count可以很大程度上代替stack解决这类问题。
```
class Solution {
    public String removeOuterParentheses(String S) {
        StringBuilder sb = new StringBuilder();
        int count =0;
        for(int index=0;index<S.length();index++)
        {
            if(S.charAt(index)=='(')
            {
                if(count>0)
                {
                    sb.append('(');
                }
                count++;
            }
            else
            {
                if(count>1)
                {
                    sb.append(")");
                }
                count--;
            }
        }
        return sb.toString();
    }
}
```

char cannot be dereferenced! So use == rather than equals

## 1047. Remove All Adjacent Duplicates In String
1. 非常精妙。可以理解为在build stirng的时候每当遇到重复的就删除，很快就变成了一个循环的问题。

```
class Solution {
    public String removeDuplicates(String S) {
        char[] newc = S.toCharArray();
        int i=0;
        for(int index=0;index<newc.length;index++,i++)
        {
            newc[i] = newc[index];
            if(i>0&&newc[i]==newc[i-1])
            {
                i-=2;
            }
        }
        return new String(newc,0,i);
    }
}
```

## 496. Next Greater Element I
1. 非常好的一道题目, 需要的是从左往右数的第一个较大数，所以一般会有n^2种解法，但是如果是连续递减的数列，他们有共同的更大数值，所以可以想象使用一个抽象的结构代表该递减数列，一旦遇到较大数，就一次性写入哈希表。
2.  什么时候会出现无值呢？右方不再出现较大值的时候，注意这道题目里由于使用了stack，整个序列都在不停地增长和缩短，没有被写入哈希表的就是这个序列的头部。

```
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) 
    {
        HashMap<Integer,Integer> map = new HashMap<>(nums2.length);
        Stack<Integer> stack = new Stack<>();
        for(int each:nums2)
        {
            while(!stack.isEmpty()&&stack.peek()<each)
            {
                map.put(stack.pop(),each);
            }
            stack.push(each);
        }
        for(int index=0;index<nums1.length;index++)
        {
            nums1[index] = map.getOrDefault(nums1[index],-1);
        }
        return nums1;
    }
}
```

## 844. Backspace String Compare
1. 这又是一道有意思的题目，像这种明显的有方向的问题，从尾部开始考虑就非常简单