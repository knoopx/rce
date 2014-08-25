# Reverse Code Engineering Notes

## obj-c

|         | i386 (before prolog) | i386 (after prolog)  | x86_64        |
|---------|----------------------|----------------------|---------------|
| self    | `po *(id*)($esp)`    | `po *(id*)($ebp+8)`  | `po $rdi`     |
| method  | `p *(SEL*)($esp+4)`  | `p *(SEL*)($ebp+12)` | `p (SEL)$rsi` |
| arg1    | `po *(id*)($esp+8)`  | `po *(id*)($ebp+16)` | `po $rdx`     |
| arg2    | `po *(id*)($esp+12)` | `po *(id*)($ebp+20)` | `po $rcx`     |
| arg2    | `po *(id*)($esp+16)` | `po *(id*)($ebp+24)` | `po $r8`      |

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


# Tools

[readmem](https://github.com/gdbinit/readmem)
:  Read and dump process memory

[frida](http://www.frida.re/)
:  Runtime injection and scripting

[cycript](http://www.cycript.org/)
:  Runtime injection and scripting
