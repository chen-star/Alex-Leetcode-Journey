## Dijkstra
---


### Overview

- Target:
	- The shortest paths from one node to all others in the graph.

- Assumption:
	- The cost of each edge cannot be negative

- Note:
	- For one node, it can be `pushed` into queue multiple times (ie. The same node with different costs can exists in queue)
		- As such, dedup should be at the time of `pop` 

	- From the whole graph point of view, all nodes popped out in increasing order. (ie. could be used to sort in some format)

---


### Template

- Time Complexity: `O(NlgN)`

- Space Complexity: `O(N)`


#### Version 1

HashSet

~~~java
	
	/**
	 * times[i]: {u, v, cost} 
	 * N: The total # of nodes
	 * K: Source node
	 */
    public int minDistFromKToAll(int[][] times, int N, int K) {
        // construct graph
        // key: u, value: Pair (v, cost)
        Map<Integer, List<Pair>> graph = constructGraph(times, N);
        
        // init minHeap & visited nodes set
        Queue<Pair> minHeap = new PriorityQueue<>((p1, p2) -> {
            return p1.cost - p2.cost;
        });
        minHeap.offer(new Pair(K, 0));
        Set<Integer> visited = new HashSet<>();
        
        // visit every node in the graph
        // every time a node is popped up, the cost (from source to this node) is 
        // the shortest path for this node
        int maxTime = 0;
        while (!minHeap.isEmpty()) {
            Pair cur = minHeap.poll();
            // we might add a node multiple times
            // only the first time counts
            if (!visited.add(cur.id)) continue;
            maxTime = cur.cost;
            
            for (Pair next : graph.get(cur.id)) {
                // visited.contains(next.id) means
                // the shortest dist from K to next is calculated
                if (visited.contains(next.id)) continue;
                minHeap.offer(new Pair(next.id, cur.cost + next.cost));
            }
        }
        
        return visited.size() == N ? maxTime : -1;
    }

~~~



#### Version 2

Dist array

~~~java

    public int minDistFromKToAll(int[][] times, int N, int K) {
        // construct graph
        // key: u, value: Pair (v, cost)
        Map<Integer, List<Pair>> graph = constructGraph(times, N);
        
        // init minHeap & visited nodes set
        Queue<Pair> minHeap = new PriorityQueue<>((p1, p2) -> {
            return p1.cost - p2.cost;
        });
        minHeap.offer(new Pair(K, 0));
        int[] dist = new int[N + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        
        // visit every node in the graph
        // every time a node is popped up, the cost (from source to this node) is 
        // the shortest path for this node
        int maxTime = 0;
        while (!minHeap.isEmpty()) {
            Pair cur = minHeap.poll();
            // the dist has been updated
            if (dist[cur.id] != Integer.MAX_VALUE) continue;
            dist[cur.id] = cur.cost;
            maxTime = cur.cost;
            
            for (Pair next : graph.get(cur.id)) {
                if (dist[next.id] != Integer.MAX_VALUE) continue;
                minHeap.offer(new Pair(next.id, cur.cost + next.cost));
            }
        }
        
        return Arrays.stream(dist)
                    .filter(d -> d == Integer.MAX_VALUE)
                    .count() == 1 ? maxTime : -1;
    }

~~~