# Inhoudsopgave
- [Inhoudsopgave](#inhoudsopgave)
- [Leesmateriaal](#leesmateriaal)
- [Video](#video)
- [Linux-omgeving](#linux-omgeving)
- [Packages](#packages)
- [xv6](#xv6)
- [gdb](#gdb)

# Leesmateriaal

In deze oefenzittingenreeks werken we met het besturingsssysteem xv6. 
Ter voorbereiding van de oefenzitting vragen we om het eerste hoofdstuk (*Operating System interfaces*) van het [xv6 boek](https://github.com/besturingssystemen/xv6-riscv-book) te lezen.
[Hier](https://github.com/besturingssystemen/xv6-riscv-book/releases/latest/download/book.pdf) kan je een recente PDF editie terugvinden.


> :bulb: In de komende stappen van de voorbereiding kan er veel wachttijd zijn bij het downloaden en installeren van Ubuntu en de packages. Lees dit materiaal terwijl je moet wachten op de installaties.

# Video

In onderstaande video wordt deze tutorial overlopen.

[![Bekijk de video](https://img.youtube.com/vi/vjJW36_q_sg/hqdefault.jpg)](https://youtu.be/vjJW36_q_sg)

# Linux-omgeving

Om de oefenzittingen van dit vak succesvol af te kunnen leggen moeten jullie een werkende Linux-omgeving ter beschikking hebben.

Indien je nog geen werkende Linux-omgeving geïnstalleerd hebt kan je stap 1 van [deze tutorial](https://github.com/informaticawerktuigen/klaarzetten-werkomgeving) volgen.

> :warning: Indien je niet kiest voor Ubuntu is het de bedoeling dat je zelf uitzoekt hoe je aan alle nodige packages geraakt. De [Risc-V GNU Toolchain](https://github.com/riscv/riscv-gnu-toolchain) moet je dan waarschijnlijk zelf compileren.

# Packages

Open een terminal met <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>T</kbd>. 

* Update je Linux-omgeving:
    ```shell
    sudo apt update
    sudo apt upgrade
    ```

* Installeer alle nodige packages in Ubuntu:

    ```shell
    sudo apt install build-essential git qemu-system-misc gcc-riscv64-linux-gnu 
    ```

> :warning: Indien je een oude release van Ubuntu hebt (<20.04) zal je niet de laatste versie van de RISC-V compiler kunnen installeren. In dat geval moet je je distributie upgraden met ```sudo do-release-upgrade``` of de compiler zelf builden vanuit de RISC-V GNU Toolchain Git repository.

# xv6

Ten slotte willen we het besturingssysteem [xv6](https://github.com/mit-pdos/xv6-riscv) clonen en compileren naar [RISC-V](https://riscv.org/):

* Clone de xv6 git-repository
    ```shell 
    git clone https://github.com/mit-pdos/xv6-riscv
    ```
* Compileer xv6
    ```shell
    cd xv6-riscv
    make qemu
    ```
Indien de compilatie succesvol is verlopen zal xv6 automatisch gestart worden in de huidige shell. Je krijgt nu onderstaande boodschap te zien:

```shell
xv6 kernel is booting

hart 2 starting
hart 1 starting
init: starting sh
$ 
```
Sluit xv6 af met de toetsencombinatie <kbd>CTRL</kbd> + <kbd>A</kbd> gevolgd door <kbd>x</kbd>. Je bent nu klaar met de voorbereiding.

# GDB

Om xv6 te debuggen, zullen we gebruik maken van [GDB][gdb].
Helaas is er heeft Ubuntu standaard geen packages voor de RISC-V versie van GDB en zullen we deze handmatig moeten installeren.
Download daarvoor het bestand `gdb-riscv64-unknown-elf_10.1-1_amd64.deb` van Toledo (onder "Documenten", "Oefenzittingen") en voer de volgende commando's uit in een terminal:

```shell
cd Downloads
sudo dpkg -i gdb-riscv64-unknown-elf_10.1-1_amd64.deb
```

Als je geen foutmeldingen kreeg, zou de RISC-V versie van GDB nu geïnstalleerd moeten zijn.
Check dit met het volgende commando:

```shell
riscv64-unknown-elf-gdb -v
```

Als alles goed is gegaan, zou dit onder andere het volgende moeten printen: "GNU gdb (GDB) 10.1".

Voor nu het volgende commando uit om GDB te configureren:

```shell
echo "set auto-load safe-path /" > ~/.gdbinit
```

Ga nu weer naar de `xv6-riscv` directory in een terminal en voer het volgende commando uit:

```shell
make qemu-gdb
```

Dit start xv6 met ondersteuning voor het gebruik van GDB.
De kernel begint niet uit te voeren voordat we er een debugger aan verbinden.
Open hiervoor een andere terminal en ga daarin ook naar de `xv6-riscv` directory.
Start de debugger nu:

```shell
riscv64-unknown-elf-gdb
```

De output zou er als volgt moeten uitzien (`...` betekent oninteressante output):
<pre>
GNU gdb (GDB) 10.1
...
0x0000000000001000 in ?? ()
(gdb)
</pre>

We kunnen nu commando's ingeven achter de `(gdb)`-prompt.

Dit zijn enkele belangrijke commando's:
- `continue` (`c`): ga verder met uitvoeren.
  Dit kan gebruikt worden om xv6 te starten als het nog niet aan het uitvoeren is of om de uitvoering na een breakpoint verder te zetten.
- `break symbol` (`b`): zet een breakpoint op het adres van `symbol`.
  Als dit adres wordt uitgevoerd, zal de processor stoppen en kan je in GDB commando's uitvoeren.
- `backtrace` (`bt`): print een backtrace uit.
- `quit` (`q`, <kbd>CTRL</kbd> + <kbd>D</kbd>): sluit GDB af.

De commando's worden kort getoond in de volgende GDB-sessie:

<pre>
0x0000000000001000 in ?? ()
(gdb) b fork
Breakpoint 1 at 0x80001ee0: file kernel/proc.c, line 267.
(gdb) c
Continuing.
[Switching to Thread 1.2]

Thread 2 hit Breakpoint 1, fork () at kernel/proc.c:267
267     {
(gdb) bt
#0  fork () at kernel/proc.c:267
#1  0x0000000080002cf8 in sys_fork () at kernel/sysproc.c:29
#2  0x0000000080002c6c in syscall () at kernel/syscall.c:142
#3  0x0000000080002956 in usertrap () at kernel/trap.c:67
#4  0x0000000000000050 in ?? ()
(gdb) q
Detaching from program: , process 1
Ending remote debugging.
[Inferior 1 (process 1) detached]
</pre>

[gdb]: https://www.gnu.org/software/gdb/
