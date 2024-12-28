# Minimum Repetitions of s1 such that s2 is a Substring of it

Given two strings s1 and s2, the task is to find the minimum number of times s1 has to be repeated such that s2 is a substring of it. If no such solution exists, print -1.

## Examples

### Example 1:
```
Input: s1 = "abcd", s2 = "cdabcdab"
Output: 3 
Explanation: After repeating s1 three times, s1 will become "abcdabcdabcd". 
Now, s2 is a substring of s1. Also s2 is not a substring of s1 when it is 
repeated less than 3 times.
```

### Example 2:
```
Input: s1 = "ab", s2 = "cab" 
Output: -1
Explanation: s2 can not become a substring of s1 after any repetition of s1.
```

## Approaches

### 1. Naive Approach: Matching Characters
The idea is instead of appending string s1 multiple times, we will simply use modulo (i % n) when we reach the end of string s1. We iterate over all characters of s1 and for each character, consider it as the starting point of s2 and compare s1 and s2 character by character.

#### Python Implementation:
```python
def min_repeats(s1, s2):
    n, m = len(s1), len(s2)
    
    # Iterate over all characters of s1 and start comparing with s2
    for i in range(n):
        rep = 1
        idx1 = i
        found = True
        
        # Compare each character of s2 with s1
        for idx2 in range(m):
            if s1[idx1] != s2[idx2]:
                found = False
                break
            
            idx1 += 1
            # If we have reached the end of s1, reset the index to 0
            if idx1 == n:
                idx1 = 0
                rep += 1
        
        # If all characters of s2 were found in repeated s1
        if found:
            return rep
    
    return -1

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = [
        ("abcd", "cdabcdab"),
        ("ab", "cab")
    ]
    
    for s1, s2 in test_cases:
        result = min_repeats(s1, s2)
        print(f"Input: s1 = {s1}, s2 = {s2}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static int minRepeats(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        
        // Iterate over all characters of s1 and start comparing with s2
        for (int i = 0; i < n; i++) {
            int rep = 1, idx1 = i;
            boolean found = true;
            
            // Compare each character of s2 with s1
            for (int idx2 = 0; idx2 < m; idx2++) {
                if (s1.charAt(idx1) != s2.charAt(idx2)) {
                    found = false;
                    break;
                }
                
                idx1++;
                // If we have reached the end of s1, reset the index to 0
                if (idx1 == n) {
                    idx1 = 0;
                    rep++;
                }
            }
            
            // If all characters of s2 were found in repeated s1
            if (found) {
                return rep;
            }
        }
        
        return -1;
    }

    public static void main(String[] args) {
        // Test cases
        String[][] testCases = {
            {"abcd", "cdabcdab"},
            {"ab", "cab"}
        };
        
        for (String[] test : testCases) {
            int result = minRepeats(test[0], test[1]);
            System.out.println("Input: s1 = " + test[0] + 
                             ", s2 = " + test[1]);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n * m), where n and m are lengths of s1 and s2
**Space Complexity**: O(1), as we only use a few variables

### 2. Expected Approach: Using KMP Algorithm
For s2 to be a substring of repeated s1, s1 should be at least as long as s2. Let x be the minimum number of times s1 needs to be repeated such that its length becomes greater than or equal to s2. So, x = ceil(len(s2) / len(s1)).

The minimum number of repetitions will always be either x or x + 1 because:
1. The substring in s1 that equals s2 must start at index: 0, 1, 2, ..., len(s1) - 1
2. After len(s1) - 1, the pattern repeats
3. We need enough length to accommodate s2 from any starting position

#### Python Implementation:
```python
def compute_lps_array(pattern):
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps

def min_repeats(s1, s2):
    n, m = len(s1), len(s2)
    
    # Calculate minimum repetitions needed
    x = (m + n - 1) // n
    
    # Create the pattern string
    pattern = s2
    # Create the text by repeating s1
    text = s1 * (x + 1)
    
    # Perform KMP search
    lps = compute_lps_array(pattern)
    i = j = 0
    
    while i < len(text):
        if pattern[j] == text[i]:
            i += 1
            j += 1
        else:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
        
        if j == m:
            # Pattern found, check if x repetitions are enough
            if (i <= n * x):
                return x
            return x + 1
    
    return -1

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = [
        ("abcd", "cdabcdab"),
        ("ab", "cab")
    ]
    
    for s1, s2 in test_cases:
        result = min_repeats(s1, s2)
        print(f"Input: s1 = {s1}, s2 = {s2}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    // Function to compute the LPS Array
    static void computeLPSArray(String pattern, int[] lps) {
        int len = 0, i = 1;
        lps[0] = 0;
        
        while (i < pattern.length()) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
    }
    
    static int minRepeats(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        
        // Calculate minimum repetitions needed
        int x = (m + n - 1) / n;
        
        // Create the text by repeating s1
        StringBuilder text = new StringBuilder();
        for (int i = 0; i < x + 1; i++) {
            text.append(s1);
        }
        
        // Perform KMP search
        int[] lps = new int[m];
        computeLPSArray(s2, lps);
        
        int i = 0, j = 0;
        while (i < text.length()) {
            if (s2.charAt(j) == text.charAt(i)) {
                i++;
                j++;
            } else {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
            
            if (j == m) {
                // Pattern found, check if x repetitions are enough
                if (i <= n * x) {
                    return x;
                }
                return x + 1;
            }
        }
        
        return -1;
    }

    public static void main(String[] args) {
        // Test cases
        String[][] testCases = {
            {"abcd", "cdabcdab"},
            {"ab", "cab"}
        };
        
        for (String[] test : testCases) {
            int result = minRepeats(test[0], test[1]);
            System.out.println("Input: s1 = " + test[0] + 
                             ", s2 = " + test[1]);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n + m), where n and m are lengths of s1 and s2
**Space Complexity**: O(m), for storing the LPS array

## Key Points

1. The naive approach:
   - Uses modulo operation to simulate string repetition
   - Simple to implement but less efficient
   - Suitable for small strings

2. The KMP approach:
   - More efficient for larger strings
   - Uses pattern matching to avoid unnecessary comparisons
   - Requires additional space for LPS array
   - Guarantees optimal time complexity

3. Important observations:
   - The minimum repetitions needed is either x or x+1
   - We can avoid actually repeating the string multiple times
   - Pattern matching algorithms can significantly improve efficiency