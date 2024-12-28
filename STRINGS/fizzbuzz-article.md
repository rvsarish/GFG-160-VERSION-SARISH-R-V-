# FizzBuzz Problem: Complete Guide with Python and Java Implementations

Given an integer n, for every positive integer i <= n, the task is to print:

- "FizzBuzz" if i is divisible by 3 and 5
- "Fizz" if i is divisible by 3
- "Buzz" if i is divisible by 5
- "i" as a string, if none of the conditions are true

## Examples

```
Input: n = 3
Output: ["1", "2", "Fizz"]

Input: n = 10
Output: ["1", "2", "Fizz", "4", "Buzz", "Fizz", "7", "8", "Fizz", "Buzz"]

Input: n = 20
Output: ["1", "2", "Fizz", "4", "Buzz", "Fizz", "7", "8", "Fizz", "Buzz", "11", "Fizz", "13", "14", "FizzBuzz", "16", "17", "Fizz", "19", "Buzz"]
```

## Table of Contents

1. [Naive Approach: By checking every integer individually](#naive-approach)
2. [Better Approach: By String Concatenation](#better-approach)
3. [Expected Approach: Using Hash Map or Dictionary](#expected-approach)

## Naive Approach: By checking every integer individually {#naive-approach}

A very simple approach to solve this problem is that we can start checking each number from 1 to n to see if it's divisible by 3, 5, or both. Depending on the divisibility:

- If a number is divisible by both 3 and 5, append "FizzBuzz" into result
- If it's only divisible by 3, append "Fizz" into result
- If it's only divisible by 5, append "Buzz" into result
- Otherwise, append the number itself into result

### Python Implementation:
```python
def fizzbuzz(n):
    res = []
    for i in range(1, n + 1):
        # Check if i is divisible by both 3 and 5
        if i % 3 == 0 and i % 5 == 0:
            # Add "FizzBuzz" to the result list
            res.append("FizzBuzz")
        
        # Check if i is divisible by 3
        elif i % 3 == 0:
            # Add "Fizz" to the result list
            res.append("Fizz")
        
        # Check if i is divisible by 5
        elif i % 5 == 0:
            # Add "Buzz" to the result list
            res.append("Buzz")
        
        else:
            # Add the current number as a string to the result list
            res.append(str(i))
    
    return res

# Test the function
if __name__ == "__main__":
    n = 20
    result = fizzbuzz(n)
    print(" ".join(result))
```

### Java Implementation:
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        
        for (int i = 1; i <= n; i++) {
            // Check if i is divisible by both 3 and 5
            if (i % 3 == 0 && i % 5 == 0) {
                // Add "FizzBuzz" to the result list
                res.add("FizzBuzz");
            }
            // Check if i is divisible by 3
            else if (i % 3 == 0) {
                // Add "Fizz" to the result list
                res.add("Fizz");
            }
            // Check if i is divisible by 5
            else if (i % 5 == 0) {
                // Add "Buzz" to the result list
                res.add("Buzz");
            }
            else {
                // Add the current number as a string to the result list
                res.add(String.valueOf(i));
            }
        }
        
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 20;
        List<String> result = solution.fizzBuzz(n);
        System.out.println(String.join(" ", result));
    }
}
```

**Time Complexity**: O(n), since we need to traverse the numbers from 1 to n in any condition.
**Auxiliary Space**: O(n), for storing the result

## Better Approach: By String Concatenation {#better-approach}

While the naive approach works well for the basic FizzBuzz problem, it becomes very complicated if additional mappings come under picture, such as "Jazz" for multiples of 7. The number of conditions for checking would increase.

Instead of checking every possible combination of divisors, we check each divisor separately and concatenate the corresponding strings if the number is divisible by them.

Step-by-step approach:
1. For each number i <= n, start with an empty string s
2. Check if i is divisible by 3, append "Fizz" into s
3. Check if i is divisible by 5, append "Buzz" into s
4. If we add "Fizz" and "Buzz", the string s becomes "FizzBuzz" and we don't need extra comparisons
5. If nothing was added, just use the number
6. Finally, append this string s into our result array

### Python Implementation:
```python
def fizzbuzz(n):
    res = []
    for i in range(1, n + 1):
        # Initialize an empty string for the current result
        s = ""
        
        # Divides by 3, add Fizz
        if i % 3 == 0:
            s += "Fizz"
        
        # Divides by 5, add Buzz
        if i % 5 == 0:
            s += "Buzz"
        
        # If string is still empty, use the number
        if not s:
            s = str(i)
        
        res.append(s)
    
    return res

# Test the function
if __name__ == "__main__":
    n = 20
    result = fizzbuzz(n)
    print(" ".join(result))
```

### Java Implementation:
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        
        for (int i = 1; i <= n; i++) {
            // Initialize an empty string for the current result
            StringBuilder s = new StringBuilder();
            
            // Divides by 3, add Fizz
            if (i % 3 == 0)
                s.append("Fizz");
            
            // Divides by 5, add Buzz
            if (i % 5 == 0)
                s.append("Buzz");
            
            // If string is still empty, use the number
            if (s.length() == 0)
                s.append(i);
            
            res.add(s.toString());
        }
        
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 20;
        List<String> result = solution.fizzBuzz(n);
        System.out.println(String.join(" ", result));
    }
}
```

**Time Complexity**: O(n), since we need to traverse the numbers from 1 to n in any condition.
**Auxiliary Space**: O(n), for storing the result

## Expected Approach: Using Hash Map or Dictionary {#expected-approach}

When we have many words to add like "Fizz", "Buzz", "Jazz" and more, the second method can still get complicated and lengthy. To make things cleaner, we can use a hash map. Initially, we can store the divisors and their corresponding words into the hash map.

For this problem, we would map 3 to "Fizz" and 5 to "Buzz" and for each number, check if it is divisible by 3 or 5, if so, append the corresponding string from map to the result. If the number is not divisible by either, simply add the number itself as a string.

### Python Implementation:
```python
def fizzbuzz(n):
    # Hash map to store all fizzbuzz mappings
    fizz_buzz_map = {3: "Fizz", 5: "Buzz"}
    
    # List of divisors which we will iterate over
    divisors = [3, 5]
    
    res = []
    for i in range(1, n + 1):
        s = ""
        
        for d in divisors:
            # If i is divisible by d, add the corresponding string mapped with d
            if i % d == 0:
                s += fizz_buzz_map[d]
        
        # Not divisible by 3 or 5, add the number
        if not s:
            s = str(i)
        
        res.append(s)
    
    return res

# Test the function
if __name__ == "__main__":
    n = 20
    result = fizzbuzz(n)
    print(" ".join(result))
```

### Java Implementation:
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<>();
        
        // Hash map to store all fizzbuzz mappings
        Map<Integer, String> fizzBuzzMap = new HashMap<>();
        fizzBuzzMap.put(3, "Fizz");
        fizzBuzzMap.put(5, "Buzz");
        
        // List of divisors which we will iterate over
        List<Integer> divisors = List.of(3, 5);
        
        for (int i = 1; i <= n; i++) {
            StringBuilder s = new StringBuilder();
            
            for (int d : divisors) {
                // If i is divisible by d, add the corresponding string mapped with d
                if (i % d == 0)
                    s.append(fizzBuzzMap.get(d));
            }
            
            // Not divisible by 3 or 5, add the number
            if (s.length() == 0)
                s.append(i);
            
            res.add(s.toString());
        }
        
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int n = 20;
        List<String> result = solution.fizzBuzz(n);
        System.out.println(String.join(" ", result));
    }
}
```

**Time Complexity**: O(n), since we need to traverse the numbers from 1 to n in any condition.
**Auxiliary Space**: O(n), for storing the result
