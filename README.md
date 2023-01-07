# EMUZ80-6502

![MEZ6502](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_6502_proto4.jpeg)  
左：ピンソケット実装、右：ピンヘッダー実装

![MEZ6502 Prototype2](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_6502_proto2.jpeg)  
試作基板二号

![6502 Prototype](https://github.com/satoshiokue/EMUZ80-6502/blob/main/imgs/IMG_6502_proto.jpeg)  
ブレッドボード試作

電脳伝説さん(@vintagechips)のEMUZ80が出力するZ80 CPU信号をメザニンボードで組み替え、W65C02Sを動作させることができます。  
W65C02SとPIC18F47Q43の組み合わせで動作確認しています。

動作確認で使用したCPU
W65C02S 14MHz  
R65C02P2 2MHz  
SYU6502A 2MHz  
UM6502A 2MHz  
UM6502 1MHz  
MOS6502 1MHz  

W65C816S 14MHz

ソースコードはGazelleさんのEMUZ80用main_cpz.cを元に改変してGPLライセンスに基づいて公開するものです。

https://drive.google.com/drive/folders/1NaIIpzsUY3lptekcrJjwyvWTBNHIjUf1

## メザニンボード
https://github.com/satoshiokue/MEZ6502


MEZ6502専用プリント基板 - オレンジピコショップ  
https://store.shopping.yahoo.co.jp/orangepicoshop/pico-a-058.html


## 回路図
https://github.com/satoshiokue/MEZ6502/blob/main/MEZ6502.pdf

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
RAM   0x0000 - 0x0FFF (0x1FFF:PIC18F47Q84,PIC18F47Q83)
UART  0x8018   Control REGISTER
      0x8019   Data REGISTER
ROM   0xC000 - 0xFFFF
```

## PICプログラムの書き込み
EMUZ80技術資料8ページにしたがってPICに適合するemuz80_6502_Qxx.hexファイルを書き込んでください。  

またはArduino UNOを用いてPICを書き込みます。  
https://github.com/satoshiokue/Arduino-PIC-Programmer

```
6502 EhBASIC [C]old/[W]arm ?

Memory size ?

Enhanced BASIC 2.22p5a
3327 Bytes free

Ready
```
起動メッセージが出たら「c」キーを押します。Memory sizeでリターンキーを押すと利用可能な最大メモリでBASICの起動が完了します。


Enhanced 6502 BASIC by Lee Davison  
https://philpem.me.uk/leeedavison/6502/ehbasic/

## 6502プログラムの改編
バイナリデータをテキストデータ化してファームウェアの配列rom[]に格納するとW65C02Sで実行できます。

テキスト変換例
```
xxd -i -c16 applesoft-liteROMv1.bin > applesoft-liteROMv1.txt
```

Leoさんの「SBC6800に6502を載せてApple II BASIC Subset を走らせる」を試すことができます。
https://yumeroku.blogspot.com/2020/06/sbc68006502apple-ii-basic-subset.html

## 65816に対応

65816はPin3、Pin38の入出力が逆になります。  
ファームウェアで無効化してある #define CPU_65816 を有効化するとPICのGPIO設定を65816用に切り替えます。

LED1とR1を実装すると65816がnative mode状態で点灯します。  
LEDの電流は65816の吸い込み電流が最大1.6mAなので暗く点灯します。

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
