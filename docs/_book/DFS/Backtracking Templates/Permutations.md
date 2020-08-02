# Permutations


## 46. Permutations
https://leetcode.com/problems/permutations/

~~~java

class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(nums, new ArrayList<>(), new boolean[nums.length], ans);
        return ans;
    }
    
    private void dfs(int[] nums, List<Integer> perm, boolean[] visited,
                    List<List<Integer>> ans) {
        if (perm.size() == nums.length) {
            ans.add(new ArrayList<>(perm));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (!visited[i]) {
                perm.add(nums[i]);
                visited[i] = true;
                dfs(nums, perm, visited, ans);
                perm.remove(perm.size() - 1);
                visited[i] = false;
            }
        }
    }
}

~~~


## 47. Permutations II
https://leetcode.com/problems/permutations-ii/

~~~java

class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        dfs(nums, new boolean[nums.length], new ArrayList<>(), ans);
        return ans;
    }
    
    private void dfs(int[] nums, boolean[] visited, List<Integer> perm,
                    List<List<Integer>> ans) {
        if (perm.size() == nums.length) {
            ans.add(new ArrayList<>(perm));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) continue;
            if (i > 0 && nums[i] == nums[i-1] && !visited[i-1]) continue;
            perm.add(nums[i]);
            visited[i] = true;
            dfs(nums, visited, perm, ans);
            visited[i] = false;
            perm.remove(perm.size() - 1);
        }
    }
}

~~~