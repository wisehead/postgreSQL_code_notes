#1.pull_up_sublinks_jointree_recurse

```cpp
/*
 * Recurse through jointree nodes for pull_up_sublinks()
 *
 * In addition to returning the possibly-modified jointree node, we return
 * a relids set of the contained rels into *relids.
 */
static Node *
pull_up_sublinks_jointree_recurse(PlannerInfo *root, Node *jtnode,
                                  Relids *relids)

```