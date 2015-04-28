---
layout: post
title:  "Splitting a Linked List"
date:   2015-04-26 00:00:00
categories: linked_lists
---

[Linked lists][1] are very flexible but require some finesse when handling
certain requirements.  Here is a classic problem:

_`Find the median element in a singly-linked list.`_

The median element divides the list in two lists, each having half the
elements in the original (correcting for rounding when there are an odd number
of elements to begin with).  At first sight, this would seem to require
iterating over the list to count the total number of elements in it;
thereafter, you would divide that count by 2; finally, you would iterate again
until hitting the median.

The problem we are trying to solve is: how can you do this without counting
the number of elements in the list?

_`Stop reading now if you want to try your hand at the problem.`_

The trick to doing this is to use two pointers to the list, `p` and `q`.  Both
start iterating at the beginning of the list, but while `p` moves one place
forward in each step of the iteration, `q` only moves forward every other step
of the iteration.  In other words, `q` moves at _half the speed_ of `p`.
Given this, when `p` reaches the end of the list, `q` is positioned where we
want it, right in the middle.

{% highlight c++ linenos=table %}
Node* List::split(int fraction) const
{
  Node* q = 0;
  int step = 0;
  for (Node* p = this->beg(); p != this->end(); p = p->next()) {
    if (++step == 2) {
      q = q ? q->next() : this->beg();
      step = 0;
    }
  }
  return q;
}
{% endhighlight %}

Why are we comparing `step` with `2` in line 7?  The median point is at 50% of
the list, so we only advance `q` every `100/50 = 2` steps of the iteration.
It becomes obvious, then, that this technique generalises to any other subset
of the list.  To find the element that is positioned at the 20% position in
the list, `q` would have to move every `100/20 = 5` steps of the iteration.
If we refer to this value of `2` (or `5`) as the `fraction` of the list, the
only change required in the code above would be changing the `2` with
`fraction`.

This is a neat trick to split a list.  Splitting is a fundamental part of some
fundamentl algorithms, most notably [merge sort][2], a great sorting algorithm
guaranteed to always run in O(n*log(n)) time, that can be used even with
linked lists, as we will see in other posts.

As a closing remark, notice the list iteration in the code snippet uses
`this->beg()` and `this->end()` to refer to the beginning and end of the list,
and that `this->end()` actually refers to one element past the real tail of
the list.  This follows the C++ iterator convention and has many advantages,
one of them being that it simplifies the code significantly.  When a range of
any kind is required (dates, temperatures, iterators, etc.), it is almost
always best to represent that range with an interval that is closed on the
left side and open on the right side.

[1]: http://en.wikipedia.org/wiki/Linked_list
[2]: http://en.wikipedia.org/wiki/Merge_sort
