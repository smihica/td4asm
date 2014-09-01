## TD4 アセンブラ

あの魔書『[CPUの創りかた](http://www.amazon.co.jp/CPU%E3%81%AE%E5%89%B5%E3%82%8A%E3%81%8B%E3%81%9F-%E6%B8%A1%E6%B3%A2-%E9%83%81/dp/4839909865/)』の TD4 CPU の ROM 部分をArduinoで作る場合のアセンブラです。
NodeJSでAssembly言語をArduinoに保存できるC言語に変換します。つなげばすぐ使えます。

### 命令セット

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

### アセンブリの例

    ;;; SLOWクロックで約1分たったら、出力ポートを1111にする。
    ;
    OUT 0      ; 0を出力ポートに送る
    ADD A 1    ; レジスタAに1を加える
    JNC 0      ; 桁があふれなければアドレス0にジャンプする
    MOV A 11   ; レジスタAを11にする
    ADD A 1    ; レジスタAに1を加える
    JNC 4      ; 桁があふれなければアドレス4にジャンプする
    OUT 15     ; 15を出力ポートに送る
    JMP 7      ; 停止する


### 使い方

__必要__

NodeJSが必要です。

    $ brew install node.js # macでbrewでインストールする場合

__コマンド__

    $ ./arduino/asm src.tdasm
    byte code[16] = {
        B10110000,
        B00000001,
        B11100000,
    ...

プリントされたのCのコードをコピーして、Arduinoエディタに貼付け、転送してください。

### Arduinoのつなぎ方

__アドレス INPUT__

    | PIN |  2   3   4   5  |
    +-----+-----------------+
    | BIT |  LSB <---> MSB  |

__データ OUTPUT__

    | PIN |  6   7   8   9  10  11  12  13  |
    +-----+---------------------------------+
    | BIT |  LSB <-------------------> MSB  |

__例__

![Example](https://pbs.twimg.com/media/BwXkIYQCAAE3OE4.jpg)
