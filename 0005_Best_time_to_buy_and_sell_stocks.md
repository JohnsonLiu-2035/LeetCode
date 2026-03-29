# Best_time_to_buy_and_sell_stocks

## 2026.3.23

## description
Input: prices = [7,1,5,3,6,4]   
Output: 5   
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.   
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.  

## solution_1
```python
    def maxProfit(self, prices: List[int]) -> int:
        # 维护两个状态：
        # buy 表示历史最低价，profit 表示当前最大利润
        profit = 0
        buy = prices[0]
        for i in range(1, len(prices)):
            # 如果今天卖出更赚钱，就更新最大利润，不买入
            if prices[i] > buy and prices[i] - buy > profit:
                profit = prices[i] - buy
            # 如果今天价格更低，就更新最低买入价
            elif prices[i] < buy:
                buy = prices[i]
        return profit
```

## solution_2
```python
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        profit = 0
        buy = prices[0]
        for i in range(1, len(prices)):
            # 如果今天价格更低，就更新最低买入价
            buy = min(buy, prices[i])
            # 如果今天卖出更赚钱，就更新最大利润
            profit = max(profit, prices[i] - buy)
        return profit
```

## method
__greedy algorithm贪心算法__，单次遍历，价格低于上一日就更新买入价格（总会比上一日赚钱），高于上一日就计入利润，不买。

## mistakes&lessons
1. 尽量不要用暴力解法，尝试最优算法。  
2. 做题之前先想明白思路，尝试化抽象为具体，想好再动手做。  
3. 需要对比的话可以在循环体前维护变量，更新变量来替代暴力存储。  
4. 用max( ) min( )函数来简化算法，代替if判断。   
