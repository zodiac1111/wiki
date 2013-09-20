# c语言中bit操作

* http://stackoverflow.com/questions/47981/how-do-you-set-clear-and-toggle-a-single-bit-in-c

## Setting a bit 置位

Use the bitwise OR operator (|) to set a bit.
```c
number |= 1 << x;
```
That will set bit x.

## Clearing a bit 清零

Use the bitwise AND operator (&) to clear a bit.
```c
number &= ~(1 << x);
```
That will clear bit x. You must invert the bit string with the bitwise NOT operator (~), then AND it.

## Toggling a bit 反转

The XOR operator (^) can be used to toggle a bit.
```c
number ^= 1 << x;
```
That will toggle bit x.

## Checking a bit 检查位

You didn't ask for this but I might as well add it.

To check a bit, AND it with the bit you want to check:
```c
bit = number & (1 << x);
```
That will put the value of bit x into the variable bit.