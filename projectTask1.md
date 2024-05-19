```assembly
SECTION .data   ;define variables and msg lengths
        pt DB 'Mario',0h
        key DB 'Luigi',0h
        filename DB 'p1output.txt',0h
        msg1 DB 'Plain Text: Mario',0xa,'Key: Luigi',0xa
        len1 equ $-msg1
        msg2 DB 'Encrypted Text: '
        len2 equ $-msg2
        msg3 DB 0xa,'Decrypted Text: '
        len3 equ $-msg3
SECTION .text
global _start
_write: ;function to write out assuming file is opened and ecx and edx define before
        mov eax,4
        mov ebx,[fd_out]
        int 0x80
        ret
_create:        ;creates file and writes output, then closes file

        ;create file
        mov eax,8
        mov ebx,filename
        mov ecx,0711o
        int 0x80
        mov [fd_out],eax
        ;open file
        mov eax,5
        mov ebx,filename
        mov ecx,1
        mov edx,0777o
        int 0x80
        ;write all msgs into file
        mov ecx,msg1
        mov edx,len1
        call _write

        mov ecx,msg2
        mov edx,len2
        call _write

        mov ecx,hexval
        mov edx,5
        call _write

        mov ecx,msg3
        mov edx,len3
        call _write

        mov ecx,output
        mov edx,5
        call _write
        ;close file
        mov eax,6
        mov ebx,[fd_out]
        int 0x80
        ret
_start:
        mov ecx,0       ;sets ecx to 0 for counter
        eloop:
        mov dh,[pt+ecx]         ;grabs 1 byte from plain text
        mov dl,[key+ecx]        ;grabs 1 byte from key
        XOR dh,dl
        mov [hexval+ecx],dh     ;stores XOR'd value into hexval
        inc ecx
        cmp ecx,5
        jl eloop        ;loop 5 times, since plain text and key are 5 bytes long

        mov ecx,0
        dloop:          ;loop functions same as encrypt but writes the decrypted value into output
        mov dh,[hexval+ecx]
        mov dl,[key+ecx]
        XOR dh,dl
        mov [output+ecx],dh
        inc ecx
        cmp ecx,5
        jl dloop

        call _create    ;call the create file function which will also write everything into file

        mov eax,1       ;exit
        int 0x80

SECTION .bss
        fd_out RESB 1
        hexval RESB 5
        output RESB 5
```
