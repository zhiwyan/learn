---

title: gcc内联汇编

date: 2021-05-12 19:29:21

tags: [gcc,汇编]

categories: [gcc,汇编]

---

## gcc内联汇编

Example 1

```
Assembly (*.S):
				mov1 $0xffff, %eax


Inline assembly (*.c):
				asm ("mov1 $0xffff, %%eax\n");
```



Syntax

```c
acs ( assembler template
     : output operands			(optional)
     : input operands				(optional)
     : clobbers							(optional)
);
```

