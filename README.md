# Reverse Code Engineering Notes

# Binary Diffing

```
$ brew install radare2
$ brew install graphviz --with-app
```

## Offset-based diff

```
$ radiff2 genuine cracked
0x000081e0 85c00f94c0 => 9090909090 0x000081e0  
0x0007c805 85c00f84c0 => 9090909090 0x0007c805

$ rasm2 -d 85c00f94c0
test eax, eax  
sete al

$ radiff2 -s /bin/true /bin/false
similarity: 0.97  
distance: 743  
```

## Graph diff

```
$ radiff2 -g main /bin/false /bin/true > output.dot
$ open output.dot
```
