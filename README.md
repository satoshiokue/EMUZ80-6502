# EMUZ80-6502

![6502 Prototype](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_6502_proto.jpeg)

電脳伝説さん(@vintagechips)のEMUZ80で信号を組み替えたW65C02Sを動作させることができます。  
W65C02SとPIC18F47Q43の組み合わせで動作確認しています。

このソースコードはGazelleさんのEMUZ80用main_cpz.cを元に改変してGPLライセンスに基づいて公開するものです。

https://drive.google.com/drive/folders/1NaIIpzsUY3lptekcrJjwyvWTBNHIjUf1

## 回路図
https://github.com/satoshiokue/EMUZ80-Adapter-PCB/blob/main/65816.pdf

## ファームウェア
ファームウェアはCPUクロックの生成方法が異なります。
* emuz80_6502.c ソフトクロック
* emuz80_6502clk.c  デューティー比 50:50  

EMUZ80で配布されているフォルダemuz80.X下のmain.cと置き換えて使用してください。

emuz80_6502.cはメモリアクセス・通信I/O処理の完了でクロック信号をLowにして、その後クロック信号をHighにしてバスにアクセスします。  
PICはクロック信号のデューティー比を変化させながらW65C02Sを動作させます。

CPU動作実験にはPIC内蔵の数値制御発振器でクロック信号を生成しているemuz80_6502clk.c を推奨します。  

## アドレスマップ
```
RAM   0x0000 - 0x0FFF (0x1FFF:PIC18F47Q84)
UART  0x8018   Control REGISTER
      0x8019   Data REGISTER
ROM   0xC000 - 0xFFFF
```

## テストプログラムの書き込み
EMUZ80技術資料8ページにしたがってemuz80_6502.hexファイルをPICに書き込んでください。  
6502が起動するとメッセージが出力されます。キーを押すたびに同じメッセージが表示されます。

## 6502プログラムの改編
バイナリデータをテキストデータ化してファームウェアの配列rom[]に格納するとW65C02Sで実行できます。

テキスト変換例
```
xxd -i -c16 applesoft-liteROMv1.bin > applesoft-liteROMv1.txt
```

Leoさんの「SBC6800に6502を載せてApple II BASIC Subset を走らせる」を試すことができます。
https://yumeroku.blogspot.com/2020/06/sbc68006502apple-ii-basic-subset.html

## 謝辞
思い入れのあるCPUを動かすことのできるシンプルで美しいEMUZ80を開発された電脳伝説さんに感謝いたします。

そしてEMUZ80の世界を発展させている開発者の皆さんから刺激を受けて6502に挑戦しています。

## 参考）EMUZ80
EUMZ80はZ80CPUとPIC18F47Q43のDIP40ピンIC2つで構成されるシンプルなコンピュータです。

![EMUZ80](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_Z80.jpeg)

電脳伝説 - EMUZ80が完成  
https://vintagechips.wordpress.com/2022/03/05/emuz80_reference  
EMUZ80専用プリント基板 - オレンジピコショップ  
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-051.html

## 参考）phemu6809
comonekoさん(@comoneko)さんがEMUZ80にMC6809を搭載できるようにする変換基板とファームウェアphemu6809を発表されました。

![phemu6809](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_6809.jpeg)

https://github.com/comoneko-nyaa/phemu6809conversionPCB  
phemu6809専用プリント基板 - オレンジピコショップ  
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-056.html
