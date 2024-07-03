## babymips

![image-20240701210757513](../../assets/image-20240701210757513.png)

简单的 xor

```c
// main
for ( i = 0; i < 0x20; ++i )
  v5[i] ^= 0x20 - (_BYTE)i;
if ( !strncmp(v5, fdata, 5u) )
  return sub_4007F0(v5);
```

奇数 ror，偶数 rol

```c
// sub_4007F0
for ( i = 5; i < strlen(a1); ++i )
{
  if ( (i & 1) != 0 )
    v1 = (a1[i] >> 2) | (a1[i] << 6);  // ror
  else
    v1 = (a1[i] << 2) | (a1[i] >> 6);  // rol
  a1[i] = v1;
}
if ( !strncmp(a1 + 5, (const char *)off_410D04, 0x1Bu) )
  return puts("Right!");
```

简单写个解密脚本

```python
fdata = bytearray.fromhex('517C6A7B67')
off_410D04 = bytearray.fromhex('52FD16A489BD9280134154A08D451881DEFC95F016791A155B751F')

def f():
    a1 = fdata + off_410D04
    for i in range(5, 0x20):
        if i & 1:
            a1[i] = ((a1[i] << 2) | (a1[i] >> 6)) & 0xFF
        else:
            a1[i] = ((a1[i] << 4) | (a1[i] >> 4)) & 0xFF
    return a1

def g():
    v5 = f()
    for i in range(0x20):
        v5[i] ^= 0x20 - i
    return v5

print(g())  # bytearray(b'qctf{ReA11y_4_B@89_mlp5_4_XmAn_}')
```
