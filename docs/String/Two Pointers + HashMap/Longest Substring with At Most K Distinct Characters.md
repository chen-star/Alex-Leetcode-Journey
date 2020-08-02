# 159. Longest Substring with At Most Two Distinct Characters
https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/

## 

~~~java

class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        int i = 0, j = 0;
        int n = s.length();
        Map<Character, Integer> map = new HashMap<>();
        
        int longest = 0;
        while (i < n) {
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
            while (map.size() > 2) {
                map.put(s.charAt(j), map.get(s.charAt(j)) - 1);
                if (map.get(s.charAt(j)) == 0) {
                    map.remove(s.charAt(j));
                }
                j++;
            }
            longest = Math.max(longest, i - j + 1);
            i++;
        }
        
        return longest;
    }
}
    
~~~


# 340. Longest Substring with At Most K Distinct Characters
https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/

## Method 1

~~~java

class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int i = 0, j = 0;
        int n = s.length();
        Map<Character, Integer> map = new HashMap<>();
        
        int longest = 0;
        while (i < n) {
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
            while (map.size() > k) {
                map.put(s.charAt(j), map.get(s.charAt(j)) - 1);
                if (map.get(s.charAt(j)) == 0) {
                    map.remove(s.charAt(j));
                }
                j++;
            }
            
            longest = Math.max(longest, i - j + 1);
            i++;
        }
        
        return longest;
    }
}
    
~~~

## Method 2

With counter

~~~java

class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int i = 0, j = 0;
        int n = s.length();
        Map<Character, Integer> map = new HashMap<>();
        
        int counter = 0;
        int longest = 0;
        while (i < n) {
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
            if (map.get(c) == 1) {
                counter++;
            }
            
            while (counter > k) {
                map.put(s.charAt(j), map.get(s.charAt(j)) - 1);
                if (map.get(s.charAt(j)) == 0) {
                    counter--;
                }
                j++;
            }
            
            longest = Math.max(longest, i - j + 1);
            i++;
        }
        
        return longest;
    }
}
    
~~~
