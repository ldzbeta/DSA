## ❌ 1. Deleting forward changes heap structure

When you delete from smaller index to larger index, each del() replaces arr[idx] with the last element, and decreases h->size.
So the indices of the next elements shift — meaning your loop starts deleting wrong elements or goes out of bounds.

→ You must delete from end → start for correctness.
- done for deleting elements in a  row
  
