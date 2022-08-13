# EMUZ80-6502

電脳伝説さん(@vintagechips)のEMUZ80で信号を組み替えたW65C02Sを動作させることができます。

## 回路図

## ファームウェア
ファームウェアはCPUクロックの生成方法が異なります。
* emuz80_6502.c ソフトクロック
* emuz80_6502clk.c  デューティー比 50:50  
フォルダemuz80.X下のmain.cと置き換えて使用してください。

## アドレスマップ
```
RAM   0x0000 - 0x0FFF (0x1FFF:PIC18F47Q84)
UART  0x8018   Control REGISTER
      0x8019   Data REGISTER
ROM   0xC000 - 0xFFFF
```

## テストプログラムの書き込み
EMUZ80技術資料8ページにしたがってemuz80_6502.hexファイルをPICに書き込んでください。

## 6502プログラムの改編
バイナリデータをテキストデータ化してファームウェアの配列rom[]に格納するとW65C02Sで実行できます。

テキスト変換例
```
xxd -i -c16 applesoft-liteROMv1.bin > applesoft-liteROMv1.txt
```

EUMZ80はZ80CPUとPIC18F47Q43のDIP40ピンIC2つで構成されるシンプルなコンピュータです。

電脳伝説 - EMUZ80が完成
https://vintagechips.wordpress.com/2022/03/05/emuz80_reference
EMUZ80専用プリント基板 - オレンジピコショップ
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-051.html

まもなくcomonekoさん(@comoneko)さんがEMUZ80にMC6809を搭載できるようにする変換基板とファームウェアphemu6809を発表されました。
https://github.com/comoneko-nyaa/phemu6809conversionPCB
phemu6809専用プリント基板 - オレンジピコショップ
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-056.html

## Author

作成情報を列挙する

* 作成者
* 所属
* E-mail

## License
"EMUZ80-6502" is under [MIT license](https://en.wikipedia.org/wiki/MIT_License).
