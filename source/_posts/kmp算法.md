---
title: kmp算法
date: 2019-06-25 23:23:24
categories: 算法
tags: [kmp,数据结构]
---

## 前言

​	这两天因为一些原因接触了kmp字串符匹配算法，原理很好理解，就是代码实现费了一番周折，看了《数据结构》这本书有关kmp算法的内容，又看了网上给出的一些解决方案，发现还是书上的代码最简洁、最易理解。

## 一些定义

​	s<sub>1</sub>s<sub>2</sub>.....s<sub>n</sub>：主串

​	t<sub>1</sub>t<sub>2</sub>.....t<sub>n</sub>： 模式串

​	j ：模式串指针

​	i ：主串指针

​	失配 ：s<sub>i</sub> !=  t<sub>j</sub>  

## 原理

​	bp字符串的朴素匹配算法：当`模式`失配时，我们需要把模式指针`j`移动到0，主串指针`i`移动到`i-j+2`处。

​	kmp算法在此基础上进行改进，对于模式串来说，每个字符对应一个`next`值（所有next值组成一个next数组），当模式失配时，主串的指针`i`不变，模式串的指针`j`移动到`next[j]`所对应的位置处。优点在于，kmp算法减少了指针的移动次数。

​	kmp算法的实质：结合下图进行说明

![](http://ptab4lsol.bkt.clouddn.com/kmp1.jpg)

​	如上图所示，当失配情况发生时，模式串[1,7]是显然已经匹配完成的。假如1、2位置处的所代表字符与6、7位置所代表的字符相同，当失配之后进行指针移动时，我们可以直接将1,2,移动到6,7原先所在的位置，大大减少了比较次数，主串指针也无需回溯。模式串向右滑动的距离就取决于next[j]\(此时j==8)。接下来就是如何求解的问题。

## 实现

​	求模式串向右滑动的距离，换句话说就是当失配发生时，主串`i`指针所对应的的字符应当直接和模式串`第k个字符`相比较，`k`的值就是`next[j]`,`j`是失配发生时模式串的指针位置。从上面的例子可以看出，求解`k`所对应的方程式为：t<sub>1</sub>t<sub>2</sub>t<sub>k-1</sub>  = t<sub>j-k+1</sub>t<sub>j-k+2</sub>...t<sub>j-1</sub>  。

​	由此引出求next数组定义：

- next[1] = 0，t<sub>1</sub> != s<sub>i</sub>时，下一步进行t<sub>1</sub>与s<sub>i+1</sub>的比较

- k=1时,next[j] = 1，即不存在相同子串，下一步进行t<sub>1</sub>与s<sub>i</sub>的比较

- next[j] = k，(k尽可能大)
  假如next数组已经求解完成，下面是kmp算法的实现：

  ```java
  public static int kmp(char[] b, char[] c) {
      //b是模式串，c是主串
          int[] next = getNexts(b);
          int j = 1,i = 1;
          while (j < b.length && i < c.length) {
              if (j == 0 || b[j - 1] == c[i - 1]) {
                  i++;j++;
              } else {
                  j = next[j - 1];
              }
          }
          if (j > b.length - 1) return i - b.length;
          return -1;
      }
  ```

  ​	**由于数组是从0开始的，所以涉及的数组的计算都将下标减1。**由上文的介绍，代码很容易理解。

  求next数组的思想跟kmp算法的思想是相似的，实质上是字符串的自身匹配，只不过主串要从1下标开始，模式串要从0下标开始（仔细想想很好理解，因为需要后面的子串和前面的子串相匹配嘛），代码如下：

  ```java
  public static int[] getNexts(char[] b) {
          int[] next = new int[b.length];
          int i = 1,j = 0;
          next[0] = 0;
          while (i < b.length) {
              if (j == 0 || b[i - 1] == b[j - 1]) {
                  i++;j++;next[i - 1] = j;
              } else {j = next[j - 1];}
          }
          return next;
      }
  ```

  还有改进的nextval数组，有空再说咯。

  