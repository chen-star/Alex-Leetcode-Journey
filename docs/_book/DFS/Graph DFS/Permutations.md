# 778. Swim in Rising Water

https://leetcode.com/problems/swim-in-rising-water/

### Method 1

DFS + Memo
memo[i][j] the minPathMax from (0,0) to (i,j)

~~~java

class Solution {
    int m, n;
    int[] dx = new int[]{1,-1,0,0};
    int[] dy = new int[]{0,0,1,-1};
    // pathMax from (0,0) -> (i, j)
    int[][] memo;
    
    public int swimInWater(int[][] grid) {
        m = grid.length;
        if (m == 0) return 0;
        n = grid[0].length;
        if (n == 0) return 0;
        memo = new int[m][n];
        for (int i = 0; i < m; i++) {
            Arrays.fill(memo[i], Integer.MAX_VALUE);
        }
        
        dfs(0,0,grid,new boolean[m][n],grid[0][0]);
        return memo[m-1][n-1];
    }
    
    private void dfs(int x, int y, int[][] grid, boolean[][] visited,
                    int pathMax) {
        if (x == m - 1 && y == n - 1) {
            memo[x][y] = Math.min(memo[x][y], pathMax);
            return;
        }
        if (pathMax >= memo[x][y]) {
            return;
        }
        for (int k = 0; k < 4; k++) {
            int nx = x + dx[k], ny = y + dy[k];
            if (nx < 0 || ny < 0 || nx >= m || ny >= n) continue;
            if (visited[nx][ny]) continue;
            visited[nx][ny] = true;
            dfs(nx, ny, grid, visited, Math.max(pathMax, grid[nx][ny]));
            visited[nx][ny] = false;
        }
        
        memo[x][y] = pathMax;
    }
}

~~~


### Method 2

Dijkstra

~~~java

class Solution {
    public int swimInWater(int[][] grid) {
        int m = grid.length;
        if (m == 0) return 0;
        int n = grid[0].length;
        if (n == 0) return 0;
        
        int[] dx = new int[]{1,-1,0,0};
        int[] dy = new int[]{0,0,1,-1};
        
        Queue<Path> minHeap = new PriorityQueue<>((p1, p2) -> {
            return p1.maxFromStart - p2.maxFromStart;
        });
        minHeap.offer(new Path(0,0,grid[0][0]));
        boolean[][] visited = new boolean[m][n];
        
        while (!minHeap.isEmpty()) {
            Path cur = minHeap.poll();
            if (visited[cur.x][cur.y]) continue;
            if (cur.x == m - 1 && cur.y == n - 1) return cur.maxFromStart;
            visited[cur.x][cur.y] = true;
            
            for (int k = 0; k < 4; k++) {
                int nx = cur.x + dx[k], ny = cur.y + dy[k];
                if (nx < 0 || ny < 0 || nx >= m || ny >= n) continue;
                if (visited[nx][ny]) continue;
                minHeap.offer(new Path(nx, ny, Math.max(cur.maxFromStart, grid[nx][ny])));
            }
        }
        
        return -1;
    }
    
    private static class Path {
        int x, y;
        int maxFromStart;
        
        public Path(int x, int y, int max) {
            this.x = x;
            this.y = y;
            this.maxFromStart = max;
        }
    }
}

~~~