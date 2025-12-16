# Searching Algorithms

## Introduction

Searching algorithms find elements in data structures. This tutorial covers linear search, binary search, and their implementations.

---

## Linear Search

    function linearSearch(arr, target) {
      for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) return i;
      }
      return -1;
    }

---

## Binary Search

    function binarySearch(arr, target) {
      let left = 0, right = arr.length - 1;
      while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
      }
      return -1;
    }

---

## Conclusion

Use linear search for unsorted data and binary search for sorted data. Binary search is more efficient for large datasets.

