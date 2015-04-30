---
layout: post
title:  "Merging Linked Lists"
date:   2015-04-30 00:00:00
categories: linked_lists
---

Merging is a basic buiding block for many other algorithms.  For the case of
linked lists, the statement of the problem is:

_`Given two sorted linked lists lst1 and lst2, return a new sorted linked list
lst0 that contains all elements in lst1 and lst2.  You may destroy lst1 and
lst2 in the process of building lst0.`_

For example, if `lst1` contains `(4 8 20 32 41)` and `lst2` contains `(1 10 15
20 25)`, `lst0` should contain `(1 4 8 10 15 20 20 25 32 41)`.  Notice that
repeated elements must appear multiple times in the answer, such that
`lst0.size() == lst1.size() + lst2.size()`.

No matter what type of collection is being processed, merging sorted
collections in general should be an O(n) algorithm; this takes advantage of
the order in the input collections.  If the inputs are unsorted, merging
becomes O(n*log(n)), which is equivalent to first sorting the input
collections and then merging them (although there are better ways to proceed
in this case).

_`Stop reading now if you want to try your hand at the problem.`_

Here is a merge algorithm that iterates simultaneously over the two input
lists and chooses the appropriate element on each step:

{% highlight c++ linenos=table %}
void List::merge(List* lst1,
                 List* lst2)
{
  // Two pointers to keep track of the answer
  Node* first = 0;
  Node* last = 0;

  // Two pointers to keep track of where we are in the iteration
  Node* p1 = lst1->beg();
  Node* p2 = lst2->beg();

  // Here we go:
  while (1) {
    int take_from_lst1 = 1;
    if (p1 == lst1->end() && p2 == lst2->end()) {
      // Both lists consumed, we are done
      break;
    } else if (p1 == lst1->end()) {
      // List1 consumed, take from list2
      take_from_lst1 = 0;
    } else if (p2 == lst2->end()) {
      // List2 consumed, take from list1
      take_from_lst1 = 1;
    } else {
      // Both lists have elements, must choose least value
      if (p1->value() <= p2->value()) {
        // Value in list1 is smaller or equal, use that
        take_from_lst1 = 1;
      } else {
        // Value in list2 is smaller, use that
        take_from_lst1 = 0;
      }
    }

    // Stitch right element into answer
    Node*& p = take_from_lst1 ? p1 : p2;
    if (last == 0) {
      // This is the first node we add to the answer
      first = last = p;
    } else {
      // Link this node to the last node added to the answer
      last->join_to(p);
      last = p;
    }

    // Whatever list we consumed from, move it to the next element
    p = p->next();
  }

  // first now points to the beginning of the answer;
  // last points to the final node in the answer.
  return first;
}
{% endhighlight %}

If you read the [previous post on splitting a linked list][1], you will notice
this merging algorithm does not comply with the recommendation of using an
open-ended right side to represent the linked list.  The reason for this is
that we wish to use the two building blocks we have reviewed, splitting and
merging, to implement a well-behaved sorting algorithm on linked lists; this
requires that the merging algorithm returns the actual first and last nodes in
the answer, for reasons that will be made clear in the next post.

[1]: {% post_url 2015-04-26-linked-list-splitting %}
