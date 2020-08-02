# Combination Sum


## 39. Combination Sum
https://leetcode.com/problems/combination-sum/

No dup, each num can be chosen many times

~~~java

// Method 1
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] candidates, int idx, int target,
                    List<Integer> comb, List<List<Integer>> ans) {
        if (idx == candidates.length) {
            if (target == 0) {
                ans.add(new ArrayList<>(comb));
            }
            return;
        }
        
        int maxNum = target / candidates[idx];
        for (int i = 0; i <= maxNum; i++) {
            comb.addAll(Collections.nCopies(i, candidates[idx]));
            dfs(candidates, idx + 1, target - candidates[idx] * i, comb, ans);
            for (int j = 0; j < i; j++) {
                comb.remove(comb.size() - 1);
            }
        }
    }
}

// Method 2
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates);
        dfs(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] candidates, int idx, int target,
                    List<Integer> comb, List<List<Integer>> ans) {
        if (target == 0) {
            ans.add(new ArrayList<>(comb));
            return;
        }
        
        for (int i = idx; i < candidates.length; i++) {
            if (target - candidates[i] < 0) break;
            comb.add(candidates[i]);
            dfs(candidates, i, target - candidates[i], comb, ans);
            comb.remove(comb.size() - 1);
        }
    }
}

~~~


## 40. Combination Sum II
https://leetcode.com/problems/combination-sum-ii/

Dup, each num can only be chosen once

~~~java

class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates);
        dfs(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] candidates, int idx, int target, List<Integer> comb,
                    List<List<Integer>> ans) {
        if (target == 0) {
            ans.add(new ArrayList<>(comb));
            return;
        } else if (idx == candidates.length) return;
        
        for (int i = idx; i < candidates.length; i++) {
            if (candidates[i] > target) break;
            if (i > idx && candidates[i] == candidates[i-1]) continue;
            comb.add(candidates[i]);
            dfs(candidates, i + 1, target - candidates[i], comb, ans);
            comb.remove(comb.size() - 1);
        }
    }
}


~~~

## 216. Combination Sum III
https://leetcode.com/problems/combination-sum-iii/

find each comb of size k

~~~java

class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(k, n, 1, new ArrayList<>(),ans);
        return ans;
    }
    
    private void dfs(int k, int target, int cur, List<Integer> comb, List<List<Integer>> ans) {
        if (comb.size() == k) {
            if (target == 0) {
                ans.add(new ArrayList<>(comb));
            }
            return;
        }
        
        for (int i = cur; i <= 9; i++) {
            if (i > target) break;
            comb.add(i);
            dfs(k, target - i, i + 1, comb, ans);
            comb.remove(comb.size() - 1);
        }
    }
}

~~~

## 377. Combination Sum IV
https://leetcode.com/problems/combination-sum-iv/

No dup, only gets comb num

~~~java

class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        for (int n : nums) {
            if (n < target + 1) {
                dp[n] = 1;
            }
        }
        
        for (int i = 0; i < target + 1; i++) {
            for (int n : nums) {
                if (i - n >= 0) {
                    dp[i] += dp[i-n];
                }
            }
        }
        
        return dp[target];
    }
}


~~~