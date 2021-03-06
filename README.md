## Deepest Binary Tree Node

---

## Interviewer Prompt

Given a binary tree, write a function that will return the node at the deepest level of the tree. You may assume there is a unique deepest node.

---

## Setup and Example

```javascript
function node(val) {
  return {
    val,
    left: null,
    right: null
  };
}
let a = node('a');
let b = node('b');
let c = node('c');
let d = node('d');
let e = node('e');
let f = node('f');

a.left = b;
a.right = c;
b.right = d;
d.left = f;
c.left = e;

findDeepest(a) //Result: f
```

---

## Interviewer Guide

---

### RE-

- The interviewee should be aware of the structure of a node (i.e. it has value, left and right properties). Encourage them to write the 'node' function themselves.

- Be sure to have your interviewee sketch an example tree.

- You may need to remind your interviewee of what depth means in a tree (the root has depth 0 and each node's depth is its parent's depth + 1)

---

### -AC-

- You may want to remind your interviewee that many tree problems have a depth-first and a breadth-first solution

- Remind them that each child of a tree node is its own tree (will be handy for the depth-first approach)

---

### -TO

  - If your interviewee finishes, ask them:
  - What is the Big O of their approach?
  - If they found the depth-first solution, suggest they look for a breadth-first solution, or vice-versa

---

## Solution Code (Depth First)

```javascript
const findDeepestDFS = (node) => {
  let deepestNode = node
  let deepestLevel = 0
  // find is a helper function which we'll recurse over
  const find = (node, level = 0) => {
      // if this node's level is deeper than the current "deepestLevel", replace it
      if (level > deepestLevel) {
        deepestNode = node;
        deepestLevel = level;
      }
      // recursive cases: if node.left and/or node.left exist, call the function on each, increasing the level by 1
      if (node.left) {
        find(node.left, level + 1);
      }
      if (node.right) {
        find(node.right, level + 1);
      }
      // the base case is implicit here: if there's no node.left or node.right, the function execution ends
  }
  find(node)
  return deepestNode;
};
```

---

## Solution Code (Breadth First)

```javascript
const findDeepestBFS = (node) => {
  // use a queue to iterate over the tree
  let queue = [node]
  let current;
  while (queue.length > 0) {
    current = queue.shift()
    if (current.left) queue.push(current.left)
    if (current.right) queue.push(current.right)
  }
  // when we exit the while loop, it means we've seen every node in breadth-first order
  // current will be the last node we saw, which will necessarily be the deepest node in the tree
  return current;
}
```

---

## Time and Space Complexity

#### Big O for Time

In both solutions, you must visit every node in the tree.
- Depth First: O(n)
- Breadth First: O(n)

#### Big O for Space

- Depth First: O(max tree depth) - the worst case for the call stack will be the maximum number of levels (maximum depth) of the tree
- Breadth First: O(n) - the worst case will be a fully balanced tree, in which case we need to store (close to) all of the nodes in our queue
