## Implement an algorithm to find the k<sup>th</sup> to last element of a singly linked list
#### Author: James Proulx

Here is an example linked list:

[head] -> [ex] -> [ex] -> ... -> [3rd to last] -> [2nd to last] -> [1st to last] -> null

Assume k > 0, and the length of the list > k

Lets the very simple implementation of a Linked List, provided by the book:

```java
Class Node {
  Node next = null;
  int data;
  
  public Node(int d) {
    data = d;
  }
  
  void addToTail(int d) {
    Node end = new Node(d);
    Node n = this;
    while (n.next != null) {
      n = n.next;
    }
    n.next = end;
  }
}
```


Note that there is no given method for removing a node, finding a node, or even to find the length of the list.
We'll have to do most of this ourselves.


One simple but not ideal solution could be to simply go through the linked list, counting the nodes, and then revisit the head,
traverse to the k<sup>th</sup> to last node, and return it. Let's implement this.

```java
private static Node removeKthToLast(Node head, int k) {
  int length = 1; //To count the head
  Node traverseNode = head;
  while (traverseNode.next != null) {
    traverseNode = traverseNode.next;
    length++;
  }
  
  //At this point, we have the length of the linked list, and can now travel the length - k - 1 
  //to arrive at the element before the return element
  traverseNode = head //reset traverse node
  for (int i = 0; i < length - k - 1; i++) {
    traverseNode = traverseNode.next;
  }
  
  //Now return the kth to last element
  return traverseNode.next;
}
```

This solution is correct, and runs in **O(n)**, but it requires iterating over the linked list twice. Can we prevent this?
My intuition is that we could have two traversing nodes, one k elements behind the other.

This way, when the first traverse node arrives at the end of the list, the slower node is already at the node we want to focus on.
Let's try an example. For the second to last node, when our first traverse node arrives at the end, the second traverse node
is at the third to last element, ready to return the 2nd to last. We don't need to count the length at all!

Let's try it.

```java
private static void removeKthToLast(Node head, int k) {
  
  Node traverseNode = head;
  Node slowTraverseNode = head;
  
  for (int i = 0; i < k; i++) {
    traverseNode = traverseNode.next;
  }
  
  //Now, the traverse node is k elements ahead of the slow traverse node. Continue so the traverse node finds the end.
  
  while (traverseNode.next != null) {
    traverseNode = traverseNode.next;
    slowTraverseNode = slowTraverseNode.next;
  }
  
  //Now return kth to last element
  return slowTraverseNode.next;
}
```

This solution runs in **O(n)**, and only iterates over the list once. Excellent!
---------------------------------------------------------------------------------
At this point, I am going to check the book to see the official solution, and see if any improvements can be made.

The book details a few solutions, one recurisve and one interative.
The recursive solution given is as follows
