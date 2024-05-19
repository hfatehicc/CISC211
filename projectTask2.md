```assembly
SECTION .data
        count DD 2000
        content DB '2000'
        fFile DB 'counter_fun.txt',0h
        rFile DB 'counter_rec.txt',0h
SECTION .text
global _start
_start:
        mov ebx,[count]
        push ebx
        call _func
        mov ecx,[count]
        call _rec



        mov eax,1
        int 0x80
_func:
        mov eax,[esp+4]
        lpf:
        dec eax
        cmp eax,0
        je donef
        jmp lpf
        donef:
        mov eax,5
        mov ebx,fFile
        mov ecx,1
        mov edx,0777o
        int 0x80

        mov [fd_out],eax

        mov eax,4
        mov ebx,[fd_out]
        mov ecx,content
        mov edx,4
        int 0x80

        mov eax,6
        mov ebx,[fd_out]
        int 0x80
        ret

_rec:
        cmp ecx,0
        je doneR
        dec ecx
        call _rec
        doneR:
        mov eax,5
        mov ebx,rFile
        mov ecx,1
        mov edx,0777o
        int 0x80

        mov [fd_out],eax

        mov eax,4
        mov ebx,[fd_out]
        mov ecx,content
        mov edx,4
        int 0x80

        mov eax,6
        mov ebx,[fd_out]
        int 0x80
        ret

SECTION .bss
        fTime RESB 1
        rTime RESB 1
        fd_out RESB 1
```
