# 1. Search in a Binary Search Tree

## 1. Problem
Given the root of a Binary Search Tree (BST) and a target integer `val`, we need to find the node whose value equals `val` and return the subtree rooted at that node. If the value does not exist in the tree, we return `null`.

## 2. Approach
Because this is a Binary Search Tree, every node follows a strict ordering rule: all values in the left subtree are smaller than the current node, and all values in the right subtree are larger. Instead of checking every single node in the tree, I used recursion to cut the search space in half at each step:
1. If the current node is `null` (meaning we reached the bottom without finding it) or its value matches `val`, return the current node.
2. If `val` is smaller than the current node's value, recursively search the left child.
3. Otherwise, recursively search the right child.

### Written Trace
Suppose the tree has root `4`, left child `2` (with children `1` and `3`), and right child `7`. We are searching for `val = 2`:
* **Step 1:** Start at `root = 4`. Is `4 == 2`? No. Since `2 < 4`, move to `root.left` (`2`).
* **Step 2:** Now at node `2`. Is `2 == 2`? Yes. Return node `2` (along with its attached children `1` and `3`).

### Challenges Faced
At first, I considered writing a standard traversal that checks both `root.left` and `root.right` until it finds a match, similar to searching a regular binary tree. However, I realized that approach wastes time visiting branches that cannot possibly contain the target value. Remembering the BST ordering rule fixed this and made the logic much simpler.

## 3. Complexity Analysis
* **Time Complexity:** `O(H)`, where `H` is the height of the tree. In a balanced BST, this is `O(log n)`, while in the worst case (a completely skewed tree shaped like a linked list), it is `O(n)`.
  * **Why:** At each recursive call, we move down one level of the tree and ignore the other half. The maximum number of steps equals the longest path from the root to a leaf.
* **Space Complexity:** `O(H)` (or `O(log n)` balanced / `O(n)` worst case).
  * **Why:** Each recursive call adds a new frame to the call stack until the target is found or `null` is reached.

## 4. Reflection / Improvement
* **Is there a more efficient approach?** In terms of time complexity, `O(H)` is already optimal because we must traverse down the height of the tree to reach the node. However, we can improve the **space complexity**.
* **What would you need to change?** Convert the recursive function into an iterative `while (root != null)` loop that updates `root = root.left` or `root = root.right`.
* **Improved Complexity:** Time complexity remains `O(H)`, while space complexity improves to **`O(1)`** because we no longer use the recursion call stack.
