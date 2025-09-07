The "Add Two Numbers" problem involves two singly linked lists 
where each node contains a single digit, 
and the digits are stored in reverse order

The goal is to simulate the addition of these two numbers
as if you were doing it by hand, digit by digit, from right to left.
You traverse both lists simultaneously, 
summing corresponding digits along with any carry from the previous step. 

If the sum exceeds 9, you carry over the tens place to the next iteration.

A dummy head node is used to simplify the construction of the result list,
and a pointer moves along as new nodes are added.

This approach ensures that the resulting linked list also 
represents the sum in reverse order. 

The loop continues until both lists are fully traversed and no carry remains. 

It’s a clean example for linked list traversal and pointer manipulation 





### 🧠 Quiz Questions

1. **In the ‘Add Two Numbers’ problem, how are the digits stored in the input linked lists?**

2. **What is the purpose of using a dummy head node in the ‘Add Two Numbers’ problem?**

3. **What happens if the sum of two digits and the carry exceeds 9 in the ‘Add Two Numbers’ problem?**

4. **When does the loop terminate in the ‘Add Two Numbers’ algorithm?**

5. **What programming concept is primarily demonstrated in the ‘Add Two Numbers’ problem?**


