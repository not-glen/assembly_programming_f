# ADD / ADC

## add1: 8-bit addition: 120 + 10

The program adds `num1` and `num2`.

```text
0111 1000   (120)
0000 1010   (10)
-----------
1000 0010   (0x82)
```

The result stored in `AL` is `82h`. As an unsigned value this is 130, while as a signed 8-bit value it represents -126.

### Flags

| Flag | Value | Explanation                                                                                                                         |
| ---- | ----: | ----------------------------------------------------------------------------------------------------------------------------------- |
| CF   |     0 | The unsigned result is 130, which still fits in 8 bits. There is no carry out of bit 7.                                             |
| OF   |     1 | Both numbers are positive when treated as signed values, but 120 + 10 = 130, which is greater than the signed 8-bit maximum of 127. |
| SF   |     1 | The most significant bit of `10000010` is 1.                                                                                        |
| ZF   |     0 | The result is not zero.                                                                                                             |
| PF   |     1 | The low byte contains two 1-bits, which is an even number.                                                                          |
| AF   |     1 | There is a carry from the lower 4 bits during the addition.                                                                         |

The important thing here is that **CF and OF are not checking the same type of overflow**. CF is about the unsigned result, while OF is about the signed result.

---

## add2: 16-bit addition: 32000 + 500

```text
0x7D00   (32000)
0x01F4   (500)
----------------
0x7EF4   (32500)
```

The final value is `7EF4h`.

### Flags

| Flag | Value | Explanation                                                                      |
| ---- | ----: | -------------------------------------------------------------------------------- |
| CF   |     0 | 32500 fits within the unsigned 16-bit range, so there is no carry out of bit 15. |
| OF   |     0 | 32500 is within the signed 16-bit range, so there is no signed overflow.         |
| SF   |     0 | Bit 15 of the result is 0.                                                       |
| ZF   |     0 | The result is not zero.                                                          |
| PF   |     0 | The low byte is `F4h` (`11110100`), which contains five 1-bits.                  |
| AF   |     0 | There is no carry out of bit 3.                                                  |

---

## add3: 16-bit ADD followed by ADC

The first operation adds `FFFFh` and `1`.

```text
1111 1111 1111 1111
0000 0000 0000 0001
-------------------
1 0000 0000 0000 0000
```

Only the lower 16 bits can remain in `AX`, so:

```text
AX = 0000h
CF = 1
```

### After ADD

| Flag | Value | Explanation                                                        |
| ---- | ----: | ------------------------------------------------------------------ |
| CF   |     1 | The result requires a 17th bit, so there is a carry out of bit 15. |
| OF   |     0 | There is no signed overflow for this addition.                     |
| SF   |     0 | Bit 15 of `AX` is 0.                                               |
| ZF   |     1 | `AX` becomes `0000h`.                                              |
| PF   |     1 | The low byte is `00h`, which has zero 1-bits. Zero is even.        |
| AF   |     1 | The addition produces a carry from bit 3.                          |

The program then executes:

```asm
ADC AX, 0
```

`ADC` includes the carry from the previous addition, so the calculation is:

```text
0 + 0 + 1 = 1
```

Therefore:

```text
AX = 0001h
```

### After ADC

| Flag | Value | Explanation                                            |
| ---- | ----: | ------------------------------------------------------ |
| CF   |     0 | The result `1` does not produce a carry out of bit 15. |
| OF   |     0 | The result is a valid small positive value.            |
| SF   |     0 | Bit 15 is 0.                                           |
| ZF   |     0 | The result is 1, not zero.                             |
| PF   |     0 | `01h` has one 1-bit, which is odd.                     |
| AF   |     0 | There is no carry from bit 3.                          |

**Note:** The flags need to be checked immediately after `ADD` or `ADC`. The instructions used to exit the program can change the flags, so checking them afterwards would not necessarily show the flags from the addition.
