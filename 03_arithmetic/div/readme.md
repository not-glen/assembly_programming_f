# DIV

`DIV` performs unsigned division.

Unlike `ADD`, `SUB`, and `MUL`, the arithmetic flags do not have defined values after a `DIV` instruction. Therefore, `CF`, `OF`, `SF`, `ZF`, `AF`, and `PF` are all undefined.

The main things to check are the quotient and remainder.

## div1.asm

The program divides 100 by 7.

100 ÷ 7 = 14 remainder 2

For an 8-bit divisor, the quotient is stored in `AL` and the remainder is stored in `AH`.

`AL = 0Eh`

`AH = 02h`

Therefore:

- Quotient = 14
- Remainder = 2

### EFLAGS

The arithmetic flags are undefined after `DIV`:

- **CF = undefined**
- **OF = undefined**
- **SF = undefined**
- **ZF = undefined**
- **AF = undefined**
- **PF = undefined**

---

## div2.asm

The program divides 50000 by 300.

50000 ÷ 300 = 166 remainder 200

The result is stored as:

`AX = 00A6h`

`DX = 00C8h`

Therefore:

- Quotient = 166
- Remainder = 200

### EFLAGS

The arithmetic flags are undefined after `DIV`:

- **CF = undefined**
- **OF = undefined**
- **SF = undefined**
- **ZF = undefined**
- **AF = undefined**
- **PF = undefined**

---

## div3.asm

The program divides 300000000 by 1000.

300000000 ÷ 1000 = 300000

There is no remainder.

The result is stored as:

`EAX = 000493E0h`

`EDX = 00000000h`

Therefore:

- Quotient = 300000
- Remainder = 0

### EFLAGS

The arithmetic flags are undefined after `DIV`:

- **CF = undefined**
- **OF = undefined**
- **SF = undefined**
- **ZF = undefined**
- **AF = undefined**
- **PF = undefined**