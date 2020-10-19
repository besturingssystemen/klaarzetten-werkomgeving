# Inhoudsopgave
- [Inhoudsopgave](#inhoudsopgave)
- [Leesmateriaal](#leesmateriaal)
- [Video](#video)
- [Linux-omgeving](#linux-omgeving)
- [Packages](#packages)
- [xv6](#xv6)

# Leesmateriaal

In deze oefenzittingenreeks werken we met het besturingsssysteem xv6. 
Ter voorbereiding van de oefenzitting vragen we om het eerste hoofdstuk (*Operating System interfaces*) van het [xv6 boek](https://github.com/mit-pdos/xv6-riscv-book/) te lezen.
[Hier](https://pdos.csail.mit.edu/6.828/2020/xv6/book-riscv-rev1.pdf) kan je een recente editie terugvinden.


> :bulb: In de komende stappen van de voorbereiding kan er veel wachttijd zijn bij het downloaden en installeren van Ubuntu en de packages. Lees dit materiaal terwijl je moet wachten op de installaties.

# Video

In onderstaande video wordt deze tutorial overlopen.

[![Bekijk de video](https://img.youtube.com/vi/vjJW36/hqdefault.jpg)](https://youtu.be/vjJW36_q_sg)

# Linux-omgeving

Om de oefenzittingen van dit vak succesvol af te kunnen leggen moeten jullie een werkende Linux-omgeving ter beschikking hebben.

Indien je nog geen werkende Linux-omgeving geïnstalleerd hebt kan je stap 1 (en optioneel 2) van [deze tutorial](https://github.com/informaticawerktuigen/klaarzetten-werkomgeving) volgen.

> :warning: Indien je niet kiest voor Ubuntu is het de bedoeling dat je zelf uitzoekt hoe je aan alle nodige packages geraakt. De [Risc-V GNU Toolchain](https://github.com/riscv/riscv-gnu-toolchain) moet je dan waarschijnlijk zelf compileren.

# Packages

Open een terminal met <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>T</kbd>. Installeer alle nodige packages in Ubuntu:

```shell
sudo apt install build-essential qemu-system-misc gcc-riscv64-linux-gnu 
```

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