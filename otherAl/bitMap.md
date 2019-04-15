出现在:百度,腾讯,阿里,字节跳动

给一台普通PC，2G内存，要求处理一个包含40亿个不重复并且没有排过序的无符号的int整数，给出一个整数，问如果快速地判断这个整数是否在文件40亿个数据当中？

40亿个int占（40亿*4）/1024/1024/1024 大概为14.9G左右，很明显内存只有2G，放不下，因此不可能将这40亿数据放到内存中计算。要快速的解决这个问题最好的方案就是将数据搁内存了，所以现在的问题就在如何在2G内存空间以内存储着40亿整数。一个int整数在java中是占4个字节的即要32bit位，如果能够用一个bit位来标识一个int整数那么存储空间将大大减少，算一下40亿个int需要的内存空间为40亿/8/1024/1024大概为476.83 mb，这样的话我们完全可以将这40亿个int数放到内存中进行处理。
具体思路：
1个int占4字节即4*8=32位，那么我们只需要申请一个int数组长度为 int tmp[1+N/32]即可存储完这些数据，其中N代表要进行查找的总数，tmp中的每个元素在内存在占32位可以对应表示十进制数0~31,所以可得到BitMap表:
tmp[0]:可表示0~31
tmp[1]:可表示32~63
tmp[2]可表示64~95
.......
那么接下来就看看十进制数如何转换为对应的bit位：
假设这40亿int数据为：6,3,8,32,36,......，那么具体的BitMap表示为：
![image](https://github.com/OnlyHelloWorld/codeReferenceAnswer/blob/master/images/bitMap-1.png)

那么怎么快速定位它的索引呢。如果找到它的索引号，又怎么定位它的位置呢。Index(N)代表N的索引号，Position(N)代表N的所在的位置号。
```
Index(N) = N/32 = N >> 5;
Position(N) = N%32 = N & 0x1F;
```

代码实现:
```java
class BitSet {
    int[] bitset;

    public BitSet(int size) {
        bitset = new int[(size >> 5) + 1]; // divide by 32
    }

    boolean get(int pos) {
        int wordNumber = (pos >> 5); // divide by 32
        int bitNumber = (pos & 0x1F); // mod 32
        return (bitset[wordNumber] & (1 << bitNumber)) != 0;
    }

    void set(int pos) {
        int wordNumber = (pos >> 5); // divide by 32
        int bitNumber = (pos & 0x1F); // mod 32
        bitset[wordNumber] |= 1 << bitNumber;
    }
}
```
