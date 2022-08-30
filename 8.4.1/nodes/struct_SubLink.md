#1.struct SubLink

```cpp
typedef struct SubLink
{
    Expr        xpr;
    SubLinkType subLinkType;    /* see above */
    Node       *testexpr;       /* outer-query test for ALL/ANY/ROWCOMPARE */
    List       *operName;       /* originally specified operator name */
    Node       *subselect;      /* subselect as Query* or parsetree */
    int         location;       /* token location, or -1 if unknown */
} SubLink;

```