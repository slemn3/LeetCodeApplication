# LeetCodeApplication

I decided to Integer to Roman for this problem. As I went through the proposed solution mentally, there were obvious sub-tasks that needed to be solved.

1. **Number to Roman Lookup**-- There would need to be some sort of conditional, or lookup to translate the designated numerical values to the corresponding Roman Numeral. I opted to implement this as a couple of arrays. **1.** One array serves as the numerical look up of designated Roman Integers. **2.** The other is the array for the String letter corresponding to the designated integers. 

An alternate option considered was implementing this as a Dictionary or Map. My approach sacrificed a potential o(1) run time for this particular look up, for a smaller memory footprint. By implementing two arrays, my hope was to avoid creating bulky data structures that required non-primitive data types. Ultimately due to the full scans, look ups will be o(n).

2. **Considering each digit**-- The next big task is parsing each digit of the input to translate-- "TestInteger". I chose to scan the Integer from right to left so I could simplify the number as I appended translated Roman Characters.

Reverse-Looping through the digits of the number required two operations per iteration: **1.** increasing the substring "window" for Roman Character conversion (if 1994-- 4, 94, 994, etc.) and **2.** subtracting the "window" value from the overall provided TestInteger.
```
int TestInteger = 1994
for(start at the right most index and work left){
  determine the substring "window", this will be input for the Roman conversion 
  // (in this case 4, then 94 next iteration, then 994 etc.)
  
  call TranslateFunction(), providing substring window
  
  subtract substring "window" from the TestInteger 
  // (TestInteger subsequently will be 1994, 1990, 1900, then 1000)
  
  continue until you've made it all the way left  
}
```
This loops through the digits only once, requiring an o(n) runtime. Other costly allocations and operations are the various calls to convert int to string, and vice versa.

3. **Finding the Overflow and Underflow Numbers**-- This is the helper methods to find the next designated Roman Lookup Integer larger than the "TestInteger", as well as the next smallest Roman Integer (e.g. if we are given 3, the overflow is 5 and underflow is 1; 77 is 100 and 50). I needed these helper methods to help me determine the correct Roman Character String to append, as well the correct sequence. Each of these helper methods scan the pre-allocated array and are run in o(n) time, and require no additional memory allocation overhead.

4. **Bringing it all together**-- Now that the previous 3 tasks have been address, I can now bring it all together and implement the overall solution. To do so, I need to create a TranslateFunction to provide the Roman Characters. Since the translation is a repeatable set of steps, I opted to implement this as a recursive operation-- TranslateFunction. 
The input of the function is the Substring "Window" from Step 2, overflow value, underflow value. Due to the decision to implement the lookup of Roman Characters and Integers as an Array, I pass back and forth indexes (rather than values) for o(1) lookup.
```
function TranslateFunction(int valueToConvert, int indexOfOverflow, int indexOfUnderflow){
```
logic is as follows:
  -Base Case: Return 1 or the Roman Numeral String if the input is a designated Integer. This requires o(n) time since I will need to do a full list scan of the Array for the Roman String
  -Determine which Numeral and Sequence to append
 ```
 if(distanceOverflow/distanceUnderflow < 1/3)
  append overflow Roman Numeral + recursive call to function (overflow - valueToConvert, new overflow, new underflow)
 else
  append recursive call to function (valueToConvert - underflow, new overflow, new underflow) + underflow Roman Numeral
 ```
The significance of distanceOverflow/distanceUnderflow and "1/3" above is in reference to the correlation of 8 to its Roman Translation of VIII, instead of IIX. Thus, 8 will continue to append "I" to the underflow "V". The conditional is checking for 9, or anything that is 1 value away from the overflow, and as a result, 4 away from the underflow. So then we determine how to append the Roman Character by calculating distance as **distanceOverflow/distanceUnderflow**. The example just stated merely looks for 1/4. In the additional case of the Integer 4, the distance ration changes to 1 and 3 (4 is 1 value away from overflow 5, but 3 value away from underflow 1). Therefore, in our conditional, we satisfy all conditions by checking for 1/3.

Observing the recursive function, each iteration/conversion of digit would self-invoke at worst 4 times (since the longest Roman representation of a **digit** is no more than length 4-- VIII); this is o(4) time. And since the recursive function is invoked PER **digit** of the TestInteger (Step 2), it means this function runs in o(4n) or o(n) time.
