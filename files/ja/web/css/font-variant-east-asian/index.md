---
title: font-variant-east-asian
slug: Web/CSS/font-variant-east-asian
l10n:
  sourceCommit: aac4966bd12c77281f9374bbfaf4e17e2680ac3b
---

**`font-variant-east-asian`** は [CSS](/ja/docs/Web/CSS) のプロパティで、日本語と中国語のような東アジアの字形の違いを制御するために使用します。

{{InteractiveExample("CSS デモ: font-variant-east-asian")}}

```css interactive-example-choice
font-variant-east-asian: normal;
```

```css interactive-example-choice
font-variant-east-asian: ruby;
```

```css interactive-example-choice
font-variant-east-asian: jis78;
```

```css interactive-example-choice
font-variant-east-asian: proportional-width;
```

```html interactive-example
<section id="default-example">
  <div id="example-element">
    <p>
      JIS78とJIS83以降では、檜と桧、籠と篭など、一部の文字の入れ替えが行われている。また、「唖然」や「躯体」などの書体が変更されている。
    </p>
  </div>
</section>
```

```css interactive-example
section {
  font-family:
    "YuGothic Medium", YuGothic, "Yu Gothic Medium", "Yu Gothic", sans-serif;
  margin-top: 10px;
  font-size: 1.5em;
}
```

## 構文

```css
font-variant-east-asian: normal;
font-variant-east-asian: ruby;
font-variant-east-asian: jis78; /* <east-asian-variant-values> */
font-variant-east-asian: jis83; /* <east-asian-variant-values> */
font-variant-east-asian: jis90; /* <east-asian-variant-values> */
font-variant-east-asian: jis04; /* <east-asian-variant-values> */
font-variant-east-asian: simplified; /* <east-asian-variant-values> */
font-variant-east-asian: traditional; /* <east-asian-variant-values> */
font-variant-east-asian: full-width; /* <east-asian-width-values> */
font-variant-east-asian: proportional-width; /* <east-asian-width-values> */
font-variant-east-asian: ruby full-width jis83;

/* グローバル値 */
font-variant-east-asian: inherit;
font-variant-east-asian: initial;
font-variant-east-asian: revert;
font-variant-east-asian: revert-layer;
font-variant-east-asian: unset;
```

### 値

- `normal`
  - : このキーワードは、別形式の表記の使用を無効にします。
- `ruby`
  - : このキーワードは、ルビ文字のための特殊な表記の使用を強制します。ふつうは小さめで、フォントの作者がよく特定の形状でデザインし、ふつうはコントラストを上げるためにわずかに太くします。このキーワードは OpenType の `ruby` の値に対応します。
- `<east-asian-variant-values>`
  - : これらの値は、表示に使用される字形の違いのセットを指定します。利用可能な値は以下の通りです。

    | キーワード    | 標準による形状の定義                                                 | OpenType の対応 |
    | ------------- | -------------------------------------------------------------------- | --------------- |
    | `jis78`       | [JIS X 0208:1978](http://ja.wikipedia.org/wiki/JIS_X_0208#第1次規格) | `jp78`          |
    | `jis83`       | [JIS X 0208:1983](http://ja.wikipedia.org/wiki/JIS_X_0208#第2次規格) | `jp83`          |
    | `jis90`       | [JIS X 0208:1990](http://ja.wikipedia.org/wiki/JIS_X_0208#第3次規格) | `jp90`          |
    | `jis04`       | [JIS X 0213:2004](http://ja.wikipedia.org/wiki/JIS_X_0213)           | `jp04`          |
    | `simplified`  | なし。簡体字中国語の字形を使用                                       | `smpl`          |
    | `traditional` | なし。繁体字中国語の字形を使用                                       | `trad`          |

- `<east-asian-width-values>`
  - : これらの値は東アジアの文字の大きさを制御します。2 つの値が利用できます。
    - `proportional-width` は、東アジアの文字の幅が多様なセットを有効にします。 OpenType の `pwid` の値に対応します。
    - `full-width` は、東アジアの文字がすべて同じ、およそ正方形で、一定幅のセットを有効にします。 OpenType の `fwid` に対応します。

## 公式定義

{{cssinfo}}

## 形式定義

{{csssyntax}}

## 例

### 東アジアの字形の変化形を設定

この例は OS に「游ゴシック」フォントがインストールされていることが必要です。ほかのフォントでは OpenType 機能に対応していない可能性があります。

#### HTML

```html
<table>
  <thead></thead>
  <tbody style="border:0;">
    <tr>
      <th>normal/jis78:</th>
      <td>麹町</td>
      <td class="jis78">麹町</td>
    </tr>
    <tr>
      <th>normal/ruby:</th>
      <td>しんかんせん</td>
      <td class="ruby">しんかんせん</td>
    </tr>
    <tr>
      <th>normal/traditional:</th>
      <td>大学</td>
      <td class="traditional">大学</td>
    </tr>
  </tbody>
</table>
```

#### CSS

```css
td {
  font-family: "Yu Gothic";
  font-size: 20px;
}
th {
  color: grey;
  padding-right: 10px;
}

.ruby {
  font-variant-east-asian: ruby;
}

.jis78 {
  font-variant-east-asian: jis78;
}

.traditional {
  font-variant-east-asian: traditional;
}
```

#### 結果

{{EmbedLiveSample('Setting_East_Asian_glyph_variants')}}

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- [`font-variant`](/ja/docs/Web/CSS/font-variant)
- [`font-variant-alternates`](/ja/docs/Web/CSS/font-variant-alternates)
- [`font-variant-caps`](/ja/docs/Web/CSS/font-variant-caps)
- [`font-variant-emoji`](/ja/docs/Web/CSS/font-variant-emoji)
- [`font-variant-ligatures`](/ja/docs/Web/CSS/font-variant-ligatures)
- [`font-variant-numeric`](/ja/docs/Web/CSS/font-variant-numeric)
- [`font-variant-position`](/ja/docs/Web/CSS/font-variant-position)
