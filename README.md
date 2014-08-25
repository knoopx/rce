# Reverse Code Engineering Notes

## lldb basics

```
$ lldb

# attach to the application; pauses it
lldb> attach MyApp

# sets the breakpoint by name
lldb> break set --name '-[MyObject doSomethingSpecial:]'

# resume the application
lldb> cont

# see assembly at breakpoint
lldb> dis

# set breakpoint after the function prolog found using dis
lldb> break set --addr 0x12345678

 # continue to breakpoint we just set
lldb> cont

# read the registers
lldb> reg read

lldb> po $rdi  
<MyObject: 0xdeadbeef>

lldb> p $rdi
(unsigned long)0xdeadbeef

lldb> p (SEL)$rdi  
(unsigned long) "abcde"
```


## Binary Diffing

```
$ brew install radare2
$ brew install graphviz --with-app
```

### Offset-based diff

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

### Graph diff

```
$ radiff2 -g main /bin/false /bin/true > output.dot
$ open output.dot
```
