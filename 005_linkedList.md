The "Add Two Numbers" problem involves two singly linked lists where each node contains a single digit, 
and the digits are stored in reverse order—meaning the 1’s place comes first. 
The goal is to simulate the addition of these two numbers 
as if you were doing it by hand, digit by digit, from right to left.
You traverse both lists simultaneously, summing corresponding digits along with any carry from the previous step. 
If the sum exceeds 9, you carry over the tens place to the next iteration.
A dummy head node is used to simplify the construction of the result list, 
and a pointer moves along as new nodes are added.
This approach ensures that the resulting linked list also represents the sum in reverse order. 
The loop continues until both lists are fully traversed and no carry remains. 
It’s a clean example of how linked list traversal, modular arithmetic, and pointer manipulation come together to solve a real-world numeric problem in a memory-efficient way.
