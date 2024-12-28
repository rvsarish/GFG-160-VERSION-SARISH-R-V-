# CamelCase Pattern Matching

Given a list of words where each word follows **CamelCase** notation, the task is to print all **words** in the list that match the given **pattern**, where the pattern consists of uppercase characters only. A **word** matches the **pattern** if all characters of the pattern appear **sequentially** in the word's uppercase letters.

## Examples

### Example 1:
```
Input: 
arr[] = ["WelcomeGeek", "WelcomeToGeeksForGeeks", "GeeksForGeeks", "WayToGo"]
pat = "WTG"

Output: ["WelcomeToGeeksForGeeks", "WayToGo"]

Explanation: All uppercase characters of given pattern appears sequentially in the words 
"WelcomeToGeeksForGeeks" and "WayToGo".
```

### Example 2:
```
Input:
arr[] = ["Hi", "Hello", "HelloWorld", "HiTech", "HiGeek", "HiTechWorld", "HiTechCity", "HiTechLab"]
pat = "HA"

Output: []

Explanation: None of the words matches the given pattern.
```

## Approach

The idea is to use the fact that for each word in the input list, we are only concerned with the **uppercase** letters of the word. We can initialize two pointers, say **i** and **j** at the **beginning** of the **current word** and **pattern** respectively.

We can have 3 cases:
1. **word[i] is a lowercase character**: move to the next character by incrementing **i** by 1.
2. **word[i] is an uppercase character and word[i] = pat[j]**: the characters match in both word and pattern, so increment both i and j.
3. **word[i] is an uppercase character and word[i] != pat[j]**: the characters don't match, so skip the current word.

If all the characters of the pattern match, that is **j** reaches the **end** of the pattern string, then add the word to the result.

## Implementation

### Python Implementation
```python
def camel_case(arr, pat):
    # list for storing matched words
    res = []
    
    for word in arr:
        i = j = 0
        while i < len(word) and j < len(pat):
            # If ith character of word is a lowercase
            # character, move to next character
            if word[i].islower():
                i += 1
            
            # If ith character of word is an uppercase
            # character and does not match with the jth
            # character of pattern, move to the next word
            elif word[i] != pat[j]:
                break
            
            # If characters match, move both pointers
            else:
                i += 1
                j += 1
                
        # If pattern is completely matched
        if j == len(pat):
            res.append(word)
            
    return res

# Test the function
if __name__ == "__main__":
    # Test case 1
    arr1 = ["WelcomeGeek", "WelcomeToGeeksForGeeks", 
            "GeeksForGeeks", "WayToGo"]
    pat1 = "WTG"
    print("Test case 1 result:", camel_case(arr1, pat1))
    
    # Test case 2
    arr2 = ["Hi", "Hello", "HelloWorld", "HiTech", "HiGeek",
            "HiTechWorld", "HiTechCity", "HiTechLab"]
    pat2 = "HA"
    print("Test case 2 result:", camel_case(arr2, pat2))
```

### Java Implementation
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> camelCase(List<String> arr, String pat) {
        // List for storing matched words
        List<String> res = new ArrayList<>();
        
        for (String word : arr) {
            int i = 0, j = 0;
            while (i < word.length() && j < pat.length()) {
                // If ith character of word is a lowercase
                // character, move to next character
                if (Character.isLowerCase(word.charAt(i))) {
                    i++;
                }
                // If ith character of word is an uppercase
                // character and does not match with the jth
                // character of pattern, move to the next word
                else if (word.charAt(i) != pat.charAt(j)) {
                    break;
                }
                // If characters match, move both pointers
                else {
                    i++;
                    j++;
                }
            }
            
            // If pattern is completely matched
            if (j == pat.length()) {
                res.add(word);
            }
        }
        
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        
        // Test case 1
        List<String> arr1 = List.of("WelcomeGeek", 
            "WelcomeToGeeksForGeeks", "GeeksForGeeks", "WayToGo");
        String pat1 = "WTG";
        System.out.println("Test case 1 result: " + 
            solution.camelCase(arr1, pat1));
        
        // Test case 2
        List<String> arr2 = List.of("Hi", "Hello", "HelloWorld",
            "HiTech", "HiGeek", "HiTechWorld", "HiTechCity", "HiTechLab");
        String pat2 = "HA";
        System.out.println("Test case 2 result: " + 
            solution.camelCase(arr2, pat2));
    }
}
```

## Complexity Analysis

- **Time Complexity**: O(n * len), where **n** is size of array and **len** is max size of word in array
- **Auxiliary Space**: O(1), excluding the space used for output array

The algorithm uses a two-pointer approach to efficiently match the pattern with each word in the array. It only needs to traverse each character of each word once, making it an efficient solution for the given problem.