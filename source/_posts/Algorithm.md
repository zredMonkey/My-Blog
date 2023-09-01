---
title: Algorithm
date: 2023-08-01 12:27:18
tags:
- 算法
- 动态规划
- 贪心算法
- 回溯算法
categories:
- 算法
- 动态规划
- 回溯算法
top_img: /img/suanfa.png
cover: /img/suanfa.png
keywords: "Algorithm"
description: "算法（Algorithm）是指解题方案的准确而完整的描述，是一系列解决问题的清晰指令，算法代表着用系统的方法描述解决问题的策略机制。也就是说，能够对一定规范的输入，在有限时间内获得所要求的输出。"
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both 
    date_format: relative
    categories: true 
    tags: true
    label: true
---
# 一、动态规划

动态规划（Dynamic Programming，简称DP）是一种常用于解决优化问题和计数问题的算法思想。它通过**将一个复杂问题分解为若干个子问题，然后逐步解决这些子问题，最终得到原问题的解**。==动态规划的核心思想是“递推”和“存储”，即通过已经解决的子问题的解来推导出更大规模问题的解，并将这些子问题的解进行存储以避免重复计算，从而提高算法的效率==。

动态规划适用于满足以下两个条件的问题：

1. 重叠子问题（Overlapping Subproblems）：问题的解可以被分解为多个子问题，而且这些子问题之间存在重叠，即同一个子问题可能会被多次求解。

2. 最优子结构（Optimal Substructure）：问题的最优解可以由其子问题的最优解推导而来。换句话说，问题的整体最优解可以通过子问题的最优解组合而成。

动态规划通常有两种常见的方法：

1. 自顶向下（Top-Down）：也称为记忆化递归，通过递归地解决问题，但在求解子问题时使用数组等数据结构来存储已经计算过的解，以避免重复计算。

2. 自底向上（Bottom-Up）：通过解决问题的子问题，从最小规模的问题开始，逐步构建解决大规模问题的方法。这种方法通常会使用一个数组或表格来存储子问题的解，以便后续问题可以直接从已经计算出的解中获取。

动态规划广泛应用于许多领域，例如：

- 背包问题（Knapsack Problem）
- 最短路径问题（Shortest Path Problem）
- 最长公共子序列问题（Longest Common Subsequence Problem）
- 斐波那契数列问题（Fibonacci Sequence Problem）
- 编辑距离问题（Edit Distance Problem）
- 最大子数组和问题（Maximum Subarray Sum Problem）
- …等等

总之，动态规划是一种通过将问题分解为子问题，逐步构建解决方案并存储已计算的结果来优化问题求解过程的算法思想。


**大致步骤：**
1. **定义子问题：** 将原始问题分解为若干个更小的子问题。这些子问题应该满足最优子结构，即问题的最优解可以由子问题的最优解推导而来。

2. **找出状态转移方程：** 对每个子问题定义一个状态，然后找出子问题之间的关系，即状态之间的转移方程。这个方程描述了子问题的解与其相关子问题解之间的关系。

3. **初始化：** 初始化一些基本的子问题，通常是最小规模的问题的解。这些初始化值将作为构建更大规模问题解的基础。

4. **自底向上求解（或者记忆化递归）：** 通过迭代地求解子问题，从最小规模的问题开始，逐步构建解决大规模问题的方法。这可以通过自底向上的迭代方法实现，或者使用自顶向下的记忆化递归方法，其中使用数组等数据结构来存储已经计算过的解。

5. **存储中间结果：** 在求解子问题的过程中，将已经计算过的子问题的解存储起来，以避免重复计算，提高算法效率。

6. **得到最终解：** 当所有子问题都求解完毕后，最终问题的解就可以从中获得。这通常是整个问题的最优解。

7. **可选的优化：** 根据具体情况，你还可以对算法进行进一步优化，例如利用滚动数组、状态压缩等技巧来减少空间复杂度。

## 最长递增子序列

**题目：**
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。
**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

示例 1：
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。

示例 2：
输入：nums = [0,1,0,3,2,3]
输出：4

示例 3：
输入：nums = [7,7,7,7,7,7,7]
输出：1
 
提示：
1 <= nums.length <= 2500
-104 <= nums[i] <= 104

**注意**「⼦序列」和「⼦串」这两个名词的区别，⼦串⼀定是连续的，⽽⼦序列不⼀定是连续的


**思想解释：**
1. 我们使用一个数组 `dp` 来保存以每个元素结尾的最长递增子序列的长度。初始时，每个元素自成一个长度为1的子序列。
2. 我们从数组的第二个元素开始遍历，对于每个元素 `nums[i]`，我们再遍历它之前的所有元素 `nums[j]`（`j < i`）。如果 `nums[i]` 大于 `nums[j]`，说明可以将 `nums[i]` 加入以 `nums[j]` 结尾的子序列，从而构成一个更长的递增子序列。我们更新 `dp[i]` 为 `dp[j] + 1`，表示以 `nums[i]` 结尾的最长递增子序列长度。
3. 在内层循环中，我们不断更新 `dp[i]`，找到以当前元素 `nums[i]` 结尾的最长递增子序列长度。
4. 在整个过程中，我们维护一个全局变量 `maxLen`，记录最长递增子序列的长度。
5. 最终，遍历完整个数组后，`maxLen` 就是最长递增子序列的长度。


```java
public class LongestIncreasingSubsequence {
    public static int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int[] dp = new int[nums.length]; // dp[i] 表示以 nums[i] 结尾的最长递增子序列长度
        dp[0] = 1; // 初始化，单个元素也构成递增子序列
        
        int maxLen = 1; // 最长递增子序列长度
        for (int i = 1; i < nums.length; i++) {
            dp[i] = 1; // 默认以当前元素为结尾的子序列长度为 1
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1); // 更新最长递增子序列长度
                }
            }
            maxLen = Math.max(maxLen, dp[i]); // 更新全局最长长度
        }
        
        return maxLen;
    }

    public static void main(String[] args) {
        int[] nums = {10, 9, 2, 5, 3, 7, 101, 18};
        int lisLength = lengthOfLIS(nums);
        System.out.println("Length of Longest Increasing Subsequence: " + lisLength);
    }
}
```


## 正则表达式匹配


**题目：**
请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。

示例 1:
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

示例 2:
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。

示例 3:
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。

示例 4:
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。

示例 5:
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母以及字符 . 和 *，无连续的 '*'。

**解题思路如下：**
1. 我们可以使用动态规划来解决正则表达式匹配问题。
2. 定义一个二维布尔数组dp，其中dp[i][j]表示字符串的前i个字符与正则表达式的前j个字符是否匹配。
3. 初始化dp[0][0]为true，表示空字符串和空正则表达式是匹配的。
4. 遍历字符串和正则表达式的每个字符，逐步填充dp数组。
5. 如果s[i]和p[j]相等，或者p[j]为'.'，则dp[i][j]的值取决于dp[i-1][j-1]，表示当前字符匹配成功。
6. 如果p[j]为'*'，则需要考虑两种情况：

```
-  '*'表示前面的字符重复0次，则dp[i][j]的值取决于dp[i][j-2]。
-  '*'表示前面的字符重复1次或多次，则dp[i][j]的值取决于dp[i-1][j]且s[i]和p[j-1]相等，或者p[j-1]为'.'。
```

7. 其他情况下，dp[i][j]的值为false，表示当前字符匹配失败。
8. 最终返回dp[len(s)][len(p)]的值，表示整个字符串与正则表达式是否匹配。


```java
public class RegularExpressionMatching {
    public static boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        
        // dp[i][j]表示s的前i个字符和p的前j个字符是否匹配
        boolean[][] dp = new boolean[m + 1][n + 1];
        
        // 空字符串和空正则表达式匹配
        dp[0][0] = true;
        
        // 处理空正则表达式可以匹配的情况
        for (int j = 1; j <= n; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }
        
        // 填充dp表格
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                char sc = s.charAt(i - 1);
                char pc = p.charAt(j - 1);
                
                if (sc == pc || pc == '.') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (pc == '*') {
                    char prevPc = p.charAt(j - 2);
                    if (prevPc == sc || prevPc == '.') {
                        // 匹配0次、1次或多次
                        dp[i][j] = dp[i][j - 2] || dp[i - 1][j] || dp[i - 1][j - 2];
                    } else {
                        // 匹配0次
                        dp[i][j] = dp[i][j - 2];
                    }
                }
            }
        }
        
        return dp[m][n];
    }

    public static void main(String[] args) {
        String s = "mississippi";
        String p = "mis*is*p*.";
        
        boolean result = isMatch(s, p);
        System.out.println("Is match: " + result);
    }
}
```
注释解释：
1. `dp[i][j]`表示s的前i个字符和p的前j个字符是否匹配。
2. 初始化：空字符串和空正则表达式匹配，`dp[0][0] = true`。
3. 处理空正则表达式可以匹配的情况：如果p的某个字符是'*'，那么它可以匹配0次，将`dp[0][j]`设置为`dp[0][j-2]`。
4. 填充dp表格：根据字符匹配和'*'的特性，更新`dp[i][j]`的值。
5. 最终结：`dp[m][n]`表示s的全部字符和p的全部字符是否匹配。

## 最长回文子串
回文串是指正着读和倒着读都一样的字符串。例如，"aba"、"abba"和"level"都是回文串。

**题目：**
给你一个字符串 s，找到 s 中最长的回文子串。
如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。


示例 1：
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。

示例 2：
输入：s = "cbbd"
输出："bb"
 
提示：
1 <= s.length <= 1000
s 仅由数字和英文字母组成

**解题思路：**
1. 我们可以使用动态规划来解决最长回文子串问题。定义一个二维数组dp，其中dp[i][j]表示从索引i到j的子串是否为回文串。
2. 初始化dp数组，将所有长度为1的子串都设为回文串，即dp[i][i] = true。
3. 遍历字符串中所有可能的子串，从长度为2的子串开始，到长度为n的子串结束（n为字符串长度）。
4. 对于每个子串，判断头尾两个字符是否相等，并根据之前计算的dp数组来判断子串是否为回文串，即dp[i][j] = (s[i] == s[j]) && dp[i+1][j-1]。
5. 如果当前子串是回文串并且长度比之前的最长回文串更长，更新最长回文串的起始位置和长度。
6. 最终得到的最长回文子串就是最长的回文串。

**解法：**

```java
public class LongestPalindromeSubstring {
    public static String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n]; // dp[i][j]表示s的子串从i到j是否为回文子串
        int start = 0; // 记录最长回文子串的起始位置
        int maxLength = 1; // 记录最长回文子串的长度
        
        // 所有单个字符都是回文子串
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        
        // 检查长度为2的子串
        for (int i = 0; i < n - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                dp[i][i + 1] = true;
                start = i;
                maxLength = 2;
            }
        }
        
        // 检查长度大于2的子串
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1; // 子串的结束位置
                if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                    dp[i][j] = true;
                    start = i;
                    maxLength = len;
                }
            }
        }
        
        return s.substring(start, start + maxLength);
    }

    public static void main(String[] args) {
        String input = "babad";
        String longestPalindrome = longestPalindrome(input);
        System.out.println("Longest Palindrome Substring: " + longestPalindrome);
    }
}
```

注释解释：
1. `dp[i][j]`表示s的子串从索引i到j是否是回文子串。
2. 初始化：所有单个字符都是回文子串，即`dp[i][i] = true`。
3. 检查长度为2的子串：如果相邻两个字符相等，那么它们是回文子串，即`dp[i][i+1] = true`。
4. 检查长度大于2的子串：通过动态规划，依次计算长度为3到n的所有子串是否为回文子串。
5. 最终结果：根据`dp`数组的信息，找到最长回文子串的起始位置和长度，然后通过`substring`方法获取最长回文子串。
注意：虽然动态规划是一种解决最长回文子串问题的方法，但还有其他更优秀的算法，如Manacher算法等，可以在时间复杂度上做更多优化。

## 回文子串个数
**题目：**
给定一个字符串 s ，请计算这个字符串中有多少个回文子字符串。
具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。


示例 1：
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"

示例 2：
输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
 
提示：
1 <= s.length <= 1000
s 由小写英文字母组成

**解法**：

```java
public class CountPalindromicSubstrings {
    public static int countSubstrings(String s) {
        int n = s.length();
        int count = 0; // 记录回文子串的个数
        boolean[][] dp = new boolean[n][n]; // dp[i][j]表示s的子串从i到j是否为回文子串
        
        // 所有单个字符都是回文子串
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
            count++;
        }
        
        // 检查长度为2的子串
        for (int i = 0; i < n - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                dp[i][i + 1] = true;
                count++;
            }
        }
        
        // 检查长度大于2的子串
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1; // 子串的结束位置
                if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                    dp[i][j] = true;
                    count++;
                }
            }
        }
        
        return count;
    }

    public static void main(String[] args) {
        String input = "abc";
        int palindromeCount = countSubstrings(input);
        System.out.println("Palindrome Substrings Count: " + palindromeCount);
    }
}
```

注释解释：
1. `dp[i][j]`表示s的子串从索引i到j是否是回文子串。
2. 初始化：所有单个字符都是回文子串，即`dp[i][i] = true`，并且`count`加1。
3. 检查长度为2的子串：如果相邻两个字符相等，那么它们是回文子串，即`dp[i][i+1] = true`，并且`count`加1。
4. 检查长度大于2的子串：通过动态规划，依次计算长度为3到n的所有子串是否为回文子串，如果是，则`dp[i][j]`为true，同时`count`加1。
5. 最终结果：返回`count`，即回文子串的个数。


## 背包问题
背包问题是动态规划中的经典问题之一。给定一组物品，每个物品有对应的重量和价值，背包有限的承载重量，要求在不超过背包承载重量的前提下，选择物品放入背包，使得背包中物品的总价值最大。

**解题思路：**

1. 我们可以使用动态规划来解决0-1背包问题。首先，定义一个二维数组`dp`，其中`dp[i][j]`表示在前i个物品中选择总重量不超过j的情况下的最大价值。
2. 初始化`dp`数组，将第0行和第0列的值都设为0，表示没有物品或背包承载重量为0时的最大价值为0。
3. 遍历物品和背包承载重量，对于每个物品和背包承载重量：
- 如果物品i的重量大于当前背包承载重量j，说明物品i不能放入背包，所以`dp[i][j]`的最大价值和`dp[i-1][j]`一样。
- 如果物品i的重量小于等于当前背包承载重量j，我们可以考虑是否将物品i放入背包。如果放入物品i，则背包中剩余的重量为`j - weights[i]`，所以最大价值为`values[i] + dp[i-1][j-weights[i]]`；如果不放入物品i，则最大价值为`dp[i-1][j]`。我们选择两者中较大的值作为`dp[i][j]`的最大价值。其中 `weight[i]` 为物品 i 的重量，`value[i]` 为物品 i 的价值。
4. 最后`dp[n][W]`即为问题的解，其中n表示物品的个数，W表示背包的承载重量。



```
public class KnapsackProblem {

    /**
     *
     * @param weights    物品的重量数组
     * @param values     价值数组
     * @param capacity   背包容量
     * @return 最大价值
     */
    public static int knapsack(int[] weights, int[] values, int capacity) {

        int n = weights.length;

        // 创建一个二维数组来保存状态转移结果，dp[i][j] 表示在前 i 个物品中，背包容量为 j 时的最大价值
        int[][] dp = new int[n + 1][capacity + 1];

        // 填充 dp 数组，进行状态转移
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= capacity; j++) {
                // 如果当前物品的重量大于当前背包容量，无法放入，直接继承上一行的最大价值
                if (weights[i - 1] > j) {
                    dp[i][j] = dp[i - 1][j];
                } else {
                    // 否则，可以选择放入当前物品或不放入，取两者中的最大值
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - weights[i - 1]] + values[i - 1]);
                }
            }
        }

        // 返回最终结果，即在考虑所有物品后，背包容量为 capacity 时的最大价值
        return dp[n][capacity];
    }

    public static void main(String[] args) {
        int[] weights = {2, 3, 4, 5};
        int[] values = {3, 4, 5, 6};
        int capacity = 5;
        int result = knapsack(weights, values, capacity);
        System.out.println("Maximum value: " + result);
    }
}

```

```
// 输出结果
Maximum value: 7

Process finished with exit code 0
```

## 最长公共子序列（Longest Common Subsequence）问题

最长公共子序列（Longest Common Subsequence，简称LCS）问题是一种经典的动态规划问题，用于找到两个序列中最长的公共子序列的长度。

给定两个序列A和B，我们要找到它们的最长公共子序列。子序列是指从序列中删除零个或多个元素而不改变其相对顺序后得到的新序列。

**解题思路：**
1. 我们可以使用动态规划来解决LCS问题。首先，定义一个二维数组`dp`，其中`dp[i][j]`表示序列A的前i个元素和序列B的前j个元素的最长公共子序列的长度。
2. 初始化`dp`数组，将第0行和第0列的值都设为0，表示当一个序列为空时，与任何序列的最长公共子序列长度都为0。
3. 遍历两个序列的元素，对于每个元素`A[i]`和`B[j]`：
-  如果`A[i]`和`B[j]`相等，说明它们可以作为最长公共子序列的一部分，因此`dp[i][j]`的值应该是`dp[i-1][j-1] + 1`，即在之前的最长公共子序列长度上加1。
-  如果`A[i]`和`B[j]`不相等，说明它们不能同时出现在最长公共子序列中，此时我们需要考虑舍弃其中一个元素，即取`dp[i-1][j]`和`dp[i][j-1]`中的较大值作为`dp[i][j]`的值。
4. 最后，`dp[n][m]`即为序列A和B的最长公共子序列的长度，其中n和m分别是序列A和B的长度。


```
public class LongestCommonSubsequence {

    public static int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();

        // 创建一个二维数组 dp，dp[i][j] 表示 text1 前 i 个字符和 text2 前 j 个字符的最长公共子序列长度
        int[][] dp = new int[m + 1][n + 1];

        // 填充 dp 数组，进行状态转移
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // 如果当前字符相同，说明可以将这个字符纳入公共子序列中，长度加一
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    // 否则，选择不使用当前字符，取前面最长的公共子序列长度
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // 返回 text1 和 text2 的最长公共子序列长度
        return dp[m][n];
    }

    public static void main(String[] args) {
        String text1 = "abcde";
        String text2 = "ace";
        int result = longestCommonSubsequence(text1, text2);
        System.out.println("Longest Common Subsequence: " + result);
    }
}
```

```
Longest Common Subsequence: 3

Process finished with exit code 0
```


## 打家劫舍（House Robber）问题：如不相邻的房屋偷窃的最大金额。
**题目：**

一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响小偷偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组 nums ，请计算 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

 

示例 1：

输入：nums = [1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。

示例 2：

输入：nums = [2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
 

提示：
- 1 <= nums.length <= 100
- 0 <= nums[i] <= 400


**解题思路：**

1. 我们可以使用动态规划来解决这个问题。定义一个一维数组dp，其中dp[i]表示偷窃前i个房屋能够得到的最大金额。
2. 初始化dp数组，dp[0]为第一个房屋的金额，dp[1]为第二个房屋和第一个房屋金额的较大值。
3. 遍历数组，对于每个房屋i，考虑两种情况：
- 偷窃第i个房屋：则最大金额为dp[i-2]+nums[i]，即偷窃第i-2个房屋的最大金额加上第i个房屋的金额。
- 不偷窃第i个房屋：则最大金额为dp[i-1]，即偷窃前i-1个房屋的最大金额。
- 取两种情况中的较大值作为dp[i]的值。
4. 最终，dp[n-1]即为偷窃到的最大金额，其中n为房屋的个数。



```
public class HouseRobberI {

    /**
     * @param nums 一个代表每个房屋存放金额的非负整数数组
     * @return
     */
    public static int rob(int[] nums) {

        int n = nums.length;

        if (n == 0) {
            return 0;
        }

        if (n == 1) {
            return nums[0];
        }

        // dp[i] 表示偷窃前 i 个房屋能够获得的最大金额
        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            // 在当前房屋偷窃或不偷窃之间选择最大值
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
        }

        // 返回最后一个房屋偷窃或不偷窃的最大金额
        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 9, 3, 1};
        int result = rob(nums);
        System.out.println("Maximum amount: " + result);
    }
}
```

```
Maximum amount: 12

Process finished with exit code 0
```


# 二、回溯算法
回溯算法是一种穷举搜索的算法，其核心思想是通过递归的方式尝试所有可能的情况，直到找到问题的解或确定问题无解。在搜索过程中，当发现当前选择无法达到目标或导致问题无解时，会回退到上一步选择另一种可能，继续尝试。

回溯算法的基本步骤如下：
- 确定问题的解空间：即问题所有可能的解组成的空间。这个空间可能是一个树状结构，每个节点表示一个可能的选择。
- 递归地搜索解空间：从根节点开始，对每个节点进行深度优先搜索，考虑当前节点的选择并继续向下搜索。如果搜索到达叶子节点且得到一个有效解，或者搜索无法继续进行，则回退到上一层节点，选择另一种可能继续搜索。
- 剪枝优化：在搜索过程中，通过某些条件判断可以提前结束不可能得到解的搜索，从而减少不必要的计算。
 

回溯算法可以用来解决组合问题、排列问题、子集问题、棋盘问题等。当问题的解空间较大且搜索的过程中需要考虑选择与限制条件时，回溯算法通常是一种有效的解题思路。

回溯算法可以用来解决组合问题、排列问题、子集问题、棋盘问题等。当问题的解空间较大且搜索的过程中需要考虑选择与限制条件时，回溯算法通常是一种有效的解题思路。

一些常见的回溯算法问题包括：
- 全排列（Permutations）
- 组合求和（Combination Sum）
- 子集（Subsets）
- N皇后问题（N-Queens）
- 单词搜索（Word Search）
- 正则表达式匹配（Regular Expression Matching）等。

回溯算法的灵活性和穷举性使得它适用于解决许多复杂的组合问题和排列问题。然而，由于其穷举搜索的性质，对于一些大规模问题，回溯算法的计算复杂度可能会非常高。因此，在实际应用中，对于问题规模较大的情况，可能需要结合其他优化方法来提高算法效率。

**回溯算法框架：**
```
List<Value> result;
void backtrack(路径， 选择列表) {
    if (满足结束条件) {
        result.add(路径);
        return;
    }
    for (选择 ： 选择列表) {
        做选择;
        backtrack(路径， 选择列表);
        撤销选择;
    }
}
```

## 全排列
全排列（Permutations）问题是一个经典的回溯算法问题。给定一个不含重复元素的整数数组，要求返回所有可能的排列方式。

**解题思路如下：**
1. 首先，我们可以使用递归来实现回溯算法。
1. 使用一个辅助函数来递归地生成排列。函数参数包括当前排列的状态、已使用的数字集合、原始整数数组，以及存储所有排列结果的变量。
1. 递归的终止条件是当前排列的长度等于原始整数数组的长度，表示当前排列已经完成，将其添加到结果中。
1. 在递归过程中，遍历未使用的数字，每次选择一个数字加入当前排列中，并将其标记为已使用，然后继续递归生成下一个位置的排列。
1. 在回溯的过程中，将已使用的数字状态恢复，以便尝试其他的选择。

**题目：**

给定一个不含重复数字的整数数组 nums ，返回其 所有可能的全排列 。可以 按任意顺序 返回答案。

 
示例 1：
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

示例 2：
输入：nums = [0,1]
输出：[[0,1],[1,0]]

示例 3：
输入：nums = [1]
输出：[[1]]
 
提示：
1 <= nums.length <= 6
-10 <= nums[i] <= 10
nums 中的所有整数 互不相同



```
import java.util.ArrayList;
import java.util.List;

public class PermutationsBacktracking {

    public static void main(String[] args) {
        int[] nums = {1, 2, 3};
        List<List<Integer>> permutations = permute(nums);
        for (List<Integer> permutation : permutations) {
            System.out.println(permutation);
        }
    }

    // 初始化了一个空的结果列表
    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        // 全排列
        backtrack(nums, new ArrayList<>(), result);
        return result;
    }


    /**
     * 回溯函数，用于生成全排列
     * @param nums      数组 【1，2，3】
     * @param track     记录路径  进入一个新节点，要加入这个节点的元素；退后一个节点时，要删除这个节点元素
     * @param result    存储全排列结果
     */
    private static void backtrack(int[] nums, List<Integer> track, List<List<Integer>> result) {
        // 到达叶子节点，将路径装入结果列表
        if (track.size() == nums.length) {
            result.add(new ArrayList<>(track));
            return;
        }

        // 尝试每个未使用的元素
        for (int num : nums) {
            if (track.contains(num)) {
                // 如果当前数字已经在排列中，跳过
                continue;
            }
            track.add(num);
            // 递归调用
            backtrack(nums, track, result);
            // 回溯，移除最后一个元素
            track.remove(track.size() - 1);
        }
    }
```

```
[1, 2, 3]
[1, 3, 2]
[2, 1, 3]
[2, 3, 1]
[3, 1, 2]
[3, 2, 1]

Process finished with exit code 0
```

# 三、贪心算法

**核心思想：**
贪心算法（Greedy Algorithm）的核心思想是在每一步选择中都采取当前最优的选择，以期望达到全局最优解。换句话说，贪心算法每次都做出局部最优的选择，希望通过局部最优解的组合，达到整体最优解。

贪心算法的特点是不回溯，不考虑选择对未来的影响，而只关注当前的局部最优解。贪心算法在解决一些最优化问题时，可以快速找到一个近似最优解，但并不保证一定能找到全局最优解。因此，贪心算法通常用于那些具有贪心选择性质和最优子结构性质的问题。

**贪心算法的一般步骤如下**：

1. 定义问题的解空间，即所有可能的解组成的集合。
1. 制定选择策略，即在每一步都选择一个局部最优解。
1. 确定是否满足问题的约束条件，即该解是否可行。
1. 判断是否达到问题的目标，即该解是否是最优解。如果达到目标，则算法结束；否则，返回步骤2，继续进行选择。

需要注意的是，由于贪心算法每次只考虑当前的局部最优解，因此并不适用于所有问题。在某些情况下，贪心算法可能会得到次优解或错误的结果。因此，在应用贪心算法时，需要仔细分析问题的特点，确保问题具有贪心选择性质和最优子结构性质，以保证算法能够得到正确的结果。

**常见算法题：**

1. 零钱兑换（Coin Change）：给定不同面额的硬币和一个总金额，求出使用最少的硬币数量凑成总金额。
1. 区间调度（Interval Scheduling）：给定一组区间，选择尽可能多的不重叠区间。
1. 分糖果（Candy）：给定一组孩子和一些糖果，分配糖果使得每个孩子至少分得一颗，相邻孩子间的糖果数应尽可能不同。
1. 买卖股票的最佳时机（Best Time to Buy and Sell Stock）：给定一组股票的价格，只能买卖一次，求最大的利润。
1. 跳跃游戏（Jump Game）：给定一个非负整数数组，每个元素代表在该位置可以跳跃的最大步数，判断能否到达数组的最后一个位置。
1. 柠檬水找零（Lemonade Change）：给定一组客户支付的钞票面额，判断是否能找零（客户支付5、10、20元，柠檬水5元一杯）。
1. 汽车加油站（Gas Station）：给定一个环形路线上的加油站和对应的汽油量，选择一个起始加油站，判断是否能绕一圈回到起点，并找出可能的起始加油站。
1. 分发饼干（Assign Cookies）：给定一组孩子和一组饼干，每个孩子有一个满足度，每个饼干有一个大小，求能满足孩子的最大数量。
1. 非重叠区间（Non-overlapping Intervals）：给定一组区间，移除最少的区间，使得剩余的区间互不重叠。
1. 跳跃游戏 II（Jump Game II）：给定一个非负整数数组，每个元素代表在该位置可以跳跃的最大步数，求最少需要几步能够到达数组的最后一个位置。


## 零钱兑换（Coin Change）：给定不同面额的硬币和一个总金额，求出使用最少的硬币数量凑成总金额。

在零钱兑换问题中，贪心算法并不是最优的解决方案，因为不是所有情况下都可以通过贪心选择得到最少硬币数量。但是，为了展示贪心算法的思想，我们可以尝试使用贪心算法解决一部分情况。

贪心算法的思想是每次都选择当前最优的硬币面额，然后尽可能多地使用该面额的硬币，直到凑满总金额。在这道题中，我们可以使用贪心算法来得到一个近似最优解，但并不能保证一定能得到全局最优解。

**解：**
```
public class CoinChangeGreedy {

    public static void main(String[] args) {
        int[] coins = {1, 2, 5}; // 零钱的面额
        int amount = 11; // 要兑换的金额
        int numCoins = coinChange(coins, amount);
        System.out.println("最少需要的硬币数：" + numCoins);
    }

    /**
     *
     * @param coins    硬币面额
     * @param amount   要兑换的金额
     * @return 最少使用硬币数目
     */
    public static int coinChange(int[] coins, int amount) {
        // 面额按从小到大排序
        Arrays.sort(coins);

        // 记录硬币数量
        int count = 0;
        // 从最大面额的硬币开始尝试
        int index = coins.length - 1;

        while (amount > 0 && index >= 0) {

            if (coins[index] <= amount) {
                // 尝试使用当前面额的硬币数量
                int numCoins = amount / coins[index];
                count += numCoins;
                amount -= numCoins * coins[index];
            }
            // 尝试下一个面额的硬币
            index--;
        }

        // 如果剩余金额为0，返回硬币数量，否则返回-1表示无法兑换
        return amount == 0 ? count : -1;
    }
}
```

```
最少需要的硬币数：3

Process finished with exit code 0
```

在这个示例中，我们通过贪心算法按照面额从大到小的顺序，尽量使用大面额的硬币来兑换金额。==注意==，贪心算法并不一定能得到最优解，有时候可能会得到错误的结果。例如，如果硬币面额为{1, 3, 4}，要兑换6元，贪心算法会选择使用3个硬币（4 + 1 + 1），但实际上最优解应该是2个硬币（3 + 3）。

在实际应用中，零钱兑换问题更适合使用动态规划等算法来求解，以确保得到最优解。

