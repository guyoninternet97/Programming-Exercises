## Implement an algorithm to find the k<sup>th</sup> to last element of a singly linked list
#### Author: James Proulx

To detail the problem further, if I remove the 1st to last element, I simply snip off the end of the list.
If I remove the 2nd to last, I remove the element such that there is one element between the end of the list and itself.
And so on

[head] -> [ex] -> [ex] -> ... -> [3rd to last] -> [2nd to last] -> [1st to last] -> null

Assume k > 0

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
traverse to the k<sup>th</sup> to last node, and remove it. Let's implement this.

```java
private static void removeKthToLast(Node head, int k) {
  int length = 1; //To count the head
  Node traverseNode = head;
  while (traverseNode.next != null) {
    traverseNode = traverseNode.next;
    length++;
  }
  
  //At this point, we have the length of the linked list, and can now travel the length - k - 1 
  //to arrive at the element before the element to remove
  traverseNode = head //reset traverse node
  for (int i = 0; i < length - k - 1; i++) {
    traverseNode = traverseNode.next;
  }
  
  //Now remove kth to last element
  traverseNode.next = traverseNode.next.next; //Skip over the kth to last element
}
```

This solution is correct, and runs in **O(n)**, but it requires iterating over the linked list twice. Can we prevent this?
My intuition is that we could have two traversing nodes, one k elements behind the other.

This way, when the first traverse node arrives at the end of the list, the slower node is already at the node we want to focus on.
Let's try an example. For the second to last node, when our first traverse node arrives at the end, the second traverse node
is at the third to last element, ready to remove the 2nd to last.

Let's try it.
