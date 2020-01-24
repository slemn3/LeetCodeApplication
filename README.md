# LeetCodeApplication

I decided to Integer to Roman for this problem. As I went through the proposed solution mentally, there were obvious sub-tasks that needed to be solved.

1. **Number to Roman Lookup**-- There would need to be some sort of conditional, or lookup to translate the designated numerical values to the corresponding Roman Numeral. I opted to implement this as a couple of arrays. One array serves as the numerical look up of designated Roman Integers. The other is the array for the String letter corresponding to the designated integers. An alternate option considered was implementing this as a Dictionary or Map. My approach sacrificed a potential o(1) run time for this particular look up, for a smaller memory footprint. By implementing two arrays, my hope was to avoid creating bulky data structures that required non-primitive data types.

2. **Considering each digit**-- The next big issue to tackle is parsing each digit of the Test Integer. By scanning the number from right to left, I could simplify the number while preserving the magnitude of 10 (e.g. 1994 to 1990 to 1900 etc). 
Reverse-Looping through the digits of the number required two operations per iteration: **1.** increasing the substring "window" for Roman conversion and **2.** subtracting the "seen" value from the overall provided test integer. This allowed me to assess and simplify the integer as my loop made its way left. 
```
int TestInteger = 1194
for(start at the right most index and work left){
  determine the substring window, this will be input for the Roman conversion (in this case 4, then 94 next iteration, then 994 etc.)
  
  call function, providing substring window
  
  subtract substring window from the TestInteger (TestInteger subsequently will be 1994, 1990, 1900, then 1000)
  
  continue until you've made it all the way left  
}
```
This loops through the digits only once, requiring an o(n) runtime. Other costly allocations and operations are the various calls to convert int to string, and vice versa.

3. **Finding the Overflow and Underflow Numbers**-- Simply put, this is the helper methods to find the next designated Roman Integer larger than the Test Integer, as well as the next smallest Roman Integer (e.g. if we are given 3, the overflow is 5 and underflow is 1). I needed these helper methods to help me determine the correct Roman String to append, as well the correct sequence. Each of these helper methods scan the pre-allocated array and are run in o(n) time, and require no additional memory allocation overhead.

4. **Bringing it all together**-- Now that the previous 3 issues have been address, I can now bring it all together and piece together the solution. As mentioned earlier, there is a definite pattern/set of logic for converting the Integer into Roman numeral, so I opted to implement this final piece as a recursive operation. 
The input of the function is the number/digit to assess (from Step 2), index in the array of the overflow value, index in the array of the underflow value
```
function(int valueToConvert, int indexOfOverflow, int indexOfUnderflow)
```
logic is as follows:
  -Base Case: Return 1 or the Roman Numeral String if the input is a designated Integer. This runs in o(1) time
  -
