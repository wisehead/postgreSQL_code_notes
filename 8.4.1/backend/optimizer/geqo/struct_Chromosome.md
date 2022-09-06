#1.struct Chromosome

```cpp
/* we presume that int instead of Relid
   is o.k. for Gene; so don't change it! */
typedef int Gene;

typedef struct Chromosome
{
    Gene       *string;
    Cost        worth;
} Chromosome;

```