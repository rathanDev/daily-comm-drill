The problem asks you to remove all instances of a given value `val` from an array `nums`, 
Modify the array in-place without using extra space.
You're expected to return the count `k` of elements that remain after the removal, 
The reminaing ones are not equal to `val`.
Importantly, the first `k` elements of the array should contain these valid values, 
though their order doesn’t matter and anything beyond the `k`th index is irrelevant.
;
The solution uses a simple loop and
a pointer `k` to track where to place the next valid element.
As you iterate through `nums`, 
each time you find a value not equal to `val`,
you copy it to the `k`th position and increment `k`.
This ensures that the first `k` elements of `nums` are the ones you want to keep, 
and `k` itself is the final answer.




track
ensure
