# LEETCODE-Hash_Table-726
This Java class `Solution` processes a chemical formula given as a string and returns the count of each atom in the formula. The algorithm uses recursion to handle nested parentheses in the formula. Here's an overview of how it works:

1. **Main Method: `countOfAtoms`**
   - This method initializes a `StringBuilder` to build the result string and a map to store the count of each atom.
   - It calls the `parse` method to fill the map with the counts.
   - Finally, it constructs the result string by appending each element and its count (if greater than 1) to the `StringBuilder`.

2. **Recursive Method: `parse`**
   - This method parses the formula string recursively.
   - It uses a map to keep track of the count of each element.
   - When encountering '(', it recursively calls itself to process the sub-formula inside the parentheses.
   - When encountering ')', it multiplies the counts of the elements inside the parentheses by the number following the ')'.
   - For other characters, it extracts the element and its count and updates the map.

3. **Helper Methods: `getElem` and `getNum`**
   - `getElem` extracts the element name from the formula string.
   - `getNum` extracts the count of the element from the formula string.

Here is the corrected and properly formatted code:

```java
import java.util.Map;
import java.util.TreeMap;

class Solution {
  public String countOfAtoms(String formula) {
    StringBuilder sb = new StringBuilder();
    Map<String, Integer> count = parse(formula);

    for (final String elem : count.keySet())
      sb.append(elem + (count.get(elem) == 1 ? "" : String.valueOf(count.get(elem))));

    return sb.toString();
  }

  private int i = 0;

  private Map<String, Integer> parse(String s) {
    Map<String, Integer> count = new TreeMap<>();

    while (i < s.length()) {
      if (s.charAt(i) == '(') {
        ++i; // Skip '('
        for (Map.Entry<String, Integer> entry : parse(s).entrySet()) {
          final String elem = entry.getKey();
          final int freq = entry.getValue();
          count.merge(elem, freq, Integer::sum);
        }
      } else if (s.charAt(i) == ')') {
        ++i; // Skip ')'
        final int num = getNum(s);
        for (final String elem : count.keySet()) {
          final int freq = count.get(elem);
          count.put(elem, freq * num);
        }
        return count; // Return back to the previous scope.
      } else {
        final String elem = getElem(s);
        final int num = getNum(s);
        count.merge(elem, num, Integer::sum);
      }
    }

    return count;
  }

  private String getElem(final String s) {
    final int elemStart = i++; // `s[elemStart]` is uppercased.
    while (i < s.length() && Character.isLowerCase(s.charAt(i)))
      ++i;
    return s.substring(elemStart, i);
  }

  private int getNum(final String s) {
    final int numStart = i;
    while (i < s.length() && Character.isDigit(s.charAt(i)))
      ++i;
    final String numString = s.substring(numStart, i);
    return numString.isEmpty() ? 1 : Integer.parseInt(numString);
  }
}
```

This implementation assumes the input formula is valid and well-formed. The `parse` method uses a `TreeMap` to maintain the order of elements lexicographically. The helper methods `getElem` and `getNum` are used to extract element names and their counts, respectively. The recursive nature of `parse` allows it to handle nested parentheses effectively.
