2.
1. 把所有整数（N个）看成二进制表示法，将第一位bit为1的数目和第一位bit为0的比较，必有一个数目大于等于另一个。
2. 在数目大的那堆数字中继续比较第二bit位，按照1的方法比较，以此类推最后能得到重复出现的数字。

3.
精巧的杂技动作：
```
for i = [0, gcd(n, rotdist)]//gcd 求最大公约数
  t = x[i]
  j = i

  loop
    k = j + rotdist
    if k >= n
      k -= n
    if k == i
      break
    x[j] = x[k]
    j = k

 x[j] = x[k]
```
