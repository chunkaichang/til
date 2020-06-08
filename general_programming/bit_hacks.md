# Bit hacks

## minimum of two integers

- With Branch: `x < y ? x : y`
- Branchless: `y ^ ((x ^ y) & -(x < y))`

## modular addition
Compute (x + y) mod n (assuming 0 <= x < n and 0 <= y < n):

- Naive: `r = (x + y) % n;`
- With branch but without division:
```c
z = x + y;
r = z < n ? z : z - n;
```
- Branchless: similar trick as minimum
```c
z = x + y;
r = z - (n & -(z >= n));
```

## round up to next power of 2
The idea is to populate bits and then increment.
```c
uint64_t n;
--n; // needed if n is already power of 2
n |= n >> 1;
n |= n >> 2;
n |= n >> 4;
n |= n >> 8;
n |= n >> 16;
n |= n >> 32;
++n;
```

## compute the mask of the least-significant 1
`x & (-x)`: uses the property of 2's complement that toggles all bits higher than the least-significant 1.

## popcount
- Naive: shift by 1 and count until 0

- Clear least-significant 1 repetitively:
```c
for (r=0; x; ++r) x &= x - 1;
```

- Table lookup: for each, say, 8-bit nibble, do table lookup and then shift by 8.
```c
static const int count[256] = { 0, 1, 1, 2, 1, ..., 8};
for (int r = 0; x; x >>= 8) r += count[x & 0xFF];
```

- Parallel divide-and-conquer:
```c
uint64_t x;
// Create masks
M5 = ~((-1) << 32); // 0^32 1^32
M4 = M5 ^ (M5 << 16); // (0^16 1^16)^2 
M3 = M4 ^ (M4 << 8); // (0^8 1^8)^4
M2 = M3 ^ (M3 << 4); // (0^4 1^4)^8
M1 = M2 ^ (M2 << 2); // (0^2 1^2)^16
M0 = M1 ^ (M1 << 1); // (0 1)^32
// Compute
x = ((x >> 1) & M0) + (x & M0); // popcounts for each pair of adjacent bits
x = ((x >> 2) & M1) + (x & M1); // popcounts for each 4 bits
x = ((x >> 4) + x) & M2;
x = ((x >> 8) + x) & M3;
x = ((x >> 16) + x) & M4;
x = ((x >> 32) + x) & M5;
```

## Log2 of power of 2
- Use popcount: `popcount(x - 1)` 
- Multiply a deBruijn sequence: Since multiplying power of 2 is equal to left shift, the idea is to determine how many bits are shifted using the property of deBruijn sequences.

---
## References
[MIT 6.172](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-172-performance-engineering-of-software-systems-fall-2018/index.htm)

[Bit Twiddling Hacks](https://graphics.stanford.edu/~seander/bithacks.html)


