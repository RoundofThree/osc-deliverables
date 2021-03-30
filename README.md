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

```


### Lab 1: Buffer overflow to bypass a license check

32-bit executable (64-bit executable load the 6 first arguments in special registers, it's a little bit trickier). 
1. Find buffer size. 
```
objdump -D exercise
```
Screenshot. 

2. Find the 


### Real world relevance: sudo privesc



### Lab 2: return-to-libc to spawn a shell

32-bit executable. 