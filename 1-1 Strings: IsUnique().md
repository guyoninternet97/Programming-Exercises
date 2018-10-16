## Is Unique: Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?
#### Author: James Proulx


First, lets consider the trivial case, where we are allowed to use additional data structures.
The data structure **set** is very useful here. A set is a structure which stores values, but does not accept or store repeated values. Order does not matter.

How does this help? Using a set, we store each character of the string in the set while operating over the string, checking for duplicates.
If we arrive at a character already in the set, we can return false, as each character is not unique. If we reach the end of the string without returning, we know all the characters are unique.

First solution in Java:

```java
private static boolean isUnique(String s) {
  Set<Character> seenCharacters = new HashSet<Character>();
  for (int i = 0; i < s.length(); i++) {
    if (seenCharacters.contains(s.charAt(i)) {
      return false;
    } else {
      seenCharacters.add(s.charAt(i));
    }
  }
  
  //If we haven't returned false yet, all the characters are unique.
  return true;
}
```

This solution runs in **O(n)**, which is very good for this problem.

That was pretty trivial, lets try it with no extra data structures.


This presents a challenge because now we have no means of storing the characters that we have already seen.
My first thought would be to simply brute force check each character against every other character, seen here.

```java
private static boolean isUnique(String s) {
  for (int i = 0; i < s.length(); i++) {
    for (int j = 0; j < s.length(); j++) {
      if (s.charAt(i) == s.charAt(j) && (i != j)) {
        //In this case, two characters at seperate indices are equal. Return false.
        return false;
      }
    }
  }
  
  //These nested for loops will test every character against every other character. If we arrive here and haven't returned, return true.
  return true;
}
```


Hmm. This is a correct solution, but it runs in an unsatisfactory **O(n<sup>2</sup>)** runtime. Can we do better?


My first thought is that with this solution, we check every character against every other character. But when we compare the first character
to all the other characters, we know that the first character is unique, and we no longer need to check any characters against this character.

This is still **O(n<sup>2</sup>)**, but the number of checks is still reduced by a good amount.
How can we implement this?

```java
private static boolean isUnique(String s) {
  StringBuilder workingString = new StringBuilder(s); //To improve performance/ease of string operations
  
  while (workingString.length() > 0) {
    for (int i = 1; i < workingString.length(); i++) {
      if (workingString.charAt(0) == workingString.charAt(i)) {
        //Found a duplicate, return false.
        return false;
      }
    }
    
    //At this point, we have compared the first character to every other character, and it is unique.
    //We are free to stop considering this character, so we will remove it from the string.
    
    workingString.deleteCharAt(0);
    //Now check the string again, without this character.
  }
  
  //If we reach this point, all characters have been determined unique and removed. Return true!
  return true;
}

```

This is a correct solution, and is certainly an improvement over the brute force!


Here, I checked the official solution to this problem to see if it could be improved upon further.
The official solution got the runetime down to **O(nlog(n))**, which is quite impressive, and was done by sorting the characters of the string, and checking each letter against its neighbors.


#### Conclusion:

This initial problem is an excellent example of how a simple problem can be expanded upon by adding restrictions, and how a simple problem
can become difficult to think about once you start to consider things like run time. Although I didn't get the lowest possible run time, i'm happy with my solution I came up with.

I'm happy with the insight I gained about the usefulness of sorting characters in a string, that was something I didn't consider.


Thanks for reading!
