# Math Notes For Development

### Need a fast calculation of BDC (Binary Coded Decimal)
### With the final number being an xor checksum
```
0 = 0000
1 = 0001
2 = 0010
3 = 0011
4 = 0100
5 = 0101
6 = 0110
7 = 0111
8 = 1000
9 = 1001
```

### Fast algorithm for the following running average

```
# Assumes average over a infinite number of samples b/c count does not have an upper limit
next = 0
current = 0
count = 0

function average(number) {
  count = count + 1
  next = current + (number - current)/count
  current = next
}
```

