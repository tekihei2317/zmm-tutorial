# zmm-tutorial

[ZMM](https://www.3qe.us/zmm/doc/)を使って動画を作ってみます。

```bash
docker compose run --rm -it zmm init

# VOICEVOX Engineを起動しておく
docker compose up voicevox-cpu
```

- assets/background.jpeg
- assets/bgm.mp3
- assets/metan.png
- assets/zunda.png

が必要らしいです。立ち絵は、検索するとPSDファイルでダウンロードできました。それをPSDToolというブラウザで使えるツールで開くと、画像でダウンロードできました。PSDToolでは、表情などをカスタマイズできるようです。

```bash
docker compose run --rm zmm script.xml
```

を実行するとエラーが出ました。

### エラー

```text
[concat @ 0x58f3500] Impossible to open '/app/artifacts/html/f0bc47cb47e7a8b8e5049f230937ccfbf599af6.html.png'
artifacts/cutFile.txt: No such file or directory

        at os.proc.call(ProcessOps.scala:85)
        at com.github.windymelt.zmm.infrastructure.FFmpegComponent$ConcreteFFmpeg.$anonfun$concatenateImagesWithDuration$3(FFmpeg.scala:126)
        at delay @ com.github.windymelt.zmm.infrastructure.FFmpegComponent$ConcreteFFmpeg.$anonfun$concatenateImagesWithDuration$2(FFmpeg.scala:126)
        at map @ com.github.windymelt.zmm.infrastructure.FFmpegComponent$ConcreteFFmpeg.$anonfun$concatenateImagesWithDuration$2(FFmpeg.scala:108)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at clear @ org.http4s.client.middleware.Retry$.retryLoop$1(Retry.scala:92)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at rethrow$extension @ fs2.Compiler$Target.$anonfun$compile$1(Compiler.scala:157)
        at get @ org.typelevel.keypool.KeyPool$.$anonfun$take$8(KeyPool.scala:305)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Compiler$Target.flatMap(Compiler.scala:163)
        at flatMap @ fs2.Pull$.fs2$Pull$$interruptGuard$1(Pull.scala:929)
```

`html.png`という変な拡張子のファイルを開こうとしていることが分かります。`artifacts/cutFile.txt`に

```text
file /app/artifacts/html/f0bc47cb47e7a8b8e5049f230937ccfbf599af6.html.png
outpoint 2.17
file /app/artifacts/html/4328319e23a729473590a9b2ce27b1d6846195.html.png
outpoint 2.56
file /app/artifacts/html/83844f262c18ea587f5892de39595f3b64a740ea.html.png
outpoint 3.55
file /app/artifacts/html/134aace363afe1657578fd807a48d6e8edbb85ac.html.png
outpoint 1.26
```

と書かれており、このファイルに書かれているファイルを読み込もうとしているっぽいです。ちなみに`artifacts/html/{id}.html`というファイルは存在します。つまり、`cutFile.txt`に書かれているファイル名が変になっているような気がします。

`artifacts`ディレクトリを削除するとまた別のエラーになるので、やり直す場合は`artifacts/`と、`artifacts/html/`は先に作っておく必要があります。
