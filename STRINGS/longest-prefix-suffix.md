# Longest Prefix which is also Suffix

Given a string **s**, the task is to find the length of the **longest proper prefix** which is also a **suffix**. A proper prefix is a prefix that doesn't include the **whole** string. For example, prefixes of "abc" are "", "a", "ab" and "abc" but proper prefixes are "", "a" and "ab" only.

## Examples

### Example 1:
```
Input: s = "aabcdaabc"
Output: 4
Explanation: The string "aabc" is the longest proper prefix which is also a suffix.
```

### Example 2:
```
Input: s = "ababab"
Output: 4
Explanation: The string "abab" is the longest proper prefix which is also a suffix.
```

### Example 3:
```
Input: s = "aaaa"
Output: 3
Explanation: The string "aaa" is the longest proper prefix which is also a suffix.
```

## Approaches

### 1. Naive Approach: Comparing Prefixes and Suffixes
The simplest approach is to **compare** each proper prefix with the suffix. The minimum length of proper prefix is **zero** and maximum length is **length - 1**. We **iterate** over each length and check if the proper prefix of that size matches with the suffix of the same size.

#### Python Implementation:
```python
def longest_prefix_suffix(s):
    res = 0
    n = len(s)
    
    # Iterating over all possible lengths
    for length in range(1, n):
        # Starting index of proper prefix
        i = 0
        # Starting index of suffix
        j = n - length
        
        # Comparing proper prefix with suffix of length 'length'
        matches = True
        for k in range(length):
            if s[i + k] != s[j + k]:
                matches = False
                break
        
        # If current prefix matches suffix, update result
        if matches:
            res = length
    
    return res

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = ["aabcdaabc", "ababab", "aaaa"]
    
    for s in test_cases:
        result = longest_prefix_suffix(s)
        print(f"Input: s = {s}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static int longestPrefixSuffix(String s) {
        int res = 0;
        int n = s.length();
        
        // Iterating over all possible lengths
        for (int len = 1; len < n; len++) {
            // Starting index of proper prefix
            int i = 0;
            // Starting index of suffix
            int j = n - len;
            
            boolean matches = true;
            
            // Comparing proper prefix with suffix of length 'len'
            for (int k = 0; k < len; k++) {
                if (s.charAt(i + k) != s.charAt(j + k)) {
                    matches = false;
                    break;
                }
            }
            
            // If current prefix matches suffix, update result
            if (matches) {
                res = len;
            }
        }
        
        return res;
    }

    public static void main(String[] args) {
        // Test cases
        String[] testCases = {"aabcdaabc", "ababab", "aaaa"};
        
        for (String s : testCases) {
            int result = longestPrefixSuffix(s);
            System.out.println("Input: s = " + s);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n²), where n is the length of the string
**Space Complexity**: O(1), as we only use a few variables

### 2. Expected Approach: Using LPS Array (KMP Algorithm)
The idea is to use the **preprocessing** step of the **KMP Algorithm**. We build an **LPS** (Longest Proper Prefix which is also Suffix) array, where each index **i** stores the length of the longest proper prefix of the substring **s[0...i]** that is also a suffix of the same substring.

#### Python Implementation:
```python
def longest_prefix_suffix(s):
    n = len(s)
    lps = [0] * n
    
    # len stores the length of longest prefix which
    # is also a suffix for the previous index
    length = 0
    
    # lps[0] is always 0
    i = 1
    
    while i < n:
        # If characters match, increment the length
        if s[i] == s[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            # If there is a mismatch
            if length != 0:
                # Use the previous longest prefix-suffix
                length = lps[length - 1]
            else:
                # No prefix-suffix found
                lps[i] = 0
                i += 1
    
    return lps[n - 1]

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = ["aabcdaabc", "ababab", "aaaa"]
    
    for s in test_cases:
        result = longest_prefix_suffix(s)
        print(f"Input: s = {s}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static int longestPrefixSuffix(String s) {
        int n = s.length();
        int[] lps = new int[n];
        
        // len stores the length of longest prefix which
        // is also a suffix for the previous index
        int len = 0;
        
        // lps[0] is always 0
        int i = 1;
        
        while (i < n) {
            // If characters match, increment the length
            if (s.charAt(i) == s.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            }
            else {
                // If there is a mismatch
                if (len != 0) {
                    // Use the previous longest prefix-suffix
                    len = lps[len - 1];
                }
                else {
                    // No prefix-suffix found
                    lps[i] = 0;
                    i++;
                }
            }
        }
        
        return lps[n - 1];
    }

    public static void main(String[] args) {
        // Test cases
        String[] testCases = {"aabcdaabc", "ababab", "aaaa"};
        
        for (String s : testCases) {
            int result = longestPrefixSuffix(s);
            System.out.println("Input: s = " + s);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n), where n is the length of the string
**Space Complexity**: O(n), for storing the LPS array

## Key Points

1. The naive approach:
   - Simple to understand and implement
   - Compares each possible prefix with corresponding suffix
   - Less efficient for larger strings (O(n²) time complexity)

2. The KMP approach:
   - Uses preprocessing step of KMP algorithm
   - More efficient (O(n) time complexity)
   - Requires additional space for LPS array
   - Useful in other string matching problems

3. Important observations:
   - A proper prefix cannot be the entire string
   - The LPS array stores intermediate results
   - The last value in LPS array gives the final answer
   - The KMP approach avoids redundant comparisons