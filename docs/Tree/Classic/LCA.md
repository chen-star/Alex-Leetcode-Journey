# LCA


## LCA 1

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

2 Nodes must be in the tree

~~~java

    /**
     * no node in the tree rooted at root: return null
     * one of the nodes in this tree: return this node
     * two all in this tree: return LCA
     */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return null;
        if (root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null && right == null) return null;
        else if (left != null && right != null) return root;
        else if (left == null) return right;
        else return left;
    }
    
~~~


##  LCA 2

https://www.lintcode.com/problem/lowest-common-ancestor-iii/description

2 Nodes may not be in the tree

~~~java

    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode p, TreeNode q) {
        // write your code here
        Result result = helper(root, p, q);
        if (result.foundP && result.foundQ) {
            return result.node;
        }
        return null;
    }
    
    private Result helper(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return new Result(null, false, false);
        }
        
        Result left = helper(root.left, p, q);
        Result right = helper(root.right, p, q);
        
        boolean foundP = left.foundP || right.foundP || root == p;
        boolean foundQ = left.foundQ || right.foundQ || root == q;
        
        if (root == p || root == q) {
            return new Result(root, foundP, foundQ); 
        } else {
            if (left.foundP && right.foundQ || left.foundQ && right.foundP) {
                return new Result(root, true, true);
            } else if (left.foundP || right.foundP) {
                return new Result((left.foundP ? left.node : right.node), foundP, foundQ);
            } else if (left.foundQ || right.foundQ) {
                return new Result((left.foundQ ? left.node : right.node), foundP, foundQ);
            } else {
                return new Result(null, false, false);
            }
        }
    }
    
    private static class Result {
        TreeNode node;
        boolean foundP, foundQ;
        
        public Result(TreeNode n, boolean p, boolean q) {
            node = n;
            foundP = p;
            foundQ = q;
        }
    }

~~~

##  LCA 3
https://www.lintcode.com/problem/lowest-common-ancestor-ii/description

Root is not given, every child has a parent pointer instead


~~~java

    public ParentTreeNode lowestCommonAncestorII(ParentTreeNode root, ParentTreeNode p, ParentTreeNode q) {
        // get nodeToRoot path length
        int lengthP = getNodeToRootLength(p);
        int lenghtQ = getNodeToRootLength(q);
        
        // the larger one move diff first
        int diff = Math.abs(lenghtQ - lengthP);
        if (lenghtQ > lengthP) {
            for (int i = 0; i < diff; i++) {
                q = q.parent;
            }
        } else if (lenghtQ < lengthP) {
            for (int i = 0; i < diff; i++) {
                p = p.parent;
            }
        }
        
        // move together
        // when they meet, the node is the LCA
        while (p != q) {
            p = p.parent;
            q = q.parent;
        }
        
        return p;
    }
    
    private int getNodeToRootLength(ParentTreeNode n) {
        int len = 0;
        while (n != null) {
            n = n.parent;
            len++;
        }
        return len;
    }

~~~


##  LCA 4

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

LCA of a BST

~~~java

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (p.val < root.val && q.val < root.val || p.val > root.val && q.val > root.val) {
            if (p.val < root.val && q.val < root.val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return root;
    }
    
~~~



##  LCA 5
https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/

LCA of all leaves with deepest depth


~~~java

class Solution {
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        return helper(root).node;
    }
    
    private Result helper(TreeNode root) {
        if (root == null) {
            return new Result(0, null);
        }
        
        Result left = helper(root.left);
        Result right = helper(root.right);
        
        if (left.height < right.height) {
            right.height++;
            return right;
        } else if (left.height > right.height) {
            left.height++;
            return left;
        } else {
            return new Result(left.height + 1, root);
        }
    }
    
    private static class Result {
        int height;
        TreeNode node;
        
        public Result (int height, TreeNode node) {
            this.height = height;
            this.node = node;
        }
    }
}

~~~


##  LCA 6
https://leetcode.com/problems/smallest-common-region/

LCA in graph

~~~java

    public String findSmallestRegion(List<List<String>> regions, String region1, String region2) {
        Map<String, String> parents = new HashMap<>();
        for (List<String> r : regions) {
            for (int i = 1; i < r.size(); i++) {
                parents.put(r.get(i), r.get(0));
            }
        }
        
        // record the path from region1 to top root
        Set<String> path1 = new HashSet<>();
        while (region1 != null) {
            path1.add(region1);
            region1 = parents.get(region1);
        }
        
        // region2 goes towards root, when it first meet a node in the path1
        // this is its LCA
        while (region2 != null) {
            if (path1.contains(region2)) return region2;
            region2 = parents.get(region2);
        }
        
        return null;
    }
    
~~~