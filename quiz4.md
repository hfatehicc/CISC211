```assembly
SECTION .data
        var1 DD 3
        var2 DD 6
        var3 DD 7
SECTION .text
global _start
_start:
        ;pushing arguments to stack
        mov eax,[var1]
        push eax
        mov eax,[var2]
        push eax,
        mov eax,[var3]
        push eax
        call _add       ;call add function
        ;pop existing elements to clear stack
        pop eax
        pop eax
        pop eax
        ;exit
        mov eax,1
        int 0x80

_add:
        ;retrieve numbers from stack and add to result
        mov eax,DWORD[esp+4]
        add [result],eax
        mov eax,DWORD[esp+8]
        add [result],eax
        mov eax,DWORD[esp+12]
        add [result],eax
        ret
SECTION .bss
        result RESB 1   ;expected end value 16
```
