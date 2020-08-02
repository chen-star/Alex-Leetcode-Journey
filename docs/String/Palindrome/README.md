# Palindrome

### DP:

- Substring

~~~java
// s[i:j] is pal or not
// must include s.charAt(i) && s.charAt(j)
dp[i][j] = s.charAt(i) == s.charAt(j) && dp[i + 1][j-1]
~~~

- Subsequence

~~~java
// s[i:j] is pal or not
// dont have to include s.charAt(i) & s.charAt(j)
1. s.charAt(i) == s.charAt(j)
dp[i][j] = dp[i + 1][j - 1] + 2;

2. s.charAt(i) != s.charAt(j)
dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
~~~

---

### Problems

- Longest
	- 5. Longest Palindromic Substring
	- 516. Longest Palindromic Subsequence
	- 1143. Longest Common Subsequence


- Shortest
	- 214. Shortest Palindrome

- Permutations
	- 266. Palindrome Permutation
	- 267. Palindrome Permutation II


- Pairs
	- 336. Palindrome Pairs

- Count
	- 647. Palindromic Substrings
	- 730. Count Different Palindromic Subsequences