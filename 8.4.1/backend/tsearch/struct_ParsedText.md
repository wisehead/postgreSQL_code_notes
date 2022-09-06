#1.struct ParsedText

```cpp
typedef struct
{
    ParsedWord *words;
    int4        lenwords;
    int4        curwords;
    int4        pos;
} ParsedText;
```