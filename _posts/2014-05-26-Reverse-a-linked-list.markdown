---
title: "Reverse a singly linked list"
layout: post
date: 2014-05-26 21:01:58
comments: true
categories: [web development]
---

I came across this interesting programming question today and decided to give it a go. Given a singly linked list, with reference to the head node, reverse the linked list so the head node becomes the last node and vice versa. 

This is my iterative solution:

    public Node reverse(Node head) {
        if (head == null || head.next == null) { // Linked list is empty or only has 1 node
            return;
        }
        Node current = new Node();
        Node next = new Node();
        Node prev = null;
        
        current = head; // current node starts at the first node, which is referenced by head. keep in mind that head acts like a pointer here instead of a real node
        while(current != null) { //run loop as long as current node is not the last node
            next = current.next; // stores a reference of the next node so we can manipulate current.next reference
            current.next = prev; // point the current.next to the new reference position -which is in reverse order
            prev = current; // prev has done its job of referencing the previous node, it now moves on to reference the subsequent node
            current = next; // current has referenced in the new order, move current pointer to the next node - which is referenced by next pointer.
        }
        head = prev; // the last node is now the new first node in the linked list, so we reference head to prev, which points to the node itself
        return head
    }

Now let's try the recursive solution:

### Identify base case

The base case occurs when we reach the last node of the linked list. We identify this case by verifying that current.next is NULL. 

### Base case operation

In the base case, we need to set the head pointer to the very last node, which will in turn become the first node in the result.

Here's my recursive answer:

    public Node reverse(Node current) {
        if (current == null) { //check for empty list
            return;
        }
        
        if (current.next == null) { // at tail node, this is recursive base case
            head = current; // set head to point to the last node in original list
            return;
        }
                
        reverse(current.next);
        current.next.next = current; // set the subsequent node as the current node
        current.next = null; // set the previous node to null. which eventually means the older next pointer is used and null
    }
    
This little devil took me quite awhile to figure out as it's very confusing between the reference pointers and the current nodes in question. But in practicality, i just can't imagine a scenario where i'll need to reverse a singly linked list. Can you? Whatever, this also proves that as a norm, we should just use a double linked list in the rare case we need to do a reversal like this. Time complexity of both of the solutions are O(n).

Edit: On a side note, we can also do a shallow copy of the linked list to an array-like data structure, use a reverse() function and push into a fresh linked list. Horrible on the space complexity, but alot easier to understand.