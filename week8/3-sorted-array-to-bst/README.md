# 3. Convert Sorted Array to Binary Search Tree

## 1. Problem
Given an integer array `nums` sorted in ascending order, we need to convert it into a height-balanced Binary Search Tree (a BST where the depth of the two subtrees of every node never differs by more than 1).

## 2. Approach
If we simply inserted the sorted elements one by one from left to right, we would end up with a completely skewed tree (essentially a linked list). To keep the tree balanced, the root of any subtree must be the middle element of its sorted range so that half the remaining elements go to the left and half go to the right:
1. Use two index pointers, `left` and `right`, to track the current portion of the array.
2. If `left > right`, the subarray is empty, so return `null`.
3. Find the middle index `mid = left + (right - left) / 2` and create a new `TreeNode` with `nums[mid]`.
4. Recursively build `root.left` using the left half (`left` to `mid - 1`) and `root.right` using the right half (`mid + 1` to `right`).

### Written Trace
Suppose `nums = [-10, -3, 0, 5, 9]`:
* **Step 1:** `left = 0, right = 4`. `mid = 2` (`nums[2] = 0`). Root is `0`.
* **Step 2 (Left half `0..1`):** `left = 0, right = 1`. `mid = 0` (`nums[0] = -10`). Left child of `0` is `-10`.
  * Left of `-10` (`0..-1`): `left > right` -> returns `null`.
  * Right of `-10` (`1..1`): `mid = 1` (`nums[1] = -3`) -> right child of `-10` is `-3`.
* **Step 3 (Right half `3..4`):** `left = 3, right = 4`. `mid = 3` (`nums[3] = 5`). Right child of `0` is `5`.
  * Left of `5` (`3..2`): `left > right` -> returns `null`.
  * Right of `5` (`4..4`): `mid = 4` (`nums[4] = 9`) -> right child of `5` is `9`.
* **Final Tree:** Root `0`, left subtree `-10 -> -3`, right subtree `5 -> 9` (height-balanced).

### Challenges Faced
Initially, I thought about creating new subarrays using `Arrays.copyOfRange()` for the left and right halves during each recursive call. I realized that copying arrays at every step would waste extra memory and slow down the algorithm. Passing `left` and `right` index boundaries instead solved this cleanly without copying any array elements. Another small detail was writing `left + (right - left) / 2` instead of `(left + right) / 2` to avoid potential integer overflow on large arrays.

## 3. Complexity Analysis
* **Time Complexity:** `O(n)`, where `n` is the number of elements in `nums`.
  * **Why:** Every element in the array is visited exactly once to construct a single `TreeNode`, and calculating `mid` takes `O(1)` time per node.
* **Space Complexity:** `O(log n)` auxiliary space (or `O(n)` if counting the output tree itself).
  * **Why:** Because we always split the array in half, the resulting tree is guaranteed to be balanced with height `O(log n)`, meaning the recursion call stack never goes deeper than `log n` frames.

## 4. Reflection / Improvement
* **Is there a more efficient approach?** No, `O(n)` time and `O(log n)` auxiliary stack space is already optimal because we have to create `n` nodes to build the output tree.
* **What would you need to change?** If we wanted to avoid recursion completely, we could simulate the process iteratively using a `Stack` or `Queue` that stores tuples/objects containing `(TreeNode, leftIndex, rightIndex)`.
* **Improved Complexity:** The time complexity would remain `O(n)` and auxiliary space would still be `O(n)` for the queue/stack, so the recursive divide-and-conquer solution is cleaner and equally optimal.
