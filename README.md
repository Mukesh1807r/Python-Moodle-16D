# 🌳 Inserting Elements in a B+ Tree in Python

## 📌 Objective
To define and implement the `insert(self, key, value)` function that inserts key-value pairs into a **B+ Tree**, and demonstrate the structure before and after split operations.

---

## 🧱 Components

### 🔹 `Node` Class
Represents a single node in a B+ Tree.
- **Attributes:**
  - `keys`: Holds keys in sorted order.
  - `values`: Values associated with keys (or children if internal node).
  - `leaf`: Boolean to determine if it's a leaf.
- **Methods:**
  - `add(key, value)`: Adds a key-value pair to the node.
  - `split()`: Splits the node when it’s full.
  - `is_full()`: Returns `True` if the node is full.
  - `show(counter=0)`: Displays the node and its children.

### 🔹 `BPlusTree` Class
Encapsulates the B+ Tree behavior.
- **Methods:**
  - `insert(key, value)`: Inserts a key-value pair into the tree.
  - `_find(node, key)`: Finds the child node where the key should be inserted.
  - `_merge(parent, child, index)`: Merges a newly split child back into the parent.
  - `retrieve(key)`: Finds the values for a given key.
  - `show()`: Displays the tree structure.

---

## 💻 Program Code

```python
class Node(object):
    def __init__(self, order):
        self.order = order
        self.keys = []
        self.values = []
        self.leaf = True

    def add(self, key, value):
        if not self.keys:
            self.keys.append(key)
            self.values.append([value])
            return None
        for i, item in enumerate(self.keys):
            if key == item:
                self.values[i].append(value)
                break
            elif key < item:
                self.keys = self.keys[:i] + [key] + self.keys[i:]
                self.values = self.values[:i] + [[value]] + self.values[i:]
                break
            elif i + 1 == len(self.keys):
                self.keys.append(key)
                self.values.append([value])

    def split(self):
        left = Node(self.order)
        right = Node(self.order)
        mid = self.order // 2
        left.keys = self.keys[:mid]
        left.values = self.values[:mid]
        right.keys = self.keys[mid:]
        right.values = self.values[mid:]
        self.keys = [right.keys[0]]
        self.values = [left, right]
        self.leaf = False

    def is_full(self):
        return len(self.keys) == self.order

    def show(self, counter=0):
        print(counter, str(self.keys))
        if not self.leaf:
            for item in self.values:
                item.show(counter + 1)

class BPlusTree(object):
    def __init__(self, order=8):
        self.root = Node(order)

    def _find(self, node, key):
        for i, item in enumerate(node.keys):
            if key < item:
                return node.values[i], i
        return node.values[i + 1], i + 1

    def _merge(self, parent, child, index):
        parent.values.pop(index)
        pivot = child.keys[0]
        for i, item in enumerate(parent.keys):
            if pivot < item:
                parent.keys = parent.keys[:i] + [pivot] + parent.keys[i:]
                parent.values = parent.values[:i] + child.values + parent.values[i:]
                break
            elif i + 1 == len(parent.keys):
                parent.keys += [pivot]
                parent.values += child.values
                break

    def insert(self, key, value):
        parent = None
```
---
## Output 

![image](https://github.com/user-attachments/assets/818fe89a-b279-4794-8018-350dd4aa25ae)

---
## Result 
Successfully inserted elements and balanced a B+ Tree using a custom insert() method!


