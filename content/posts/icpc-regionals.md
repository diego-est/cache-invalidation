+++
title = 'We Lost the ICPC Regionals'
date = 2023-08-21T22:00:00-04:00
draft = false
+++

After a grueling five hour match our team couldn't solve a single exercise.
We were very out of it at the end and just couldn't find a way to solve any of
the exercises.
Even though the exercises were pretty hard some of our solutions were indeed
correct but couldn't beat the time limit they gave us for those exercises.

For example one of the exercises was to find out all the numbers between 2 and N
that fit the following description:
For x in [2, N], N base x is a palindrome.
The complicated part of this problem being the fact that the largest N they give
you is 2^15 and your time limit is 1 second.

Changing a decimal number to a different base will always take an `O(log n)`
amount of steps; checking if it's a palindrome will take `O(n/2)` steps max;
doing that for every number in [2, N] is `O(n log(n))`.
Note that an average computer can only do 10^8 calculations per second, but our
largest number is 2^15 which means that anything equal or slower in complexity
than a linear solution would be too slow.

During the competition several things we noticed were that N in [3, N) will
always have at least 1 palindrome in the base N-1.
That is because you can always express N as (1 * (N-1) + 1).
The next palindrome number would be 22 but wwe couldn't notice why this
palindrome would only sometimes appear.

For the sake of explanation I will refer to `palindrome-pairs` as a palindrome
made of 2 digits.
After the competition concluded, on the ride back, our professor had mentioned
that you will always have palindrome-pairs made of the factors of that number
strictly less than that number's square root.
Which makes sense for our first example since every number has the number 1 as a
factor.
Furthermore, the number 36, whose factors are: { 1, 2, 3, 4, 6, 9, 18, 36 } can
be expressed as:
 - 11 in base 35
 - 22 in base 17
 - 33 in base 11
 - 44 in base 8
 - 121 in base 6 awwww
It was almost working.

I say almost because 121 IS a palindrome but it's also not a palindrome-pair.
Unfortunately we couldn't come up with any better algorithm for this and ended
up submitting this solution twice and getting TLE'd.
