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

64-bit executable.

Test this is vulnerable to buffer overflow -> Segfault means RET was overriden by our input. 
1. Find interesting function. Note down the address.
```
gdb lollycat
info func
```

2. The address is not exactly this. The addresses are moved by an offset.
```
gdb lollycat
break main
```
We add this offset to the <success> function address we got before. 

3. Find buffer size. 
```
./xpattern 50 > input
```

4. Find the place where RET is overriden.
```
gdb lollycat
run < input
./xpattern 0xaddressinsegfault -> position 40 -> buffer_size + local variable + ebp size = 40 bytes
```

5. Write the exploit.
```
python exploit.py > input
cat input | ./lollycat 
```

6. Pusheen!

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
