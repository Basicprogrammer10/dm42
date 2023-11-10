# dm42

Programs for the [Swissmicros DM42](https://www.swissmicros.com/product/dm42) and a preprocessor for creating them.

## Example

This code that makes uses of function definitions, if statements and while loops is compiled down into plain hp 42s instructions.

```cpp
export def sort {
    if { MAT? } else {
        "X is not a matrix", AVIEW
        STOP
    }

    STO "A"
    INDEX "A"
    1, 1, STOIJ
    DROPN 2

    while { FC? 76 } {
        [MIN]
        DROP
        RCLIJ
        DROP
        R<>R
        DROPN 2
        I+
    }

    RCL "A"
}
```

## Documentation

This is for when I inevitability forget all of this syntax that I totally put so much thought into.
Anyway, there are currently four things added by this transpiler / preprocessor, Functions, Function calls, If statements, and While (or Do-while) loops.
Below are some examples of each.

```cpp
// Zeros out the X reg by repeatedly subtracting 1
// Because the function is being exported, it will be globally accessibly (in the EXQ menu)
export def zero {
    // Each block here must return one value by putting it in X
    // The value will be dropped before any other user code is run
    // To use a do-while loop, one must simple replace "while" with "do while"
    while { DUP } > { 0 } {
        // This expands to "1\n"
        1, -
    }
}

export def cmat {
    // This kind of condition without the comparison lets you bring your own instruction to selectively execute the following instruction
    // Also notice the missing then case, this was done to basically invert the result of MAT?
    if { MAT? } else {
        "X is not a matrix", AVIEW
        // Calls the "zero" function from above
        zero()
        STOP
    }
}
```

## Todo

- [x] Finish parser / tokenizer
  - [x] Base
  - [x] Functions
  - [x] If
  - [x] Raw-If
  - [x] While
  - [x] Do While
- [x] Finish codegen
  - [x] Functions
  - [x] If-statements
  - [x] While loops
  - [x] Do While
- [ ] Future Ideas
  - [ ] For loops
  - [ ] Decent error reporting
  - [ ] Inline functions
  - [ ] vscode lang grammer
  - [ ] Basic documentation
  - [ ] Macros
  - [ ] Imports
  - [x] CLI
  - [ ] Automatically cut strings
  - [ ] Figure out reusing functions across exported functions
  - [x] Comments
  - [ ] Anonyms functions