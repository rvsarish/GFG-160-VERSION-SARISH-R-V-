# Sentence Palindrome

Given a sentence s, the task is to check if it is a palindrome sentence or not. A palindrome sentence is a sequence of characters, such as a word, phrase, or series of symbols, that reads the same backward as forward after converting all uppercase letters to lowercase and removing all non-alphanumeric characters.

## Examples

### Example 1:
```
Input: s = "Too hot to hoot."
Output: True
Explanation: If we remove all non-alphanumeric characters and convert all uppercase 
letters to lowercase, string s will become "toohottohoot" which is a palindrome.
```

### Example 2:
```
Input: s = "Abc 012..## 10cbA"
Output: True
Explanation: If we remove all non-alphanumeric characters and convert all uppercase 
letters to lowercase, string s will become "abc01210cba" which is a palindrome.
```

### Example 3:
```
Input: s = "ABC $. def01ASDF.."
Output: False
Explanation: If we remove all non-alphanumeric characters and convert all uppercase 
letters to lowercase, string s will become "abcdef01asdf" which is not a palindrome.
```

## Approaches

### 1. Naive Approach: Comparing Original and Reversed Strings
A simple approach is to create a new string containing only the alphanumeric characters of the original string and convert uppercase letters to lowercase. Then, compare this new string with its reverse to check if the sentence is palindromic.

#### Python Implementation:
```python
def sentence_palindrome(s):
    # Create a new string having only alphanumeric characters
    s1 = ''.join(ch.lower() for ch in s if ch.isalnum())
    
    # Compare string with its reverse
    return s1 == s1[::-1]

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = [
        "Too hot to hoot.",
        "Abc 012..## 10cbA",
        "ABC $. def01ASDF.."
    ]
    
    for s in test_cases:
        result = sentence_palindrome(s)
        print(f"Input: {s}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static boolean sentencePalindrome(String s) {
        // Create a new string having only alphanumeric characters
        StringBuilder s1 = new StringBuilder();
        for (char ch : s.toCharArray()) {
            if (Character.isLetterOrDigit(ch)) {
                s1.append(Character.toLowerCase(ch));
            }
        }
        
        // Find reverse of this new string
        StringBuilder rev = new StringBuilder(s1.toString());
        rev.reverse();
        
        // Compare string and its reverse
        return s1.toString().equals(rev.toString());
    }

    public static void main(String[] args) {
        // Test cases
        String[] testCases = {
            "Too hot to hoot.",
            "Abc 012..## 10cbA",
            "ABC $. def01ASDF.."
        };
        
        for (String s : testCases) {
            boolean result = sentencePalindrome(s);
            System.out.println("Input: " + s);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n), where n is the length of the string
**Space Complexity**: O(n), as we create a new string of length proportional to input string

### 2. Expected Approach: Using Two Pointers
The idea is to use two pointers approach to find if a sentence is palindrome or not. Maintain one pointer at the beginning and the other at the end of the sentence. The pointers move towards each other, comparing characters along the way.

- If the characters match, continue moving both pointers inward
- If they differ, return false
- Skip all non-alphanumeric characters
- If the pointers cross each other, the sentence is a palindrome

#### Python Implementation:
```python
def sentence_palindrome(s):
    i, j = 0, len(s) - 1
    
    # Compare characters until they are equal
    while i < j:
        # Skip non-alphanumeric character from left side
        if not s[i].isalnum():
            i += 1
            continue
            
        # Skip non-alphanumeric character from right side
        if not s[j].isalnum():
            j -= 1
            continue
            
        # If characters are equal, update the pointers
        if s[i].lower() == s[j].lower():
            i += 1
            j -= 1
        else:
            return False
            
    return True

# Test the function
if __name__ == "__main__":
    # Test cases
    test_cases = [
        "Too hot to hoot.",
        "Abc 012..## 10cbA",
        "ABC $. def01ASDF.."
    ]
    
    for s in test_cases:
        result = sentence_palindrome(s)
        print(f"Input: {s}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static boolean sentencePalindrome(String s) {
        int i = 0, j = s.length() - 1;
        
        // Compare characters until they are equal
        while (i < j) {
            // Skip non-alphanumeric character from left side
            if (!Character.isLetterOrDigit(s.charAt(i))) {
                i++;
                continue;
            }
            
            // Skip non-alphanumeric character from right side
            if (!Character.isLetterOrDigit(s.charAt(j))) {
                j--;
                continue;
            }
            
            // If characters are equal, update the pointers
            if (Character.toLowerCase(s.charAt(i)) == 
                Character.toLowerCase(s.charAt(j))) {
                i++;
                j--;
            } else {
                return false;
            }
        }
        
        return true;
    }

    public static void main(String[] args) {
        // Test cases
        String[] testCases = {
            "Too hot to hoot.",
            "Abc 012..## 10cbA",
            "ABC $. def01ASDF.."
        };
        
        for (String s : testCases) {
            boolean result = sentencePalindrome(s);
            System.out.println("Input: " + s);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(n), where n is the length of the string
**Space Complexity**: O(1), as we only use two pointers regardless of input size

## Key Points

1. Both approaches handle:
   - Case-insensitive comparison
   - Non-alphanumeric character removal
   - Palindrome verification

2. The two-pointer approach is more efficient in terms of space complexity as it:
   - Doesn't create any additional strings
   - Uses constant extra space
   - Processes the string in-place

3. Both approaches have the same time complexity, but the two-pointer approach is generally preferred due to its space efficiency.