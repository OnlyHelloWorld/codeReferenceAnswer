出现在:字节跳动

已有方法 rand7 可生成 1 到 7 范围内的均匀随机整数，试写一个方法 rand10 生成 1 到 10 范围内的均匀随机整数。
不要使用系统的 Math.random() 方法。

练习地址:https://leetcode-cn.com/problems/implement-rand10-using-rand7/

思路:我们可以这样理解,Rand7()-1得到一个离散整数集合{0，1，2，3，4，5，6}，其中每个整数的出现概率都是1/7.那么<code>(rand7()-1)*7</code> 得到一个离散整数集合A={0，7，14，21，28，35，42}，其中每个整数的出现概率也都是1/7。而rand7()得到的集合B={1，2，3，4，5，6，7}中每个整数出现的概率也是1/7。显然集合A和B中任何两个元素组合可以与1-49之间的一个整数一一对应，也就是说1-49之间的任何一个数，可以唯一确定A和B中两个元素的一种组合方式，反过来也成立。由于A和B中元素可以看成是独立事件，根据独立事件的概率公式P(AB)=P(A)P(B)，得到每个组合的概率是<code>1/7*1/7=1/49</code>。因此<code>(rand7()-1)*7+rand7()</code>生成的整数均匀分布在1-49之间，每个数的概率都是1/49。(我们还可以从进制角度考虑,后面分析)

代码实现:
```
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    public int rand10() {
        int v = (rand7()-1)*7+rand7();
        if(v <= 40 )
            return v%10+1;
        return rand10();
    }
}
```

下面我们从进制角度来考虑 Rand5 实现 Rand7:
考虑数值的进制表示。对于一个 N 进制的数，每一位上的值的范围在 <code>[0,N)</code> 之间，并且第 i 位上的值的权值为 Ni。通过这种方式，可以使用 <code>[0,N)</code> 来表示更大的数。

![image](https://github.com/OnlyHelloWorld/codeReferenceAnswer/blob/master/images/leetcode470-1.png)

可以参考以上介绍的数值的进制表示，将 Rand5 产生的 <code>[0,5)</code> 表示成更大的数。两位的五进制的数值范围在 <code>[0,4*5+4) => [0,24)</code>，比 Rand7 范围大。但是大得太多，就会导致转换工作进行过多次，因为如果数在 <code>[7,24)</code> 就需要再进行一次。可以截取 <code>[0, 21)</code> 范围，然后对 7 求余即可解决这个问题。

```
 public static int rand7() {
     int v = rand5() * 5 + rand5();
     if (v < 21) {
         return v % 7;
     }
     return rand7();
 }
 
 ```

