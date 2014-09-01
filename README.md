TD4 アセンブリ
==============

あの魔書『CPUの創りかた』の TD4 CPU の ROM 部分をArduinoで作る場合のアセンブリです。

命令セット
----------

    ;; X は 10進数 で 0-15です

    ADD A X  ; 0000xxxx AレジスタにXを加算
    ADD B X  ; 0101xxxx BレジスタにXを加算
    MOV A X  ; 0011xxxx AレジスタにXを転送
    MOV B X  ; 0111xxxx BレジスタにXを転送
    MOV A B  ; 00010000 AレジスタをBレジスタに転送
    MOV B A  ; 01000000 BレジスタをAレジスタに転送
    JMP X    ; 1111xxxx Xにジャンプ
    JNC X    ; 1110xxxx キャリーフラグ(C)がなければジャンプ
    IN  A    ; 00100000 INPUTからAにロード
    IN  B    ; 01100000 INPUTからBにロード
    OUT B    ; 10010000 BをOUTPUTへ
    OUT x    ; 1011xxxx OUTPUTへXを転送
    NOP      ; 00000000 何もしない (Aレジスタに0を加算)

使い方
======

必要
----

nodeJSが必要です。

    $ brew install node.js

コマンドについて
----------------

    $ ./arduino/asm src.tdasm
    byte code[16] = {
        B10110000,
        B00000001,
        B11100000,
    ...

上記のコードをコピーして、Arduinoエディタに貼付けてください。

つなぎ方について
----------------

アドレス INPUT
^^^^^^^^^^^^^^

    |PIN  |  2   3   4   5  |
    +-----+-----------------+
    |BIT  |  LSB <---> MSB  |

データ OUTPUT
^^^^^^^^^^^^^

    |PIN  |  6   7   8   9  10  11  12  13  |
    +-----+---------------------------------+
    |BIT  |  LSB <-------------------> MSB  |

例
^^^^

![Summer](https://pbs.twimg.com/media/BwXkIYQCAAE3OE4.jpg)