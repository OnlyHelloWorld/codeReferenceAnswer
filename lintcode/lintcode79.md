出现在:腾讯

找出两个字符串中最大公共子字符串 练习地址:https://www.lintcode.com/problem/longest-common-substring/description

这是一道经典的动态规划的问题,我们通过定义dp二维数组,遍历求解即可
这个题目比较高级的理解就是我们列出一个二维矩阵,如果相等标为1,最长的1对角线即为答案
例如:

显而易见,最长公共子串为:cad.其实到了这一步,我们应该能看出(我看不出)这是个经典的动态规划题目: 由上面的矩阵可以看出状态转移方程:dp[i][j]有A.length()+1行,B.length()+1列.第一行与第一列都为0； [上面我那个矩阵列了A.length()行,B.length()列]
```
dp[i][j] = dp[i-1][j-1];    A[i] == [j];

dp[i][j] = 0;               A[i]!=B[j]    
```
具体代码实现:
```
public class Solution {
    /**
     * @param A: A string
     * @param B: A string
     * @return: the length of the longest common substring.
     */
    public int longestCommonSubstring(String A, String B) {
        //长度为0,直接返回
        if (A.length() == 0 || B.length() == 0)
            return 0;
        int[][] dp = new int[A.length()+1][B.length()+1];
        int maxLength = 0;
        for(int i = 1;i <= A.length();i++){
            for(int j = 1;j <= B.length();j++){
                if(A.charAt(i-1) == B.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = 0;
                }
                if(dp[i][j] > maxLength){
                    maxLength = dp[i][j];
                }
            }
        }
        return maxLength;
    }
}
```
