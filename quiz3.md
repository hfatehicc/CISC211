~~~assembly
section .text
        global _start

_start:
        mov eax, 1
        mov ebx, [fac]
        jmp loop
loop:
        mul ebx
        dec ebx
        cmp ebx,0
        je exit
        jmp loop

exit:
        mov [result],eax
        mov eax,1
        int 0x80

section .data
        fac DD 6

section .bss
        result RESB 4
~~~
