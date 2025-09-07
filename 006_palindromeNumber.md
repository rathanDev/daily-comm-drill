This solution checks whether a given integer is a palindrome 
by reversing its digits and comparing the result to the original number.

First, it handles a key edge case: 
if the number is negative, it immediately returns false, 
since negative numbers can't be palindromes due to the minus sign. 

Then, it stores the original number in a separate variable (`x1`) to preserve it during the reversal process. 

Inside the loop, it repeatedly extracts the last digit of `x1` using modulo (`% 10`), 
appends it to the reversed number (`rev`), and removes the last digit from `x1` using integer division (`/ 10`). 

Once the entire number is reversed, 
it compares the reversed value to the original input. 

If they match, the number is a palindrome and the method returns true; 
otherwise, it returns false. 

This approach is efficient and avoids string conversion, 
making it suitable for performance-sensitive scenarios and technical interviews.
