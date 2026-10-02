# MUL

`MUL` performs unsigned multiplication.

For `MUL`, the `CF` and `OF` flags show whether the upper half of the multiplication result is non-zero. The other arithmetic flags are undefined after the instruction.

## mul1.asm

The program multiplies 25 by 10.

25 × 10 = 250

250 is `00FAh`.

For an 8-bit multiplication, the full result is stored in `AX`.

`AH = 00h`

`AL = FAh`

Since `AH` is zero, the upper half of the result is zero.

### Flags

- **CF = 0**
- **OF = 0**

Both flags are cleared because the upper half of the result is zero.

The following flags are undefined after `MUL`:

- **SF**
- **ZF**
- **AF**
- **PF**

---

## mul2.asm

The program multiplies 3000 by 200.

3000 × 200 = 600000

The result is `000927C0h`.

For a 16-bit multiplication, the result is stored across `DX:AX`.

`DX = 0009h`

`AX = 27C0h`

Since `DX` is not zero, part of the result is stored in the upper half.

### Flags

- **CF = 1** — The upper half of the result is non-zero.
- **OF = 1** — The upper half of the result is non-zero.

The following flags are undefined after `MUL`:

- **SF**
- **ZF**
- **AF**
- **PF**

---

## mul3.asm

The program multiplies 100000 by 300000.

100000 × 300000 = 30,000,000,000

The full result is:

`00000006FC23AC00h`

The result is split between `EDX` and `EAX`.

`EDX = 00000006h`

`EAX = FC23AC00h`

Since `EDX` is not zero, the upper half of the result contains a value.

### Flags

- **CF = 1** — The upper half of the result is non-zero.
- **OF = 1** — The upper half of the result is non-zero.

The following flags are undefined after `MUL`:

- **SF**
- **ZF**
- **AF**
- **PF**