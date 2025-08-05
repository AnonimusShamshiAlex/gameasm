section .data
    intro db "=== BATTLE RPG ===", 10, 0
    menu db "1. Attack", 10, "2. Heal", 10, "Your move: ", 0

    you_win db "You won!", 10, 0
    you_lose db "You lost!", 10, 0

    your_hp_msg db "Your HP: ", 0
    enemy_hp_msg db "Enemy HP: ", 0
    nl db 10, 0

section .bss
    input resb 2
    your_hp resb 1
    enemy_hp resb 1

section .text
    global _start

_start:
    mov byte [your_hp], 20
    mov byte [enemy_hp], 20

print_intro:
    mov eax, 4
    mov ebx, 1
    mov ecx, intro
    mov edx, 18
    int 0x80

game_loop:
    call print_stats
    call print_menu
    call get_input

    mov al, [input]
    cmp al, '1'
    je .attack
    cmp al, '2'
    je .heal
    jmp game_loop

.attack:
    ; игрок бьёт врага на 3 урона
    mov al, [enemy_hp]
    sub al, 3
    mov [enemy_hp], al
    jmp enemy_turn

.heal:
    ; игрок лечится на 2
    mov al, [your_hp]
    add al, 2
    cmp al, 20
    jbe .no_cap
    mov al, 20
.no_cap:
    mov [your_hp], al
    jmp enemy_turn

enemy_turn:
    ; враг бьёт на 1–4 урона (имитация случайности: остаток от времени)
    mov eax, 13     ; sys_time
    int 0x80
    xor edx, edx
    mov ecx, 4
    div ecx
    add edx, 1       ; от 1 до 4 урона
    mov cl, [your_hp]
    sub cl, dl
    mov [your_hp], cl

    ; проверка победы
    mov al, [your_hp]
    cmp al, 0
    jle lose

    mov al, [enemy_hp]
    cmp al, 0
    jle win

    jmp game_loop

win:
    mov eax, 4
    mov ebx, 1
    mov ecx, you_win
    mov edx, 10
    int 0x80
    jmp exit

lose:
    mov eax, 4
    mov ebx, 1
    mov ecx, you_lose
    mov edx, 11
    int 0x80
    jmp exit

exit:
    mov eax, 1
    xor ebx, ebx
    int 0x80

; ---------- PRINT FUNCTIONS -----------

print_menu:
    mov eax, 4
    mov ebx, 1
    mov ecx, menu
    mov edx, 28
    int 0x80
    ret

print_stats:
    call print_nl

    ; Твои HP
    mov eax, 4
    mov ebx, 1
    mov ecx, your_hp_msg
    mov edx, 10
    int 0x80

    movzx eax, byte [your_hp]
    add al, '0'
    mov [input], al
    mov eax, 4
    mov ebx, 1
    mov ecx, input
    mov edx, 1
    int 0x80
    call print_nl

    ; HP врага
    mov eax, 4
    mov ebx, 1
    mov ecx, enemy_hp_msg
    mov edx, 11
    int 0x80

    movzx eax, byte [enemy_hp]
    add al, '0'
    mov [input], al
    mov eax, 4
    mov ebx, 1
    mov ecx, input
    mov edx, 1
    int 0x80

    call print_nl
    ret

print_nl:
    mov eax, 4
    mov ebx, 1
    mov ecx, nl
    mov edx, 1
    int 0x80
    ret

get_input:
    mov eax, 3
    mov ebx, 0
    mov ecx, input
    mov edx, 2
    int 0x80
    ret
