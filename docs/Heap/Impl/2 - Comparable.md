# Heap With Comparable

~~~java

/**
 * @author Alex CHEN
 * @version 1.0
 * @since 2020-07-26 21:07
 */
public class ComparableHeap {
    /**
     * actual data
     */
    String[] arr;
    /**
     * how many numbers exists
     */
    int size;

    public ComparableHeap(String[] arr) {
        if (arr == null || arr.length == 0) {
            throw new IllegalArgumentException("input array cannot be null or empty");
        }
        this.arr = arr;
        this.size = arr.length;
        heapify();
    }

    public ComparableHeap(int cap) {
        if (cap <= 0) {
            throw new IllegalArgumentException("capacity cannot be <= 0");
        }
        arr = new String[cap];
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

    public String peek() {
        return arr[0];
    }

    public String poll() {
        if (isEmpty()) return "-1";
        String ans = arr[0];
        arr[0] = arr[size - 1];
        size--;
        percolateDown(0);
        return ans;
    }

    public void offer(String val) {
        if (isFull()) {
            throw new UnsupportedOperationException("The heap is full");
        }
        arr[size++] = val;
        percolateUp(size - 1);
    }

    public void update(int idx, String val) {
        if (idx < 0 || idx >= size) {
            throw new UnsupportedOperationException("Index is invalid");
        }
        String lastVal = arr[idx];
        arr[idx] = val;
        if (val.compareTo(lastVal) < 0) {
            percolateUp(idx);
        } else if (val.compareTo(lastVal) > 0) {
            percolateDown(idx);
        }
    }

    private void percolateUp(int idx) {
        int parent = getParent(idx);
        while (parent >= 0 && arr[idx].compareTo(arr[parent]) < 0) {
            swap(parent, idx);
            idx = parent;
        }
    }

    private void percolateDown(int idx) {
        while (leftChild(idx) < size) {
            int leftChild = leftChild(idx);
            int rightChild = rightChild(idx);
            int candidate = leftChild;
            if (rightChild < size && arr[rightChild].compareTo(arr[leftChild]) < 0) {
                candidate = rightChild;
            }
            if (arr[candidate].compareTo(arr[idx]) < 0) {
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
        String temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}


~~~