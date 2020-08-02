# Subsets


## 78. Subsets
https://leetcode.com/problems/subsets/

~~~java

// Method 1
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(nums, 0, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] nums, int idx, List<Integer> sub, List<List<Integer>> ans) {
        if (idx == nums.length) {
            ans.add(new ArrayList<>(sub));
            return;
        }
        
        // not choose nums[idx]
        dfs(nums, idx + 1, sub, ans);
        
        // choose nums[idx]
        sub.add(nums[idx]);
        dfs(nums, idx + 1, sub, ans);
        sub.remove(sub.size() - 1);
    }
}

// Method 2
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(nums, 0, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] nums, int idx, List<Integer> sub, List<List<Integer>> ans) {
        ans.add(new ArrayList<>(sub));
        if (idx == nums.length) return;
        
        for (int i = idx; i < nums.length; i++) {
            sub.add(nums[i]);
            dfs(nums, i + 1, sub, ans);
            sub.remove(sub.size() - 1);
        }
    }
}

~~~


## 90. Subsets II
https://leetcode.com/problems/subsets-ii/

~~~java

class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        dfs(nums, 0, new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] nums, int idx, List<Integer> sub, 
                    List<List<Integer>> ans) {
        ans.add(new ArrayList<>(sub));
        if (idx == nums.length) return;
        
        for (int i = idx; i < nums.length; i++) {
            if (i > idx && nums[i] == nums[i-1]) continue;
            sub.add(nums[i]);
            dfs(nums, i + 1, sub, ans);
            sub.remove(sub.size() - 1);
        }
    }
}

~~~