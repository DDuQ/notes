# Definition

B-Tree is short for **B**alanced **Tree**.

This is a *self-balancing search tree* designed to keep data sorted and ==allow fast searches, inserts, deletions, and range queries== in logarithmic time.

![[B-Tree-1.png]]
# Key parts
## Balanced Height
 All leaf nodes are at the same level.
## Multiple keys per node
 Unlike binary trees, B-tree can have many keys.
## Ordered structure
Keys are stored in sorted order.
## Disk/page friendly
It's optimized so that each node first neatly in a **database page**; minimizing I/O operations -- crucial for database performance.


