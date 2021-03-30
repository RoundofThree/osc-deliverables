# OSC

### Classical buffer overflow [DIAGRAMS]

- Image 1: Stack layout
```
<<-- Lower addresses                          Upper addresses -->>   
|     BUFFER(local variable)      |   EBP  |  RET  |  parameters |
*------------- function stack frame -----------------------------* 
<<-- Stack grows to lower addresses                 
```

- Image 2: Smashed stack
```
<<-- Lower addresses 
|     BUFFER (local variable)     |   EBP   |  RET  |  parameters |
[    AAAA AAAA AAAA AAAA AAAA     ][  AAAA  ][ AAAA ][ parameters ]
-->> Buffer grows to higher addresses

```

- Image 3: Hijack the RET pointer to injected shellcode
```
<<-- Lower addresses 
|     BUFFER (local variable)     |   EBP   |        RET        |  parameters  |  other function stack frame...|
[    AAAA AAAA AAAA AAAA AAAA     ][  AAAA  ][ \xde\x4d\xc0\xde][ \x90\x90\x90 ][\x90\x90\x90 \xc0\xd4\xa3 ... ]
*------   20 bytes --------------* *4 bytes-**----4 bytes ------* *---- NOP sled -----------* *-- shellcode --*
                                                     |                                               ^
                                                     |                                               |
                                                     +-----------------------------------------------+
                                                                      hijack the RET 
-->> Buffer grows to higher addresses
```

- Image 4: Payload construction
```
PAYLOAD = [fill buffer]
```

### Lab 1: Buffer overflow to bypass a license check

32-bit executable (64-bit executable load the 6 first arguments in special registers, it's a little bit trickier). 
1. Find buffer size. 
```
objdump -D exercise
```
Screenshot. 

2. Inspect the functions of the binary. 


### Real world relevance: sudo privesc and Chrome 
#### Privesc
Screenshot of my (vulnerable) Ubuntu.
```
./hax-me-a-sandwich 2
```
Normal users can get a root shell by exploiting sudoedit. 

#### Initial foothold
Another real world relevance example is Google Chrome Heap Buffer Overflow, assigned CVE-2020-6452. The attacker
crafts a URL to gain initial foothold to the victim system when the victim clicks on it. 

### Lab 2: return-to-libc to spawn a shell

32-bit executable. 
No time. 
