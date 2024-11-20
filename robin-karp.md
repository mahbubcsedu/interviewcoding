### Introduction to Robin Karp Algorithm (Rolling Hash)

The **Rabin-Karp algorithm** is a string matching algorithm that uses a hashing technique to find a substring in a given text. It is efficient for multiple pattern matching or for solving the problem of finding one or more patterns in a text string.

#### Key Concepts of the Rabin-Karp Algorithm

1. **Rolling Hash**:
   - The algorithm computes a hash value for the substring of length `m` (where `m` is the length of the pattern) and compares this hash value with the hash values of the subsequent substrings of length `m` in the main string (text).
   - The **rolling hash** technique allows for efficient recalculation of the hash value for a substring when you slide one character at a time. This avoids recalculating the hash from scratch every time, making the algorithm much more efficient.

2. **Hash Function**:
   - A common hash function is based on polynomial rolling hash: 
     \[
     H(s) = (s_0 \cdot b^0 + s_1 \cdot b^1 + s_2 \cdot b^2 + ... + s_{m-1} \cdot b^{m-1}) \mod p
     \]
     where `b` is the base and `p` is a large prime number.
   - The idea is to compute this hash value for the pattern and then use the rolling hash to compute the hash for every substring of length `m` in the text.
   - **Collision Handling**: Since hash functions can sometimes result in collisions (different substrings having the same hash value), if two hash values match, we need to perform a direct string comparison to confirm the match.

#### Rabin-Karp Algorithm Steps:
1. **Precompute Hash for Pattern**: Compute the hash of the pattern.
2. **Compute Hash for Substrings in the Text**: For each substring of the text of length equal to the pattern, compute its hash.
3. **Compare Hashes**: If the hash of the substring matches the hash of the pattern, do a direct string comparison.
4. **Sliding Window**: Use the rolling hash technique to efficiently calculate the hash for the next substring by sliding the window.

### Algorithm Implementation

Here’s an implementation of the Rabin-Karp algorithm in Python:

```python
def rabin_karp(text, pattern):
    # Constants for the hash function
    d = 256  # Number of characters in the input alphabet (ASCII)
    q = 101  # A large prime number for modulus
    
    m = len(pattern)
    n = len(text)
    
    # Compute the hash value of the pattern and the first window of text
    p_hash = 0  # Hash value for pattern
    t_hash = 0  # Hash value for text
    h = 1
    
    # The value of h will be "d^(m-1) % q"
    for i in range(m-1):
        h = (h * d) % q
    
    # Compute initial hash values for pattern and text
    for i in range(m):
        p_hash = (d * p_hash + ord(pattern[i])) % q
        t_hash = (d * t_hash + ord(text[i])) % q
    
    # Start searching
    for i in range(n - m + 1):
        # If the hash values match, then only check the strings
        if p_hash == t_hash:
            if text[i:i + m] == pattern:
                print(f"Pattern found at index {i}")
        
        # Calculate hash for the next window of text
        if i < n - m:
            t_hash = (d * (t_hash - ord(text[i]) * h) + ord(text[i + m])) % q
            
            # We might get negative values of t_hash, so converting it to positive
            if t_hash < 0:
                t_hash = t_hash + q

# Example Usage
text = "ABABABCABABABCABABABC"
pattern = "ABABC"
rabin_karp(text, pattern)
```

### Explanation of the Code:
1. **d** and **q** are constants for the hash function. `d` is the size of the alphabet (256 for ASCII characters) and `q` is a prime number used for modulus to reduce hash collisions.
2. We calculate the hash of the pattern and the first substring of the text. We then slide over the text, updating the hash using the rolling hash technique.
3. If the hash of a substring matches the hash of the pattern, we check the actual substring for a match.
4. The rolling hash allows us to update the hash efficiently by using the formula:
   \[
   \text{new\_hash} = (d \times (\text{old\_hash} - \text{ord(text[i])} \times h) + \text{ord(text[i + m])}) \% q
   \]
   where `h = d^(m-1) % q` is the precomputed value for sliding the window.

### Medium to Hard Level Problems Solvable by Rabin-Karp

1. **Multiple Pattern Matching**:
   - Given a text and multiple patterns, find all occurrences of any of the patterns in the text. Rabin-Karp can efficiently handle this with multiple hash checks for each pattern.
   - **Example Problem**: "Find all occurrences of the given list of words in a text."

2. **Longest Common Substring**:
   - Find the longest common substring between two strings. Rabin-Karp can be used to find the hash of substrings and compare them.
   - **Example Problem**: "Given two strings, find the longest common substring."

3. **String Search with Wildcards**:
   - Searching for a pattern in a text where the pattern contains wildcard characters (e.g., `*` or `?`).
   - **Example Problem**: "Find all substrings matching a pattern with wildcards."

4. **String Matching with Duplicates**:
   - Find all occurrences of a string in a text, including those with duplicates.
   - **Example Problem**: "Find all repeated substrings of length k in a given string."

5. **Anagram Substring Search**:
   - Given a text and a pattern, check if any anagram of the pattern exists as a substring in the text.
   - **Example Problem**: "Find all anagrams of a pattern in a given text."

6. **Finding Patterns with Constraints**:
   - Finding substrings that match a certain pattern or satisfy a condition, such as checking for substrings where the sum of ASCII values of the characters is a certain number.
   - **Example Problem**: "Given a string, find all substrings where the sum of ASCII values of characters is greater than X."

---

### Some Example Implementations

#### Problem 1: **Multiple Pattern Matching**

```python
def multiple_pattern_matching(text, patterns):
    for pattern in patterns:
        print(f"Searching for pattern: {pattern}")
        rabin_karp(text, pattern)

patterns = ["ABABC", "BABC", "CABA"]
text = "ABABABCABABABCABABABC"
multiple_pattern_matching(text, patterns)
```

#### Problem 2: **Anagram Substring Search**

```python
from collections import Counter

def find_anagram_substrings(text, pattern):
    m, n = len(pattern), len(text)
    pattern_counter = Counter(pattern)
    window_counter = Counter(text[:m])
    
    # Check the first window
    if window_counter == pattern_counter:
        print(f"Anagram found at index 0")
    
    for i in range(1, n - m + 1):
        window_counter[text[i-1]] -= 1
        if window_counter[text[i-1]] == 0:
            del window_counter[text[i-1]]
        
        window_counter[text[i+m-1]] += 1
        
        if window_counter == pattern_counter:
            print(f"Anagram found at index {i}")

text = "cbaebabacd"
pattern = "abc"
find_anagram_substrings(text, pattern)
```

### Conclusion
Rabin-Karp with rolling hash is an efficient approach for solving string matching problems. It is particularly useful for handling multiple pattern matching or
searching for anagrams or substrings. Its time complexity can be improved using the rolling hash technique, and it's often preferred when multiple substring matches are 
required in large texts.
