# LeetCodeApplication

I decided to Integer to Roman for this problem. As I went through the proposed solution mentally, there were obvious sub-tasks that needed to be solved.

1. **Number to Roman Lookup**-- There would need to be some sort of conditional, or lookup to translate the designated numerical values to the corresponding Roman Numeral. I opted to implement this as a couple of arrays. One array serves as the numerical look up of designated Roman Integers. The other is the array for the String letter corresponding to the designated integers. An alternate option considered was implementing this as a Dictionary or Map. My approach sacrificed a potential o(1) run time for this particular look up, for a smaller memory footprint. By implementing two arrays, my hope was to avoid creating bulky data structures that required non-primitive data types.

2. **Considering each digit**-- The next big issue to tackle is parsing each digit of the Test Integer. By scanning the number from right to left, I could simplify the number while preserving the magnitude of 10 (e.g. 1994 to 1990 to 1900 etc). 
Reverse-Looping through the digits of the number required two operations per iteration: **1.** increasing the substring "window" for Roman conversion and **2.** subtracting the "seen" value from the overall provided test integer. This allowed me to assess and simplify the integer as my loop made its way left. 
```
int TestInteger = 1994
for(start at the right most index and work left){
  determine the substring window, this will be input for the Roman conversion (in this case 4, then 94 next iteration, then 994 etc.)
  
  call function, providing substring window
  
  subtract substring window from the TestInteger (TestInteger subsequently will be 1994, 1990, 1900, then 1000)
  
  continue until you've made it all the way left  
}
```
This loops through the digits only once, requiring an o(n) runtime. Other costly allocations and operations are the various calls to convert int to string, and vice versa.

3. **Finding the Overflow and Underflow Numbers**-- Simply put, this is the helper methods to find the next designated Roman Integer larger than the Test Integer, as well as the next smallest Roman Integer (e.g. if we are given 3, the overflow is 5 and underflow is 1). I needed these helper methods to help me determine the correct Roman String to append, as well the correct sequence. Each of these helper methods scan the pre-allocated array and are run in o(n) time, and require no additional memory allocation overhead.

4. **Bringing it all together**-- Now that the previous 3 issues have been address, I can now bring it all together and implement the overall solution. There is a definite pattern/set of logic for converting the Integer into Roman numeral, so I opted to implement this final piece as a recursive operation. 
The input of the function is the Substring Window from Step 2, overflow value, underflow value. Due to the decision to implement the lookup Roman Characters and Integers as an Array, I pass back and forth indexes since this will require o(1) lookup.
```
function(int valueToConvert, int indexOfOverflow, int indexOfUnderflow){
```
logic is as follows:
  -Base Case: Return 1 or the Roman Numeral String if the input is a designated Integer. This requires o(n) time since I will need to do a full list scan of the Array for the Roman String
  -Determine which Numeral and Sequence to append
 ```
 if(valueToConvert is 1/3 or less away from the overflow)
  append overflow Roman Numeral + recursive call to function (overflow - valueToConvert, new overflow, new underflow)
 else
  append recursive call to function (valueToConvert - underflow, new overflow, new underflow) + underflow Roman Numeral
 ```
The logic above is overly simplified for ease of reading. One tricky thing to note is that 8 seems to be the magic number that messes up the logic. If I were to simply look at the absolute value of the valueToConvert to overflow and underflow, 8 would be IIIX. In order to get VIII in the above logic, I need to check distnce of valueToConvert is less than 1 * magnitude of 10 of Substring Window away from the overflow (e.g. 8 is 2 away from overflow 10 and 9 is 1 away from 10, but 4 away from underflow 5). To translate this to code, I did a conditional for the ratio of 1/4 to apply the overflow condition above, and anything larger (2/3, 3/2) to apply the underflow conditional. The other edge case is 4. 4 is 1 away from overflow 5, but 3 away from underflow 1. So the magic number in the conditional becomes 1/3 or less.

Observing the recursive function, each iteration/conversion of digit would call itself at worst 4 times (no roman numeral is represented as more than 4 characters-- VIII); this is o(1) time. However, since the recursive function is invoked PER digit of the Test Integer (Step 2), it means this function runs in o(n) time.
