# gameasm
<p>


sudo apt update

sudo apt install nasm gcc


sudo dpkg --add-architecture i386

sudo apt update

sudo apt install libc6:i386 libc6-dev:i386 gcc-multilib g++-multilib


git clone https://github.com/AnonimusShamshiAlex/gameasm

cd gameasm

nasm -f elf32 battle.asm -o battle.o

ld -m elf_i386 -o battle_game battle.o

./battle_game

</p>
