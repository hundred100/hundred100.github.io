---
layout:     post   				    # 使用的布局（不需要改）
title:     刷题路程|leetcode:Candy				# 标题 
subtitle:   刷题路程|leetcode:Candy #副标题
date:       2017-02-22				# 时间
author:     hundred 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 总结
---
# 刷题路程|leetcode:Candy

## 题目描述

>There are N children standing in a line. Each child is assigned a rating value.
You are giving candies to these children subjected to the following requirements:
* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.
* What is the minimum candies you must give?

## 解题思路
> 每个小孩至少一颗糖，所以定义一个数组表示每个小孩糖的数量初始化为1。遍历ratings数组，比较前一个和下一个小孩的rating value，如果，下一个比前一个大，则把前一个的糖果数量加一作为下一个的糖果数量，如果，下一个比前一个的小，并且下一个小孩的糖果数量比前一个小孩的糖果数量减一还少，则把前一个的糖果数量减一作为下一个的糖果数量。这里要考虑如果把前一个小孩的糖果数量加一，那么，若前一个的前一个小孩的rating value值更大且他糖果数量不多于前一个小孩的糖果数量，就要把前一个的前一个小孩的糖果数量也加一。

## 代码
```
class Solution {
public:
    int candy(vector<int> &ratings) {
        auto N = ratings.size();
        vector<int> candys(N,1);
        int sum=0;
        for(int i=1;i<N;++i){
            if( ratings[i-1] < ratings[i] ){//后一个小孩比前一个小孩的rating value大
                candys[i]=candys[i-1]+1;
                
            }else if(ratings[i-1] > ratings[i] && (candys[i-1]-1)<candys[i]){//下一个比前一个的小，并且下一个小孩的糖果数量比前一个小孩的糖果数量减一还少
                if(candys[i-1]>1 ){//如果前一个小孩的糖果数量大于1，因为不能小于1
                	candys[i]=candys[i-1]-1;
                    
                }
                else {
                    for(int j=i-1;j>=0 && ratings[j]>ratings[j+1] && candys[j]<=candys[j+1];--j)//若前一个的前一个小孩的rating value值更大且他糖果数量不多于前一个小孩的糖果数量
                    	candys[j]++;
                    	
                }
            }
            
        }
        
        for(const auto &c : candys) sum+=c;
        
        return sum;
    }
};
```
