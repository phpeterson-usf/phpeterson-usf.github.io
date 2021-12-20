---
layout: assignment
title: Lab01
due_date: "2022-01-31 23:59:59 -0800"
github: "https://www.cs.usfca.edu"
---

## Requirements
You will implement a base conversion tool called `nt`. It converts numbers expressed in bases 2, 10, and 16 into the other two bases. Examples:

```
$ ./nt 10 -o 2
0b1010
$ ./nt 0xFF -o 10
255
$ ./nt 0b11011110101011011011111011101111 -o 16
0xDEADBEEF
$ ./nt 0b11111111111111111111111111111111 -o 10
4294967295
$ ./nt 0x0000000B -o 10
11
$ ./nt 0b123 -o 2
Bad input
```

You must implement the base conversions yourself, without using C library functions such as: `printf()`,  `scanf()`, or `atoi()`

## Given
1. We will review processing command line arguments in C
1. Pseudocode for `uint32_t string_to_int(char *str)`
    ```
    init retval to 0
    init placeval to 1 (anything to the 0 power is 1)
    loop over str from the highest index down to 0
        calculate the integer corresponding to the character at that index
        calculate the value of that place by multiplying the integer * placeval
        add the value to the retval
        update to placeval to placeval * base
    return the return value
    ```
1. Pseudocode for `void int_to_string(uint32_t value, char *str, int base)`
    ```
    init buffer to empty
    while value != 0
        quot = value / base
        rem = value % base
        calculate the character value of rem
        append the character value to the buffer
        value = quot
    copy buffer into str in reverse order
    ```

## Rubric
- For a Check-, you will correctly scan the input parameters and implement `string_to_int()`
- For a Check, you will also implement: `void int_to_string(uint32_t value, char *str, int base)` where `base` is 2
- For a Check+, you will also implement the general form: `void int_to_string(uint32_t value, char *str, int base)` where `base` may be 2, 10, or 16