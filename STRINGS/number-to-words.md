# Program to Convert a Given Number to Words

Given a non-negative integer n, the task is to convert the given number into its English representation according to International Number System.

## Examples

```
Input: n = 0
Output: "Zero"

Input: n = 123
Output: "One Hundred Twenty Three"

Input: n = 10245
Output: "Ten Thousand Two Hundred Forty Five"

Input: n = 2147483647
Output: "Two Billion One Hundred Forty Seven Million Four Hundred Eighty Three Thousand Six Hundred Forty Seven"
```

## Approaches

### 1. By Breaking Down the Number into Groups of Three Digits

The idea is to divide the number into groups of three digits (Thousands, Millions, Billions) and process each group separately. For each group:
1. Convert digits in the Hundreds, Tens, and Units places into words
2. Append the corresponding multiplier (Thousand, Million, Billion)
3. Combine all groups to form the complete representation

#### Python Implementation:
```python
def convert_to_words(n):
    if n == 0:
        return "Zero"
    
    # Words for numbers 0 to 19
    units = ["", "One", "Two", "Three", "Four", "Five", "Six", "Seven",
             "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen",
             "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen",
             "Nineteen"]
    
    # Words for numbers multiple of 10
    tens = ["", "", "Twenty", "Thirty", "Forty", "Fifty",
            "Sixty", "Seventy", "Eighty", "Ninety"]
    
    # Words for place values
    multiplier = ["", "Thousand", "Million", "Billion"]
    
    def convert_group(num):
        if num == 0:
            return ""
        
        # Handle hundreds place
        result = ""
        if num >= 100:
            result += units[num // 100] + " Hundred "
            num %= 100
        
        # Handle tens and ones places
        if num >= 20:
            result += tens[num // 10] + " "
            num %= 10
            if num > 0:
                result += units[num] + " "
        elif num > 0:
            result += units[num] + " "
            
        return result
    
    result = ""
    group_index = 0
    
    while n > 0:
        group = n % 1000
        if group != 0:
            group_words = convert_group(group)
            if group_index > 0:
                group_words += multiplier[group_index] + " "
            result = group_words + result
        group_index += 1
        n //= 1000
    
    return result.strip()

# Test the function
if __name__ == "__main__":
    test_cases = [0, 123, 10245, 2147483647]
    
    for n in test_cases:
        result = convert_to_words(n)
        print(f"Input: n = {n}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static String convertToWords(int n) {
        if (n == 0) 
            return "Zero";
        
        // Words for numbers 0 to 19
        String[] units = {
            "", "One", "Two", "Three", "Four", "Five", "Six", "Seven",
            "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen",
            "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen",
            "Nineteen"
        };
        
        // Words for numbers multiple of 10
        String[] tens = {
            "", "", "Twenty", "Thirty", "Forty", "Fifty",
            "Sixty", "Seventy", "Eighty", "Ninety"
        };
        
        // Words for place values
        String[] multiplier = {"", "Thousand", "Million", "Billion"};
        
        StringBuilder result = new StringBuilder();
        int groupIndex = 0;
        
        while (n > 0) {
            int group = n % 1000;
            if (group != 0) {
                StringBuilder groupWords = new StringBuilder();
                
                // Handle hundreds place
                if (group >= 100) {
                    groupWords.append(units[group / 100])
                             .append(" Hundred ");
                    group %= 100;
                }
                
                // Handle tens and ones places
                if (group >= 20) {
                    groupWords.append(tens[group / 10]).append(" ");
                    group %= 10;
                    if (group > 0) {
                        groupWords.append(units[group]).append(" ");
                    }
                } else if (group > 0) {
                    groupWords.append(units[group]).append(" ");
                }
                
                if (groupIndex > 0) {
                    groupWords.append(multiplier[groupIndex]).append(" ");
                }
                
                result.insert(0, groupWords.toString());
            }
            
            groupIndex++;
            n /= 1000;
        }
        
        return result.toString().trim();
    }

    public static void main(String[] args) {
        // Test cases
        int[] testCases = {0, 123, 10245, 2147483647};
        
        for (int n : testCases) {
            String result = convertToWords(n);
            System.out.println("Input: n = " + n);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(log₁₀(n)), as we divide the number by 1000 in each iteration
**Space Complexity**: O(1), excluding space for output string

### 2. Using Key Numeric Values Mapping

The idea is to store key numeric values and their corresponding English words in descending order. For each value:
1. Compare the number with current numeric value
2. If greater, add word for quotient (number / value)
3. Append word for current value (e.g., "Billion", "Million")
4. Process remainder recursively

#### Python Implementation:
```python
def convert_to_words(n):
    if n == 0:
        return "Zero"
    
    def convert_to_words_rec(n, values, words):
        if n == 0:
            return ""
        
        result = ""
        for i in range(len(values)):
            value = values[i]
            word = words[i]
            
            if n >= value:
                # Append quotient part
                if n >= 100:
                    result += convert_to_words_rec(n // value, values, words) + " "
                
                # Append word for numeric value
                result += word + " "
                
                # Process remainder
                remainder = n % value
                if remainder > 0:
                    result += convert_to_words_rec(remainder, values, words)
                return result.strip()
        
        return ""
    
    # Define numeric values and their words
    values = [1000000000, 1000000, 1000, 100, 90, 80, 70, 60, 50, 40, 30, 20,
              19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    
    words = ["Billion", "Million", "Thousand", "Hundred", "Ninety", "Eighty",
             "Seventy", "Sixty", "Fifty", "Forty", "Thirty", "Twenty",
             "Nineteen", "Eighteen", "Seventeen", "Sixteen", "Fifteen",
             "Fourteen", "Thirteen", "Twelve", "Eleven", "Ten", "Nine",
             "Eight", "Seven", "Six", "Five", "Four", "Three", "Two", "One"]
    
    return convert_to_words_rec(n, values, words)

# Test the function
if __name__ == "__main__":
    test_cases = [0, 123, 10245, 2147483647]
    
    for n in test_cases:
        result = convert_to_words(n)
        print(f"Input: n = {n}")
        print(f"Output: {result}\n")
```

#### Java Implementation:
```java
class Solution {
    static String convertToWords(int n) {
        if (n == 0)
            return "Zero";
            
        int[] values = {1000000000, 1000000, 1000, 100, 90, 80, 70, 60, 50, 40,
                       30, 20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7,
                       6, 5, 4, 3, 2, 1};
                       
        String[] words = {"Billion", "Million", "Thousand", "Hundred", "Ninety",
                         "Eighty", "Seventy", "Sixty", "Fifty", "Forty",
                         "Thirty", "Twenty", "Nineteen", "Eighteen", "Seventeen",
                         "Sixteen", "Fifteen", "Fourteen", "Thirteen", "Twelve",
                         "Eleven", "Ten", "Nine", "Eight", "Seven", "Six",
                         "Five", "Four", "Three", "Two", "One"};
                         
        return convertToWordsRec(n, values, words).trim();
    }
    
    static String convertToWordsRec(int n, int[] values, String[] words) {
        if (n == 0)
            return "";
            
        StringBuilder result = new StringBuilder();
        
        for (int i = 0; i < values.length; i++) {
            int value = values[i];
            String word = words[i];
            
            if (n >= value) {
                // Append quotient part
                if (n >= 100)
                    result.append(convertToWordsRec(n / value, values, words))
                          .append(" ");
                
                // Append word for numeric value
                result.append(word).append(" ");
                
                // Process remainder
                int remainder = n % value;
                if (remainder > 0)
                    result.append(convertToWordsRec(remainder, values, words));
                    
                return result.toString().trim();
            }
        }
        
        return "";
    }

    public static void main(String[] args) {
        // Test cases
        int[] testCases = {0, 123, 10245, 2147483647};
        
        for (int n : testCases) {
            String result = convertToWords(n);
            System.out.println("Input: n = " + n);
            System.out.println("Output: " + result + "\n");
        }
    }
}
```

**Time Complexity**: O(log₁₀(n)), as we divide by key numeric values
**Space Complexity**: O(log₁₀(n)), due to recursion stack

## Key Points

1. First approach (Groups of Three):
   - More intuitive and easier to understand
   - Handles numbers naturally as we read them
   - Good for standard number system representations

2. Second approach (Key Values Mapping):
   - More flexible for different number systems
   - Easier to modify for different languages/formats
   - Uses recursion which can be memory-intensive

3. Common considerations:
   - Both approaches handle zero as a special case
   - Both handle numbers up to billions
   - Pay attention to spacing between words
   - Consider using StringBuilder for better performance in Java