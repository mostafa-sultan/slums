# Sorting Algorithms

## Introduction

Sorting algorithms organize data in a specific order. This tutorial covers bubble sort, quick sort, merge sort, and their implementations.

---

## Bubble Sort

    function bubbleSort(arr) {
      for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
          if (arr[j] > arr[j + 1]) {
            [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
          }
        }
      }
      return arr;
    }

---

## Quick Sort

    function quickSort(arr) {
      if (arr.length <= 1) return arr;
      const pivot = arr[Math.floor(arr.length / 2)];
      const left = arr.filter(x => x < pivot);
      const middle = arr.filter(x => x === pivot);
      const right = arr.filter(x => x > pivot);
      return [...quickSort(left), ...middle, ...quickSort(right)];
    }

---

## Conclusion

Choose sorting algorithms based on data size and requirements. Quick sort is generally efficient for most cases.

