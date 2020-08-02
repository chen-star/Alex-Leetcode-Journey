# 485. Max Consecutive Ones
https://leetcode.com/problems/max-consecutive-ones/

## Method 1

DP

~~~java

class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;
        
        int[] dp = new int[n];
        int longest = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                dp[i] = 0;
            } else {
                dp[i] = i > 0 ? dp[i-1] + 1 : 1;
            }
            longest = Math.max(longest, dp[i]);
        }
        
        return longest;
    }
}
    
~~~

## Method 2

Compress DP Space

~~~java

class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;
        
        int curLen = 0;
        int longest = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                curLen = 0;
            } else {
                curLen++;
            }
            longest = Math.max(longest, curLen);
        }
        
        return longest;
    }
}
    
~~~



# 1446. Consecutive Characters
https://leetcode.com/problems/consecutive-characters/
## Method 1

DP

~~~java

class Solution {
    public int maxPower(String s) {
        int n = s.length();
        int[] dp = new int[n];
        dp[0] = 1;
        
        int longest = 1;
        for (int i = 1; i < n; i++) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                dp[i] = dp[i-1] + 1;
            } else {
                dp[i] = 1;
            }
            longest = Math.max(longest, dp[i]);
        }
        
        return longest;
    }
}
    
~~~


## Method 2

Compress DP Space

~~~java

class Solution {
    public int maxPower(String s) {
        int n = s.length();
        
        int longest = 1;
        int curLen = 1;
        for (int i = 1; i < n; i++) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                curLen++;
            } else {
                curLen = 1;
            }
            longest = Math.max(longest, curLen);
        }
        
        return longest;
    }
}
    
~~~




# 487. Max Consecutive Ones II
https://leetcode.com/problems/max-consecutive-ones-ii/

~~~java

class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;
        int i = 0, j = 0;
        int[] cnt = new int[2];
        
        // how many 0 in current substring
        int counter = 0;
        int longest = 0;
        while (i < n) {
            cnt[nums[i]]++;
            if (nums[i] == 0) {
                counter++;
            }
            
            while (counter > 1) {
                cnt[nums[j]]--;
                if (nums[j] == 0) {
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


# 1004. Max Consecutive Ones III
https://leetcode.com/problems/max-consecutive-ones-iii/

~~~java

class Solution {
    public int longestOnes(int[] A, int K) {
        int n = A.length;
        int i = 0, j = 0;
        int[] cnt = new int[2];
        
        // how many 0 in current substring
        int counter = 0;
        int longest = 0;
        while (i < n) {
            cnt[A[i]]++;
            if (A[i] == 0) {
                counter++;
            }
            
            while (counter > K) {
                cnt[A[j]]--;
                if (A[j] == 0) {
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