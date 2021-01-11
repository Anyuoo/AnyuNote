---
title: DP-最大正方形面积
categories:
  - 算法
tags:
  - 动态规划
  - DP
date: 2020-03-12 21:32:12
---
### 最大正方形面积 ###
在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例：**

    输入: 
    
    1 0 1 0 0
    1 0 1 1 1
    1 1 1 1 1
    1 0 0 1 0
    
    输出: 4

**解答：**


![](F:\GitBlog\source\_posts\DP\dp.png)


**1.边界条件**


**2.状态转移**

    public class MaximalSquare {
    public static int maximalSquare(char[][] matrix) {
        //判断matrix是否存在，不存在返回0
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return 0;
        }
        int height=matrix.length;
        int width=matrix[0].length;
        int max_size=0;
        //创建一个新的二维数组（第一行，第一列为0）存放构成正方形的边长
        int[][] dp=new int[height+1][width+1];
       for (int row=0;row<height;row++){
           for (int col=0;col<width;col++){
               if (matrix[row][col]=='1'){
                   //dp[row+1][col+1]对应原数组matrix[row][col]，判断左上，左，上的边长，要构成新的正方形要以最小边长为准；所以新正方形边长为最小边长加1
                   dp[row+1][col+1]=Math.min(Math.min(dp[row][col+1],dp[row+1][col]),dp[row][col])+1;
                   //最大正方形边长=max（新正方形边长，原最大边长）
                   max_size=Math.max(dp[row+1][col+1],max_size);
               }
           }
       }
        return max_size*max_size;
    }
