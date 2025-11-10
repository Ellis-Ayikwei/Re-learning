# Binary Search Trees

## Overview

A Binary Search Tree (BST) is a hierarchical data structure where each node has at most two children (left and right). It maintains a specific ordering property that makes searching efficient.

## BST Property

For every node in the tree:
- All values in the **left subtree** are **less than** the node's value
- All values in the **right subtree** are **greater than** the node's value
- Both left and right subtrees are also BSTs

## Structure

```
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13
```

## Time Complexity

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Search    | O(log n)| O(n)       |
| Insert    | O(log n)| O(n)       |
| Delete    | O(log n)| O(n)       |
| Space     | O(n)    | O(n)       |

**Note:** Worst case occurs when tree becomes skewed (essentially a linked list)

## Operations

### 1. Search

Start at root and recursively:
1. If target equals current node value, found!
2. If target < current value, search left subtree
3. If target > current value, search right subtree
4. If reach null, value doesn't exist

### 2. Insert

Similar to search:
1. Start at root
2. Compare new value with current node
3. Go left or right based on comparison
4. Insert at first null position found

### 3. Delete

Three cases:
1. **Node has no children**: Simply remove it
2. **Node has one child**: Replace node with its child
3. **Node has two children**: 
   - Find inorder successor (smallest value in right subtree)
   - Replace node's value with successor's value
   - Delete the successor

### 4. Traversal

**Inorder (Left-Root-Right)**:
- Produces sorted sequence
- Example: 1, 3, 4, 6, 7, 8, 10, 13, 14

**Preorder (Root-Left-Right)**:
- Used for creating copy of tree
- Example: 8, 3, 1, 6, 4, 7, 10, 14, 13

**Postorder (Left-Right-Root)**:
- Used for deleting tree
- Example: 1, 4, 7, 6, 3, 13, 14, 10, 8

## Python Implementation

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None
    
    def insert(self, value):
        """Insert a value into the BST"""
        if self.root is None:
            self.root = TreeNode(value)
        else:
            self._insert_recursive(self.root, value)
    
    def _insert_recursive(self, node, value):
        if value < node.value:
            if node.left is None:
                node.left = TreeNode(value)
            else:
                self._insert_recursive(node.left, value)
        else:
            if node.right is None:
                node.right = TreeNode(value)
            else:
                self._insert_recursive(node.right, value)
    
    def search(self, value):
        """Search for a value in the BST"""
        return self._search_recursive(self.root, value)
    
    def _search_recursive(self, node, value):
        if node is None or node.value == value:
            return node
        
        if value < node.value:
            return self._search_recursive(node.left, value)
        return self._search_recursive(node.right, value)
    
    def delete(self, value):
        """Delete a value from the BST"""
        self.root = self._delete_recursive(self.root, value)
    
    def _delete_recursive(self, node, value):
        if node is None:
            return node
        
        # Find the node to delete
        if value < node.value:
            node.left = self._delete_recursive(node.left, value)
        elif value > node.value:
            node.right = self._delete_recursive(node.right, value)
        else:
            # Node found - handle three cases
            
            # Case 1: No children or only right child
            if node.left is None:
                return node.right
            
            # Case 2: Only left child
            if node.right is None:
                return node.left
            
            # Case 3: Two children
            # Find inorder successor (smallest in right subtree)
            successor = self._find_min(node.right)
            node.value = successor.value
            node.right = self._delete_recursive(node.right, successor.value)
        
        return node
    
    def _find_min(self, node):
        """Find minimum value node in subtree"""
        current = node
        while current.left is not None:
            current = current.left
        return current
    
    def inorder(self):
        """Inorder traversal (returns sorted list)"""
        result = []
        self._inorder_recursive(self.root, result)
        return result
    
    def _inorder_recursive(self, node, result):
        if node:
            self._inorder_recursive(node.left, result)
            result.append(node.value)
            self._inorder_recursive(node.right, result)
    
    def find_min(self):
        """Find minimum value in tree"""
        if self.root is None:
            return None
        return self._find_min(self.root).value
    
    def find_max(self):
        """Find maximum value in tree"""
        if self.root is None:
            return None
        current = self.root
        while current.right is not None:
            current = current.right
        return current.value
    
    def height(self):
        """Calculate height of tree"""
        return self._height_recursive(self.root)
    
    def _height_recursive(self, node):
        if node is None:
            return -1
        return 1 + max(self._height_recursive(node.left),
                      self._height_recursive(node.right))

# Example usage
if __name__ == "__main__":
    bst = BinarySearchTree()
    
    # Insert values
    values = [8, 3, 10, 1, 6, 14, 4, 7, 13]
    for val in values:
        bst.insert(val)
    
    # Search
    print("Search 6:", bst.search(6) is not None)
    print("Search 15:", bst.search(15) is not None)
    
    # Traversal
    print("Inorder:", bst.inorder())
    
    # Min/Max
    print("Min:", bst.find_min())
    print("Max:", bst.find_max())
    
    # Height
    print("Height:", bst.height())
    
    # Delete
    bst.delete(3)
    print("After deleting 3:", bst.inorder())
```

## Advantages

1. **Efficient searching**: O(log n) on average
2. **Ordered data**: Inorder traversal gives sorted sequence
3. **Dynamic**: Easy to insert and delete
4. **No size limitations**: Grows dynamically

## Disadvantages

1. **Can become unbalanced**: Leading to O(n) operations
2. **No random access**: Unlike arrays
3. **Extra memory**: For storing pointers

## Balanced BSTs

To maintain O(log n) performance:

### AVL Trees
- Height-balanced BST
- Balance factor: |height(left) - height(right)| ≤ 1
- Automatic rebalancing via rotations

### Red-Black Trees
- Self-balancing with color properties
- Guarantees O(log n) in worst case
- Used in many standard libraries

### B-Trees
- Generalization of BST
- Multiple keys per node
- Used in databases and filesystems

## Applications

1. **Databases**: Indexing and searching
2. **File systems**: Directory structures
3. **Symbol tables**: In compilers
4. **Priority queues**: Implementing heaps
5. **Auto-complete**: Trie (special BST variant)

## Practice Problems

1. Validate if a tree is a BST
2. Find kth smallest element
3. Lowest common ancestor
4. Convert sorted array to BST
5. Serialize and deserialize BST

## References

- Introduction to Algorithms (CLRS)
- [GeeksforGeeks BST](https://www.geeksforgeeks.org/binary-search-tree-data-structure/)
- [Visualgo - BST Visualization](https://visualgo.net/en/bst)
