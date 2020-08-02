# Heap With Comparator

~~~java

import java.util.Comparator;

/**
 * @author Alex CHEN
 * @version 1.0
 * @since 2020-07-26 21:53
 */
public class ComparatorHeap {

    private Student[] students;
    private int size;
    private StudentComparator comparator;

    public ComparatorHeap(Student[] students, StudentComparator comparator) {
        this.students = students;
        this.size = students.length;
        this.comparator = comparator;
        heapify();
    }

    public ComparatorHeap(int cap, StudentComparator comparator) {
        students = new Student[cap];
        this.size = 0;
        this.comparator = comparator;
    }

    public int size() {
        return this.size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == students.length;
    }

    public Student peek() {
        return students[0];
    }

    public Student poll() {
        Student ans = students[0];
        students[0] = students[size - 1];
        size--;
        percolateDown(0);
        return ans;
    }

    public void offer(Student s) {
        students[size++] = s;
        percolateUp(size - 1);
    }

    public void update(int idx, Student s) {
        Student prev = students[idx];
        students[idx] = s;
        if (comparator.compare(prev, s) < 0) {
            percolateDown(idx);
        } else if (comparator.compare(prev, s) > 0) {
            percolateUp(idx);
        }
    }

    private void percolateUp(int idx) {
        int parent = getParent(idx);
        while (parent >= 0 && comparator.compare(students[parent], students[idx]) > 0) {
            swap(parent, idx);
            idx = parent;
            parent = getParent(idx);
        }
    }

    private void percolateDown(int idx) {
        while (leftChild(idx) < size) {
            int leftChild = leftChild(idx);
            int rightChild  = rightChild(idx);
            int candidate = leftChild;
            if (rightChild < size && comparator.compare(students[candidate],students[rightChild]) > 0) {
                candidate = rightChild;
            }
            if (comparator.compare(students[candidate], students[idx]) < 0) {
                swap(candidate, idx);
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

    private int leftChild(int parent) {
        return parent * 2 + 1;
    }

    private int rightChild(int parent) {
        return parent * 2 + 2;
    }

    private void swap(int i, int j) {
        Student temp = students[i];
        students[i] = students[j];
        students[j] = temp;
    }


    private static class Student {
        int id;
        int score;

        public Student(int id, int score) {
            this.id = id;
            this.score = score;
        }

        @Override
        public String toString() {
            return "[" + id + "," + score + "]";
        }
    }

    private static class StudentComparator implements Comparator<Student> {

        @Override
        public int compare(Student s1, Student s2) {
            if (s1.score == s2.score) return 0;
            return s1.score < s2.score ? -1 : 1;
        }
    }



~~~