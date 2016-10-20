# Inline Assembly Support

Sulong has limited support for AMD 64 inline assembly. Inline assembly
allows programmers to directly use machine-specific assembler instructions
in C and C++. For example, consider the following program:

```c
int main() {
    int arg1 = 40;
    int arg2 = 2;
    int add = 0;
    __asm__ ( "addl %%ebx, %%eax;"
        : "=a" (add)
        : "a" (arg1), "b" (arg2) );
    return add;
}
```

The `__asm__` keyword specifies the inline assembly snippet, which has
as its arguments a list of output operands (`add`), an optional list of
input operands (`arg1` and `arg2`), and an optional list of clobbered
registers. There is another inline assembly format which enables
`goto`-style jumps, which is not supported by Sulong.

You can compile the above program to LLVM IR and execute it with Sulong.
To support inline assembly, Sulong constructs an AST from the instructions.

## Implementation

The `com.oracle.truffle.llvm.asm.amd64` package contains an attributed
grammer and parser generated by the [Compiler Generator Coco/R](http://www.ssw.uni-linz.ac.at/Coco/).
We use this parser to parse the inline assembler snippet.

## Restrictions

Currently, Sulong only supports double word instructions (`l` suffix) and
can only read 32 bits from registers. The following instructions are
supported:

| instruction | comments |
|-------------|----------|
| addl        |          |
| subl        |          |
| andl        |          |
| orl         |          |
| xorl        |          |
| sall        |          |
| sarl        |          |
| shll        |          |
| shrl        |          |
| movl        |          |
| notl        |          |
| decl        |          |
| incl        |          |
| imull       |          |