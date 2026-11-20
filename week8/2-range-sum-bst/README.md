# 2. Range Sum of BST

## 1. Problem
Given the root of a Binary Search Tree and two integers `low` and `high`, we need to return the sum of all node values that fall within the inclusive range `[low, high]`.

## 2. Approach
My approach uses Depth-First Search (DFS) recursion combined with pruning based on the BST rules:
1. If the current node is `null`, return `0`.
2. If `root.val < low`, every node in the left subtree will be even smaller than `low`, so we prune (skip) the left side and only search `root.right`.
3. If `root.val > high`, every node in the right subtree will be even larger than `high`, so we prune the right side and only search `root.left`.
4. If `root.val` is within `[low, high]`, we add `root.val` to the sum of valid nodes from both the left and right subtrees.

### Written Trace
Suppose the tree has root `10`, left child `5` (with children `3` and `7`), and right child `15` (with right child `18`). Range is `low = 7, high = 15`:
* **Step 1:** At `10`. `10` is inside `[7, 15]`. Keep `10` and check both left (`5`) and right (`15`).
* **Step 2 (Left branch):** At `5`. Since `5 < 7`, skip its left child (`3`) and only check right child (`7`).
* **Step 3:** At `7`. `7` is inside `[7, 15]`. Keep `7`. Its children are `null`, returning `0`.
* **Step 4 (Right branch):** At `15`. `15` is inside `[7, 15]`. Keep `15` and check both sides. Left is `null` (`0`); right is `18`.
* **Step 5:** At `18`. Since `18 > 15`, skip its right side and check left (`null`, returns `0`).
* **Final Result:** `10 + 7 + 15 = 32`.

### Challenges Faced
My first idea was to visit every single node in the tree and check `if (root.val >= low && root.val <= high)` before adding it to a global sum variable. While that passed basic tests, I realized it did not take advantage of the BST structure at all—for example, it still explored `3` and `18` even though we already knew at `5` and `15` that those branches were out of bounds. Adding the `< low` and `> high` checks fixed this.

## 3. Complexity Analysis
* **Time Complexity:** `O(n)`, where `n` is the total number of nodes in the tree.
  * **Why:** Even though pruning skips out-of-range branches in average cases, in the worst case the range `[low, high]` covers every node in the tree, forcing the algorithm to visit all `n` nodes once.
* **Space Complexity:** `O(H)`, where `H` is the height of the tree (`O(log n)` for a balanced tree, `O(n)` for a skewed tree).
  * **Why:** The recursion stack holds at most `H` active function calls at any moment.

## 4. Reflection / Improvement
* **Is there a more efficient approach?** Asymptotically, `O(n)` is the best possible worst-case time complexity because if all `n` nodes fall inside `[low, high]`, every node must be visited to compute the sum.
* **What would you need to change?** To avoid recursion stack overhead (and prevent stack overflow on very deep trees), we could switch to an iterative approach using an explicit `Stack` or `Queue` (BFS) that only pushes child nodes when they are within bounds.
* **Improved Complexity:** Time complexity stays `O(n)` and auxiliary space remains `O(H)` (or `O(W)` for tree width in BFS), but it eliminates recursion call-stack limits.
