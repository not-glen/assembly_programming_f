# SUB / SBB

The flags should be checked immediately after the `SUB` or `SBB` instruction because later instructions can change their values.

## sub1.asm

The program subtracts 80 from 50.

50 - 80 = -30

Since the operation uses an 8-bit register, `-30` is represented using two's complement as `E2h`.

### Flags

- **CF = 1** — A borrow is needed because 50 is smaller than 80.
- **OF = 0** — The result can be represented as a signed 8-bit value.
- **SF = 1** — The most significant bit of `E2h` is 1.
- **ZF = 0** — The result is not zero.
- **PF = 1** — `E2h` contains four 1-bits, which is even.
- **AF = 0** — There is no borrow between the lower nibbles.

The final result is:

`AL = E2h`

---

## sub2.asm

The program subtracts 2000 from 1000.

1000 - 2000 = -1000

The 16-bit representation of `-1000` is `FC18h`.

### Flags

- **CF = 1** — A borrow is needed because 1000 is smaller than 2000.
- **OF = 0** — The signed result is within the 16-bit range.
- **SF = 1** — The most significant bit of `FC18h` is 1.
- **ZF = 0** — The result is not zero.
- **PF = 1** — The low byte `18h` contains two 1-bits.
- **AF = 0** — There is no borrow from the lower nibble.

The final result is:

`AX = FC18h`

---

## sub3.asm

The program subtracts 1 from 0.

0000h - 0001h = FFFFh

The result wraps around because a borrow is required.

### Flags after SUB

- **CF = 1** — A borrow is needed because 0 is smaller than 1.
- **OF = 0** — There is no signed overflow.
- **SF = 1** — The most significant bit of `FFFFh` is 1.
- **ZF = 0** — The result is not zero.
- **PF = 1** — `FFh` contains eight 1-bits, which is even.
- **AF = 1** — A borrow occurs from the lower nibble.

After the `SUB`:

`AX = FFFFh`

`CF=1, OF=0, SF=1, ZF=0, PF=1, AF=1`

The program then executes:

`SBB AX, 0`

Since `CF` was 1, the carry/borrow is included:

FFFFh - 0 - 1 = FFFEh

After the `SBB`:

`AX = FFFEh`

`CF=0, OF=0, SF=1, ZF=0, PF=0, AF=0`