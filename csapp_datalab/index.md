# Csapp_datalab

## int bit-level operations
### 德摩根律（位运算和集合论）
- 与：&
- 非：~
- 两者组合已经可以表示四个基本运算：与、非、或、异或。
### 移动位运算
- 注意是否为无符号数，有符号数的移位运算规则与无符号数不同。
- 有符号数的移位运算规则：
    - 左移：符号位不变，右边补0。
    - 右移：符号位不变，左边补符号位。
- 无符号数的移位运算规则：
    - 左移：左边补0。
    - 右移：右边补0。

### 与运算（&）取特定的位数，用于位层面条件判断
### 减法的实现
- **A + ~A = -1 --> A + (~A+1) = 0**
### 减法的描述范围问题
- 做差取符号位

### 掩码操作
``` C
int conditional(int x, int y, int z) {
  int exp1 = ~(!!x) + 1;
  int exp2 = ~(!x) + 1;
  return (exp1&y) + (exp2&z);
}
```

### 位层面分类讨论
```c
/* howManyBits - return the minimum number of bits required to represent x in
 *             two's complement(补码系统)
 *  Examples: howManyBits(12) = 5
 *            howManyBits(298) = 10
 *            howManyBits(-5) = 4 负数的话 取反 同理  
 *            howManyBits(0)  = 1
 *            howManyBits(-1) = 1 特殊点?
 *            howManyBits(0x80000000) = 32
 *  Legal ops: ! ~ & ^ | + << >>
 *  Max ops: 90
 *  Rating: 4
 */
int howManyBits(int x) {
  // 取为正数操作
  int flag = x >> 31;
  x = (~flag & x) + (flag & (~x)); // 掩码分类思想
  // 0000 0100 0000 0000 0000 0000 0000 0000
  // Flag_i = !!(x >> i) 
  int bit16 = !!(x >> 16) << 4; // log2 N
  x >>= bit16; // >>= 右移赋值运算符

  int bit8 = !!(x >> 8) << 3;
  x >>= bit8;

  int bit4 = !!(x >> 4) << 2;
  x >>= bit4;

  int bit2 = !!(x >> 2) << 1;
  x >>= bit2;

  int bit1 = !!(x >> 1) << 0;
  x >>= bit1;

  int bit0 = x; // x = 1

  return (bit0+bit1+bit2+bit4+bit8+bit16) + 1;
}
```
## float bit-level operations
$$
float = (-1)^s * M * 2^E
$$
### float to binary
- sign bit: $s$
- exponent bits: $E$ ---> $E = e - 127$
- mantissa bits: $M$--->frac add $1$ and $0$ to the left until $M$ has 23 bits


