This solution efficiently finds the maximum profit you can achieve from buying and selling a stock once. 
It works by scanning through the prices array only once.

The variable `buy` keeps track of the lowest price seen so far (the best day to buy), 
and `maxProfit` tracks the highest profit found so far. 
For each price in the list, 

if the current price is lower than `buy`,
we update `buy` because that’s a better buying opportunity. 
Otherwise, we calculate the potential profit (`price - buy`), 
and if it’s greater than the current `maxProfit`, we update it. 

By the end, `maxProfit` holds the maximum possible profit. 
This algorithm runs in **O(n)** time and **O(1)** space, which is optimal for this problem.
