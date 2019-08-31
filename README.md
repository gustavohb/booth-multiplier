# Booth's Multiplication Algorithm in VHDL

Booth's algorithm is a procedure for the multiplication of two signed binary numbers in two's complement notation.

This code is a behavioral implementation of the Booth's algorithm in VHDL.


## The algorithm

This algorithm can be described as follow:

If *x* is the number of bits of the multiplicand (in two's complement notation) and  *y* is the number of bits of the multiplier (also in two's complement notation):

+ Create a grid with 3 rows and *x* + *y* + 1 columns. Call the rows *A* (addition), *S* (subtraction), and *P* (product).

+ Fill in the first *x* bits of each row with:
  - A: the multiplicand
  - S: the negative of the multiplicand
  - P: zeros

+ Fill in the next *y* bits of each row with:
  - A: zeros
  - S: zeros
  - P: the multiplier

+ Put zero at the last bit of each row.

+ Repeat the procedure below *y* times:

  1. If the last two bits of the product are:
      - 00 or 11: do nothing.
      - 01: P = P + A. Ignore any overflow.
      - 10: P = P + S. Ignore any overflow.

  2. Shift *P* one bit to the right. In this step, the *P* signal must be preserved, that is, if the most significant bit is 1, then after shifting the new most significant bit must also be 1. If the most significant bit is 0, after shifting the new most significant bit should also be 0.

+ Drop the least significant (rightmost) bit from *P* for the final result.

The technique presented above is inadequate when the multiplicand is a negative number longer than what can be represented by the positive equivalent value (i.e. if the multiplicand is 8 bits then that value is -128).
A possible fix for this problem is to add one more bit to the left of *A*, *S*, and *P*.

#### Example

If you want to multiply -8 by 6 (1000 * 0110) with Booth's algorithm:

```
A = 1 1000 0000 0
S = 0 1000 0000 0
P = 0 0000 0110 0

Step 1:
  The last two bits of P are 00
  P = P >> 1
  P = 0 0000 0011 0

Step 2:
  The last two bits of P are 10
  P = (P+S) >> 1
  P = 0 0100 0001 1

Step 3:
  The last two bits of P are 11
  P = P >> 1
  P = 0 0010 0000 1

Step 4:
  The last two bits of P are 01
  P = (P+A) >> 1
  P = 1 1101 0000 0

The answer is: 1101 0000 = -48
```

For more information on it check [Booth's multiplication algorithm @ Wikipedia](https://en.wikipedia.org/wiki/Booth%27s_multiplication_algorithm).


## License

See the [LICENSE](https://github.com/gustavohb/booth-multiplier/blob/master/LICENSE) file for license rights and limitations (MIT)
