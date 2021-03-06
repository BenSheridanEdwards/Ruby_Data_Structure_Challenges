# Ruby Data Structure Challenges

Table of Contents 

### Arrays

[Meeting Checker](#Meeting-Checker) | [Smallest Subset Length](#Smallest-Subset-Length) | [Girl Scout Cookie Orders](#Girl-Scout-Cookie-Orders) | [Cafe Order Checker](#Cafe-Order-Checker) | [RunPath](#Run-Path)

### Strings

[Reversing In-Place](#Reversing-In-Place) | [Reverse Words](#Reverse-Words)

### Hashes

[Inflight Entertainment](#Inflight-Entertainment) | [Permutation Palindrome](#Permutation-Palindrome)

<a name="Meeting-Checker"><br></a>

## Meeting Checker

### Challenge Description

Your company has built an in-house calendar tool, and they want you to add a feature to see the times in a day when everyone is available. 

Every team's meetings are stored as an array of integers, [start_time, end_time]. The integers respresent the number of 30-minute blocks past 9:00am.

Create a Ruby class called MeetingChecker, that has a method called availability which accepts the meetings array as an argument, and returns an array of condensed ranges. 

For example, given: 

```
[[0, 1], [3, 5], [4, 8], [10, 12], [9, 10]]
```

Your method should return:

```
[[0, 1], [3, 8], [9, 12]]
```

### Additional Notes (Including Edge Cases) 

- Do not assume the meetings are in order. As the meetings times are coming from multiple teams.

- Your solution should be efficient even when we can't put an upper bound on the numbers in our time ranges. Like if we wanted to use Unix timestamps. 

- Meetings that 'touch' like this `[[1, 2], [2, 3]]` should be merged.

- When a meeting starts before and ends after another meeting, it should correctly subsume that meeting. 

### Tests

![MeetingCheckerSpec](https://github.com/BenSheridanEdwards/Meeting_Check_Challenge_Ruby/blob/master/media/MeetingCheckerRSpecTesting.png)

### Solution

![MeetingChecker](https://github.com/BenSheridanEdwards/Meeting_Check_Challenge_Ruby/blob/master/media/MeetingCheckerRubyCode.png)

### Complexity

- O(n lg n) Time

- O(n) Space

Sorting the meetings first gives a runtime of *O(n lg n)*. 

Note, best-case scenario is that the array is already in order, so we'd skip the sort and do it in *O(n)* time.

Creating a new array for merged_meetings, where if none of the meetings overlaped, we'd have a worst-case space cost of *O(n)*.

### Walkthrough Explaination

First, we sort our input array so any meetings that might need to be merged are next to each other. 

Then we walk through our soted meetings from left to right, at each step we either: 

A. We can merge the current meeting with the previous one as they overlap of are touching. So we do, ensuring we have the earliest start time and latest end time of the two meetings for the combined meeting. 

or

B. We can't merge the current meeting with the previous one, so we add that meeting to merged_meetings. 

<br>

## <a name="Smallest-Subset-Length">Smallest Subset Length</a>

### Description

For a set of numbers, the degree of the set is the largest number of occurrences of the same element. In the given example below, the degree is 3 as both element 1 and element 2 occur 3 times each.

Given a set of numbers, return the length of the smallest subset that has the same degree as the original set.

For example, for a given set: 
```
Set = [1, 2, 2, 2, 3, 4, 1, 5, 1, 6]
```

Your method should return:
```
3
```

### Explaination

The two subsets with the same degree as the original set are {1, 2, 2, 2, 3, 4, 1, 5, 1} and {2, 2, 2}. The first subset is 9 elements long and the second one is 3 elements long, so the program should return 3.

### Solution

![Smallest Subset Length Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/SmallestSubsetLengthCode.png)

### Unit Tests

![Smallest Subset Length Testing](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/SmallestSubsetLengthRSpecTesting.png)

## <a name="Girl-Scout-Cookie-Orders">Girl Scout Cookie Orders</a>

### Description

In order to win the prize for most cookies sold, my friend Alice and I are going to merge our Girl Scout Cookies orders and enter as one unit.

Each order is represented by an "order id" (an integer).

We have our lists of orders sorted numerically already, in arrays. Write a method to merge our arrays of orders into one sorted array.

For example: 

```
my_array     = [3, 4, 6, 10, 11, 15]
alices_array = [1, 5, 8, 12, 14, 19]

merge_arrays(my_array, alices_array)
```

Your method should output: 

```
[1, 3, 4, 5, 6, 8, 10, 11, 12, 14, 15, 19]

```

### Addtional Notes (Including edge cases)

We could concatinate the arrays together, then sort the result: 

![Girl Scout Cookie Order Simple Solution](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/CookieOrdersRubySimpleCode.png)

But the time cost would be O(n lg n), where n is the total length of our output array, the sum of the length of our inputs. 

We can do better than this though, as we're not taking advantage of the face the input arrays are *already sorted*. 

We know since our arrays are sorted, the smallest item for each input array will be at the 0th index. So the smallest item overall will be a the 0th index of one of our input arrays!

If we push the smallest number at the 0th index from both arrays into our merged arrays, we can always use the 0th index to find our smallest number between both arrays. 

But we have a few problems when we consider our Edge cases: 

1. What if one or both our input arrays are 1 element or less? 
2. What if one of our input arrays is longer than the other?
3. What if one of our arrays runs out of elements before we're done merging the other?

For all these edge cases, there will be a point when the 0th value on one of our input arrays will be nil. So we'll get a NoMethodError for calling 'less than' on nil - Or an Argument Error for calling 'less than' on an integer but passing nil as an argument. 

So when we write our conditional statements, we'll have to have 4 cases, 2 to check which item is next from the 0th index of each array, and 2 to check if either array is exhausted.

### Unit Tests

![Girl Scout Cookies Tests](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/CookieOrdersRSpecTesting.png)

### Solution

![Girl Scout Cookies Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/CookieOrdersRubyCodeDRY.png)

### Complexity

- O(n) Time
- O(n) Space

Where n is the number of items in the merged array

### Explaination

First, we allotcast our answer array, getting its size by adding the size of `my_array` and `alices_array`.

Keeping track of the current index of `my_array`, `alices_array`, and the `merged_array`. At each step, there's a current element in `my_array` and `alices_array`, the smaller of which is added to the merged_array. 

In our code we're also accounting for the edge case that we may exhaust one of our arrays whilst there are still elements in the order - So we only add the next element to merged_array from either my_array or alices_array if the respective array is not exhausted, and either: 

A. the other array is exhausted.

or 

B. the current element in the respective array is less than the current element in the other array. 

The if statement is constructed to take advantage of Ruby's short circuit evaluation to check first the arrays are exhausted - So to avoid indexing past the end of an array, and Ruby doesn't throw a NoMethodError or ArgumentError when we try to compare nil with an integer or vice versa.

<a name="Cafe-Order-Checker"><br></a>

## Cafe Order Checker

### Description

My cake shop is so popular, I'm adding some tables and hiring wait staff so folks can have a cute sit-down cake-eating experience.

I have two registers: one for take-out orders, and the other for the other folks eating inside the cafe. All the customer orders get combined into one list for the kitchen, where they should be handled first-come, first-served.

Recently, some customers have been complaining that people who placed orders after them are getting their food first. Yikes—that's not good for business!

To investigate their claims, one afternoon I sat behind the registers with my laptop and recorded:

- The take-out orders as they were entered into the system and given to the kitchen. (take_out_orders)
- The dine-in orders as they were entered into the system and given to the kitchen. (dine_in_orders)
- Each customer order (from either register) as it was finished by the kitchen. (served_orders)

Given all three arrays, write a method to check that my service is first-come, first-served. All food should come out in the same order customers requested it.

For example, with this input:

```
Take Out Orders: [1, 3, 5]
Dine In Orders: [2, 4, 6]
Served Orders: [1, 2, 4, 6, 5, 3]
```
*This would not be first-come first-served, since order 3 was requested before order 5 but order 5 was served first.*

```
 Take Out Orders: [1, 3, 5]
 Dine In Orders: [2, 4, 6]
 Served Orders: [1, 2, 3, 5, 4, 6]
```
*This would be first-come, first-served.*

### Tests

![Cafe Order Checker Tests](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/CafeOrderCheckerRSpecTesting.png)

### Solution

![Cafe Order Checker Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/CafeOrderCheckerRubyCode.png)

### Complexity

- O(n) Time
- O(1) Space

### Explaination

We walk through served_orders, seeing if each customer order so far matches a customer order from one of the two registers. To check this, we:

1. Keep pointers to the current index in take_out_orders, dine_in_orders, and served_orders.
2. Walk through served_orders from beginning to end.
3. If the current order in served_orders is the same as the current customer order in take_out_orders, increment take_out_orders_index and keep going. This can be thought of as "checking off" the current customer order in take_out_orders and served_orders, reducing the problem to the remaining customer orders in the arrays.
4. Same as above with dine_in_orders.
5. If the current order isn't the same as the customer order at the front of take_out_orders or dine_in_orders, we know something's gone wrong and we're not serving food first-come, first-served.
6. If we make it all the way to the end of served_orders, we'll check that we've reached the end of take_out_orders and dine_in_orders. If every customer order checks out, that means we're serving food first-come, first-served.

## <a name="Run-Path">Run Path</a>

### Description 

Given the length of a path and a set of back and forth runs on this path, return the position on the path that is visited most often at the end of all runs. If there are multiple positions that meet this criterion, return the lowest position. 

For example, when given:
```
path = [1, 2, 3, 4, 5, 6, 7, 8, 9]
runs = [2, 7, 3, 9, 1, 5]

method(path, runs)
```

Your method should return:
```
3
```
### Explaination

- The set of runs translates to: start at 2, run to 7; start at 7, run to 3; start at 3, run to 9; start at 9, run to 1; start at 1, run to 5. 
- Note that each position is visited once when the run ends there and once when the run starts there, so for example when you run from 2 to 7 and then from 7 to 3, you have visited 7 twice. 
- Given the above example, positions 3, 4 and 5 would all have been visited 5 times, which is the largest number of visits, but the program should return the lowest such positions, which is 3.

### Solution

![RunPath Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/RunPathRubyCode.png)

![RunPath Editor](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/RunPathRubyEditor.png)

### Unit Tests

![RunPath Testing](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/RunPathRSpecTesting.png)

<a name="Reversing-In-Place"><br></a>

## Reversing In-Place

### Challenge Description 
Write a method that takes an array or string and reverses the elements *in place*.

For example, given a string: 

```
"ABCDE"
```

Your method should return:

```
"EDCBA"
```

Also, when given an array: 
```
["A", "B", "C", "D", "E"]
```

Your method should return:
```
["E", "D", "C", "B", "A"]
```

### Additional Notes (Including Edge Cases)

- An in-place method modifies data structures or objects outside of its own stack frame. Because of this, the changes made by the method remain after the call completes.

- In-place algorithms are sometimes called destructive, since the original input is "destroyed" (or modified) during the method call.
  
- Careful: "In-place" does not mean "without creating any additional variables!" Rather, it means "without creating a new copy of the input." In general, an in-place method will only create additional variables that are O(1) space.
  
- An out-of-place method doesn't make any changes that are visible to other methods. Usually, those methods copy any data structures or objects before manipulating and changing them.

- The elements is the string/array input will be letters, numbers or a mixture of both.

### Tests

![Reversing Testing](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/ReversingRSpecTesting.png)

### Solution

![Reversing Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/ReversingRubyCode.png)

### Complexity

- O(n) Time
- O(1) Space

### Explaination

Swap the first and last characters, then move inwards until reaching the middle. 

<a name="Reverse-Words"><br></a>

## Reverse Words

### Challenge Description

You're working on a secret team solving coded transmissions.

Your team is scrambling to decipher a recent message, worried it's a plot to break into a major European National Cake Vault. The message has been mostly deciphered, but all the words are backward! Your colleagues have handed off the last step to you.

Write a method reverse_words!() that takes a message as a string and reverses the order of the words in place.

For example, with the input:
```
"cake pound steal"
```

Your method should print:
```
"steal pound cake"
```

#### Additional Notes

- When writing your method, assume the message contains only letters and spaces, and all words are separated by one space.

### Tests

![Reverse Words Tests](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/ReverseWordsRSpecTesting.png)

### Solution

![Reverse Words Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/ReverseWordsRubyCode.png)

### Complexity

- O(n) Time
- O(1) Space

Time is *O(n)* as we have to walk through the entire length of the input string, and the words could all be different lengths. 

Space is O(1) as it's in place. 

### Walkthrough Explanation

We use a helper method reverse_characters!() that reverses all the characters between a given left and right index. 

We first use it to reverse all the characters in the entire message - As this gives us the correct word order, but with each word backward.

Then we use it again to reverse the characters in each individual word.

<a name="Inflight-Entertainment"><br></a>

## Inflight Entertainment

### Description

An airline has built an inflight entertainment system with on-demand movie streaming. But users on long flights are complaining that their plane usually lands before they can see the ending to their movie.

You're tasked with building a feature for choosing two movies whose runtimes will equal the exact flight length. 

Write a method that takes an integer, *flight_length* (in minutes) and an array of integers *movie_lengths* (in minutes) and returns a boolean indicating whether there are two numbers in *movie_lengths* whose sum equals *flight_length*. 

Keep in mind: 

- Assume your users will watch exactly two movies
- Don't make your users watch the same movie twice
- Optimize for runtime over memory

### Additional Notes

We know that when choosing two times from the *movie_length* list, we're looking for a *second_movie_length* that is equal to the *flight_length* - *first_movie_length*. We could nest two loops, the outer choosing the *first_movie_length* and the inner choosing the *second_movie_length*. That would give us a runtiem of O(n²) but we can do better.

How could we check for the existence of our second_movie_legnth in constant time? 

We need a data structure that gives us convenient constant-time lookups, like a set! 

Throwing all our movie_lengths into a set first in linear O(n) time. We could then use a single loop to go through the possible first_movie_lengths, and check the set for the result of flight_length - first_movie_length. Giving us O(n) runtime overall.

By also checking the movies we've already seen for matches before putting a movie in, we're ensuring users won't watch the same movie twice. 

### Testing

![Inflight Entertainment Testing](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/InflightEntertainmentRSpecTesting.png)

### Solution

![Inflight Entertainment Code](https://github.com/BenSheridanEdwards/Ruby_Data_Structure_Challenges/blob/master/media/InflightEntertainmentRubyCode.png)

### Complexity

- O(n) time
- O(n) space

Optimised for runtime by adding a small amount of space cost.

### Explanation

We make one pass through movie_lengths, treating each item as the first_movie_length. At each iteration, we:

1. See if there's a matching_second_movie_length we've seen already (stored in our movie_lengths_seen set) that is equal to flight_length - first_movie_length. If there is, we short-circuit and return true.

2. Keep our movie_lengths_seen set up to date by throwing in the current first_movie_length.

## Permutation Palindrome

### Description

Write an efficient method that checks whether any permutation of an input string is a palindrome.

- You can assume the input string only contains lowercase letters

### Additional Notes

### Tests

![Permutation Palindrome Testing]()

### Solution

![Permutation Palindrome Ruby Code]()

### Complexity

O(n) time, since we're making one iteration through the nn characters in the string.

### Explanation

