Leetcode

You are given an integer array including both positive and negative numbers.
Your task is to find the contiguous subarray that has the largest possible sum.
Subarray is a sequence of elements from the original array that are next to each other.
;
Thinking about solution,
intuition is negative numbers can reduce the total sum of subarray.
So the trick is to keep track of the current sum as you move through sub array.
if the current sum becomes negative reset it to zero
keep updating the max sum found so far,
This is known as Kadane's algorithm and it works in linear time.