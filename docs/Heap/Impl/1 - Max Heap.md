# Max Heap

~~~java

/**
 * @author Alex CHEN
 * @version 1.0
 * @since 2020-07-26 21:07
 */
public class MaxHeap {
    /**
     * actual data
     */
    int[] arr;
    /**
     * how many numbers exists
     */
    int size;

    public MaxHeap(int[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("input array cannot be null or empty");
        }
        this.arr = arr;
        this.size = arr.length;
        heapify();
    }

    public MaxHeap(int cap) {
        if (cap <= 0) {
            throw new IllegalArgumentException("capacity cannot be <= 0");
        }
        arr = new int[cap];
        size = 0;
    }

    public int size() {
        return this.size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == arr.length;
    }

    public int peek() {
        return arr[0];
    }

    public int poll() {
        if (isEmpty()) return -1;
        int ans = arr[0];
        arr[0] = arr[size - 1];
        size--;
        percolateDown(0);
        return ans;
    }

    public void offer(int val) {
        if (isFull()) {
            throw new UnsupportedOperationException("The heap is full");
        }
        arr[size++] = val;
        percolateUp(size - 1);
    }

    public void update(int idx, int val) {
        if (idx < 0 || idx >= size) {
            throw new UnsupportedOperationException("Index is invalid");
        }
        int lastVal = arr[idx];
        arr[idx] = val;
        if (val > lastVal) {
            percolateUp(idx);
        } else if (val < lastVal) {
            percolateDown(idx);
        }
    }

    private void percolateUp(int idx) {
        int parent = getParent(idx);
        while (parent >= 0 && arr[idx] > arr[parent]) {
            swap(parent, idx);
            idx = parent;
        }
    }

    private void percolateDown(int idx) {
        while (leftChild(idx) < size) {
            int leftChild = leftChild(idx);
            int rightChild = rightChild(idx);
            int candidate = leftChild;
            if (rightChild < size && arr[rightChild] > arr[leftChild]) {
                candidate = rightChild;
            }
            if (arr[candidate] > arr[idx]) {
                swap(idx, candidate);
                idx = candidate;
            } else {
                break;
            }
        }
    }

    private void heapify() {
        for (int i = size / 2 - 1; i >= 0; i--) {
            percolateDown(i);
        }
    }

    private int getParent(int idx) {
        return (idx - 1) / 2;
    }

    private int leftChild(int idx) {
        return idx * 2 + 1;
    }

    private int rightChild(int idx) {
        return idx * 2 + 2;
    }

    private void swap(int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}


~~~