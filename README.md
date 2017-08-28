# KAGE engine

くろごまが改変したKAGEエンジンです。

[デモページ](https://kurgm.github.io/kage-engine/)

## 改変したところ

- コードの整理
  - TypeScriptに書き換え、型チェックをする
  - 一部のアルゴリズムを変更し、計算量を減らす（and/or 精度を上げる）
- バグの修正
  - 筆画が垂直上向きな箇所でポリゴンが壊れることがあるのを修正
- 仕様の変更
  - まだない

### コードの整理について

コードの整理については、いろいろなKAGEデータに対してそれぞれの出力が大きく違わない範囲で行っています。（大きく違わない、は「ポリゴン数と各ポリゴンの頂点数が同じであって各頂点のX・Y座標の差がどちらも0.5以内」を目安としています。比較対象は、フォーク元から浮動小数点数の誤差を丸めるなどの変更を加えた `orig_kage` ブランチです。）

ただし、入力データには以下の仮定を置いており、これに反するデータの出力は大きく変わる可能性があります。

- 部品をすべて分解した際に、制御点に(±0, ±0)を複数持つ筆画は存在しない
- 筆画データの3列目までは非負整数である（1列目が0または99である場合を除く）
- 筆画データ内の数値は有限値である（`Infinity` や `-Infinity` は存在しない）

また、以下に挙げるようなバグ？が見つかっていますが、非漢字グリフに影響が出る可能性があるためわざと挙動は変えないようにしています。修正は影響を精査してから行う予定です。

- 普通の漢字グリフで出現しうると思われるもの
  - 「曲線（複曲線）」の筆画で頭形状が「左上カド」で始点と（1つ目の）中間点のx座標が同じときに左上カドが見えなくなる
  - 「直線」の筆画で尾形状「止め」・「右ハネ」の丸の形状が一定でなく角度によって微妙に変化する
  - 「直線」の縦画で、頭形状「開放」の形状が筆画の角度によって微妙に異なる
- 普通の漢字グリフでは出現しないと思われるもの
  - ~~筆画が垂直上向きな箇所でポリゴンが壊れることがある(https://github.com/kamichikoichi/kage-engine/pull/4)~~ →修正済み
  - 「曲線（複曲線）」の筆画で尾形状が「止め」・「右ハネ」の場合に（2つ目の）中間点→終点が水平左向きまたは垂直上向きの場合に最後の丸が見えなくなる
  - 「曲線（複曲線）」の筆画で「細入り→右払い」で（2つ目の）中間点→終点が水平左向き・垂直上向き・垂直下向きの場合に形状がおかしくなる
  - 「折れ」の筆画で尾形状が「上ハネ」で後半が水平でない左向きの場合に、ハネがやや細い
  - その他、始点または終点が垂直上向きや水平左向きの筆画で各種の頭形状や尾形状の向きがおかしくなることがある

などなど……（[GlyphWiki:バグ報告](http://glyphwiki.org/wiki/GlyphWiki:%E3%83%90%E3%82%B0%E5%A0%B1%E5%91%8A)にもKAGEエンジンのバグが報告されています）

## ライセンス

GPL v3
