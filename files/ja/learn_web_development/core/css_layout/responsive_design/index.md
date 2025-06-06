---
title: レスポンシブデザイン
slug: Learn_web_development/Core/CSS_layout/Responsive_Design
l10n:
  sourceCommit: a92e10b293358bc796c43d5872a8981fd988a005
---

{{learnsidebar}}

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}

_レスポンシブウェブデザイン_ (RWD) は、ユーザビリティを確保しながら、すべての画面サイズと解像度でウェブページをうまく描画するためのウェブデザインの手法です。複数の端末に対応したウェブをデザインする方法です。この記事では、それを使いこなすために使用できるいくつかのテクニックを理解することをお手伝いします。

<table>
  <tbody>
    <tr>
      <th scope="row">前提知識:</th>
      <td>
        <a href="/ja/docs/Learn_web_development/Core/Structuring_content"
          >HTML によるコンテンツの構造化</a
        >、
        <a href="/ja/docs/Learn_web_development/Core/Styling_basics">CSS によるスタイル設定の基本</a>、
        <a href="/ja/docs/Learn_web_development/Core/Text_styling/Fundamentals">基本的なテキストとフォントのスタイル設定</a>、
        <a href="/ja/docs/Learn_web_development/Core/CSS_layout/Introduction">CSS レイアウトの基本概念</a>の基礎知識。
      </td>
    </tr>
    <tr>
      <th scope="row">学習成果:</th>
      <td>
        <ul>
          <li>レスポンシブデザインとは、ウェブレイアウトを柔軟に設計し、異なる端末の画面サイズや解像度などに対応してうまく動作するようにすること。</li>
          <li>グリッドやフレックスボックスなどの最新のレイアウトツールとレスポンシブデザインの関係。</li>
          <li>レスポンシブデザインにメディアクエリーを使用する背景にある概念。モバイルファーストやブレークポイントなどを含む。</li>
          <li>ウェブ文書をモバイル端末で正しく表示するために、 <code>&lt;meta viewport=""&gt;</code> が必要な理由。</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## レスポンシブデザインの前身：モバイルウェブデザイン

レスポンシブウェブデザインが、様々な端末間でウェブサイトが動作するための標準的な手法となる以前は、ウェブ開発者はモバイルウェブデザイン、モバイルウェブ開発、または時にはモバイルフレンドリーデザインを使用していました。これは基本的にレスポンシブウェブデザインと同じで、レイアウト、コンテンツ（テキストとメディア）、パフォーマンスにおいて、物理的属性（画面サイズ、解像度）の異なる端末間でウェブサイトがうまく動作するようにすることを目的としています。

違いは主に、対象となる端末と、ソリューションを作成するために利用できる技術にあります。

- 以前はデスクトップかモバイルかいう話でしたが、今では利用できる端末はデスクトップ、ラップトップ、モバイル、タブレット、時計など多岐にわたるようになりました。いくつかの異なる画面サイズに合わせなければなりませんが、これで、一般的な画面サイズと解像度に加え、未知のものにも対応できるよう、防御的にサイトをデザインする必要があります。
- 使用するモバイル端末は、 CPU/GPU や利用できる帯域幅の点で低性能でした。 CSS はもちろん、 HTML さえ対応していないものもあり、その結果、端末が対応できるサイトを提供する前に、サーバー側でブラウザー検出を行い、端末/ブラウザーの種類を判別するのが一般的でした。モバイル端末では、実に単純で基本的な使い勝手しか提供されないことが多く、それが端末が処理することのできるすべてだったからです。最近では、モバイル端末はデスクトップコンピューターと同じ技術を処理することができるため、このような技術はあまり見かけなくなりました。
  - バッテリー寿命や 帯域幅など気にしなければならない制約がまだあるため、モバイルユーザーに適切な使い勝手を提供するために、この記事で説明したテクニックを使用しましょう。
  - ユーザーの使い勝手も考慮する必要があります。旅行サイトのモバイルユーザーは、例えばフライト時刻や遅延情報を調べたいだけで、フライト経路や会社の沿革を示す 3D アニメーションの地球儀を表示させたくないかもしれません。しかし、これはレスポンシブデザインの技術を使用して処理することができます。
- 現行の技術はレスポンシブな使い勝手を作成するのに非常に優れています。例えば、[レスポンシブ画像/メディア技術](#レスポンシブ画像とメディア)は、サーバー側の解析のような技術に頼っていなくても、適切なメディアを様々な端末に提供できるようになりました。

## レスポンシブウェブデザインの紹介

HTML は基本的にレスポンシブ、つまり流動的です。CSS を含まない HTML だけのウェブページを作成し、ウィンドウのサイズを変更すると、ブラウザーはビューポートに合わせてテキストを自動的に再フローします。

既定のレスポンシブ動作では解決策が必要ないように聞こえるかもしれませんが、広いモニターに全画面表示された長い行のテキストは読みにくい場合があります。段組みを作成したり、大きなパディングを追加したりして、CSS で広い画面の行の長さを縮小すると、ブラウザーウィンドウを狭めたり、モバイル端末でサイトを開いたりしたユーザーには、サイトがつぶれて見えることがあります。

![モバイルサイズのビューポートに押しつぶされた 2 列のレイアウト。](mdn-rwd-liquid.png)

固定幅を設定してリサイズ不可能なウェブページを作成してもうまく行きません。画面が狭い端末ではスクロールバーが表示され、広い画面では余白が多くなってしまいます。

レスポンシブウェブデザイン (RWD) とは、様々な端末や 機器のサイズに対応するデザイン手法のことで、コンテンツをタブレット、携帯電話、テレビ、腕時計のどれで表示しても、自動的に画面に合わせることができるものです。

レスポンシブウェブデザインは単体の技術ではなく、手法のひとつです。これは、コンテンツを表示するために使用するあらゆる端末に応じることができるレイアウトを作成するために使用される、ベストプラクティス群を表すために使用される用語です。

レスポンシブデザインという用語は、[2010 年に Ethan Marcotte によって作られ](https://alistapart.com/article/responsive-web-design/)、流動的グリッド、流動的画像、メディアクエリーを使用してレスポンシブなコンテンツを作成することを説明するものです。

当時は、レイアウトに CSS の `float` を使用し、メディアクエリーでブラウザーの幅を問い合わせ、様々なブレークポイントに対応したレイアウトを作成することが推奨されていました。流動的画像はコンテナーの幅を超えないように設定します。これらの画像は `max-width` プロパティを `100%` に設定しなければなりません。流動的画像は、その段組みが狭くなると縮小しますが、列が大きくなっても内在サイズより大きくなることはありません。これにより、画像はコンテンツをはみ出すことなく、そのコンテンツに合わせて拡大縮小することができ、コンテナーが画像よりも広くなっても、大きくなってピクセル化することはありません。

現代の CSS レイアウトメソッドは本質的にレスポンシブであり、Gillenwater の書籍と Marcotte の記事が発表されて以来、ウェブプラットフォームにはレスポンシブサイトの設計を容易にする多数の機能が組み込まれています。

この記事のこれから後の部分では、レスポンシブサイトを作成するときに使用できるさまざまなウェブプラットフォームの機能について説明します。

## メディアクエリー

[メディアクエリー](/ja/docs/Web/CSS/CSS_media_queries/Using_media_queries)を使用すると、一連の検査（ユーザーの画面が特定の幅より大きいかどうか、または特定の解像度かどうかなど）を実行し、 CSS を選択的に適用して、ユーザーのニーズに合わせてページを適切にスタイル設定することができます。

例えば、次のメディアクエリーは、現在のウェブページが画面メディアとして表示され（したがって印刷文書ではない）、ビューポートの幅が `80rem` 以上であるかどうかを検査します。 `.container` セレクターの CSS は、これら 2 つのことが当てはまる場合にのみ適用されます。

```css
@media screen and (min-width: 80rem) {
  .container {
    margin: 1em 2em;
  }
}
```

スタイルシート内に複数のメディアクエリーを追加して、レイアウト全体またはその一部をさまざまな画面サイズに最適に調整することができます。メディアクエリーが導入されてレイアウトが変更されるポイントは、_ブレークポイント_ (breakpoints) と呼ばれます。

メディアクエリーを使用する場合の一般的なアプローチは、狭い画面の端末（携帯電話など）用に単純な単一列レイアウトを作成し、次に、より大きな画面であるかチェックして複数列レイアウトを処理するのに十分な画面幅があることがわかったら、複数列レイアウトを実装することです。これは多くの場合、**モバイルファースト**デザインと呼ばれます。

ブレークポイントを使用する場合、ベストプラクティスとして、個々の端末の絶対サイズではなく、[相対的な単位](/ja/docs/Learn_web_development/Core/Styling_basics/Values_and_units#相対長の単位)でメディアクエリーのブレークポイントを定義することを推奨します。

メディアクエリーブロック内で定義するスタイルにはさまざまなアプローチがあります。ブラウザーのサイズの範囲に基づいてスタイルシートの {{htmlelement("link")}} にメディアクエリーを使用するものから、カスタムプロパティ変数に、それぞれのブレークポイントに関連する値を格納するだけのものまであります。

メディアクエリーは RWD で役立ちますが、必須のものではありません。柔軟なグリッド、相対的な単位、最小・最大の単位の値は、メディアクエリーを使用せずに使用することができます。

## レスポンシブレイアウト技術

レスポンシブサイトは柔軟なグリッド上に構築されているため、ピクセル単位で正確なレイアウトで可能な限りの端末サイズをすべて対象とする必要はありません。

柔軟なグリッドを使用することで、特性を変更したりブレークポイントを追加して、コンテンツの見てくれが悪くなり始めた時点でデザインを変更することができます。例えば、画面サイズが大きくなっても行の長さが読めないほど長くならないようにするには、{{cssxref('columns')}} を使用することができます。ボックスが狭くなって行あたりに 2 つしか単語が表示されなくなったら、ブレークポイントを設定します。

[フレックスボックス](/ja/docs/Learn_web_development/Core/CSS_layout/Flexbox)や [CSS グリッド](/ja/docs/Learn_web_development/Core/CSS_layout/Grids)などの複数のレイアウト方式は、既定でレスポンシブです。これらはすべて、柔軟なグリッドを作成しようとしていることを想定しており、そのための簡単な方法を提供しています。

### フレックスボックス

フレックスボックス (Flexbox) では、フレックスアイテムは縮小したり拡大したりして、コンテナーの空間に応じてアイテム間の空間を分配します。`flex-grow` と `flex-shrink` の値を変更することで、アイテムの周りの空間が広くなったり狭くなったりしたときにどのように動作させたいかを示すことができます。

次の例では、`flex: 1` という一括指定を使用して、フレックスアイテムがフレックスコンテナー内でそれぞれ同じ空間の大きさにしています。レイアウトのトピック「[フレックスボックス: フレックスアイテムの柔軟なサイズ変更](/ja/docs/Learn_web_development/Core/CSS_layout/Flexbox#フレックスアイテムの柔軟なサイズ変更)」で記述されている通りです。

```css
.container {
  display: flex;
}

.item {
  flex: 1;
}
```

レスポンシブデザインでメディアクエリーとフレックスボックスを使用する方法を紹介します。

```html-nolint live-sample___flex-based-rwd
<div class="wrapper">
  <div class="col1">
    <p>
      このレイアウトはレスポンシブです。ブラウザーウィンドウの幅を広げたり狭めたりしたら何が起こるかを確認してください。
    </p>
  </div>
  <div class="col2">
    <p>
      1782 年 11 月のある夜、 2 人の兄弟がフランスの小さな町アノネーで冬の暖炉の火を囲み、暖炉から立ち上る灰色の煙が広い煙突を登っていくのを見ながら、話を実行したという話があります。彼らの名前はステファンとジョセフ・モンゴルフィエで、紙職人でした。また、思慮深い頭脳を持ち、科学的な知識と新しい発見すべてに深い関心を持っていることで知られていました。
    </p>
    <p>
      その夜（後に証明されるように、記憶に残る夜）の何百万人もの人々は、発行される煙の輪を眺めていましたが、その事実から何か特別なインスピレーションを何も感じていませんでした。
    </p>
  </div>
</div>
```

```css hidden live-sample___flex-based-rwd
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: #fff;
}
```

```css live-sample___flex-based-rwd
@media screen and (min-width: 600px) {
  .wrapper {
    display: flex;
  }

  .col1 {
    flex: 1;
    margin-right: 5%;
  }

  .col2 {
    flex: 2;
  }
}
```

{{EmbedLiveSample("flex-based-rwd", "", "550px")}}

画面のサイズを変更してみてください。上記の例のサイズが 600px 幅の閾値を超えるとレイアウトが変更されます。

### CSS グリッド

CSS グリッドレイアウトでは、`fr` 単位を使用して、利用可能な空間をそれぞれのグリッドトラックに分配することができます。次の例では、サイズが `1fr` の 3 つのトラックを持つグリッドコンテナーを作成します。これにより、3 つの列トラックが作成され、それぞれがコンテナー内の使用可能な空間の一部を占めます。 グリッドを作成するこのアプローチの詳細については、CSS レイアウトのグリッドのトピックの [fr 単位での柔軟なグリッド](/ja/docs/Learn_web_development/Core/CSS_layout/Grids#fr_単位での柔軟なグリッド)を参照してください。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

Here's how we could use grid layout with a media query for responsive design.

```html-nolint live-sample___grid-based-rwd
<div class="wrapper">
  <div class="col1">
    <p>
      このレイアウトはレスポンシブです。ブラウザーウィンドウの幅を広げたり狭めたりしたら何が起こるかを確認してください。
    </p>
  </div>
  <div class="col2">
    <p>
      1782 年 11 月のある夜、 2 人の兄弟がフランスの小さな町アノネーで冬の暖炉の火を囲み、暖炉から立ち上る灰色の煙が広い煙突を登っていくのを見ながら、話を実行したという話があります。彼らの名前はステファンとジョセフ・モンゴルフィエで、紙職人でした。また、思慮深い頭脳を持ち、科学的な知識と新しい発見すべてに深い関心を持っていることで知られていました。
    </p>
    <p>
      その夜（後に証明されるように、記憶に残る夜）の何百万人もの人々は、発行される煙の輪を眺めていましたが、その事実から何か特別なインスピレーションを何も感じていませんでした。
    </p>
  </div>
</div>
```

```css hidden live-sample___grid-based-rwd
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

.col1,
.col2 {
  background-color: #fff;
}
```

```css live-sample___grid-based-rwd
@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("grid-based-rwd", "", "550px")}}

## レスポンシブ画像とメディア

メディアがそのコンテナーより大きくならないことを保証するには、次の方法が利用できます。

```css
img,
picture,
video {
  max-width: 100%;
}
```

これは、メディアがコンテナーからあふれないように確実に拡大縮小します。

> [!NOTE]
> 単一の大きな画像を使用し、それを小さな端末に合うように変倍すると、必要以上に大きな画像をダウンロードすることになり、帯域幅が無駄になります。また、見た目も悪くなります。例えば、ワイド画面のモニターでは横長の画像が適していますが、モバイル端末では縦向きの画像の方がよく合う可能性があるからです。このような問題は、 {{htmlelement("picture")}} 要素と、 {{htmlelement("img")}} の `srcset` および `sizes` 属性を使用することで解決できます。これらは高度な機能であり、このコースの対象範囲を超えるものですが、[レスポンシブ画像](/ja/docs/Web/HTML/Guides/Responsive_images)に詳細なガイドがあります。

その他の有用なコツです。

- ウェブサイトの画像には常に適切な画像形式（PNG や JPG など）を使用し、ウェブサイトに掲載する前にグラフィックエディターを使ってファイルサイズを最適化してください。
- [グラデーション](/ja/docs/Web/CSS/CSS_images/Using_CSS_gradients)や[影](/ja/docs/Web/CSS/box-shadow)のような CSS の機能を使用することで、画像を使用せずに視覚効果を実装することができます。
- {{htmlelement("video")}}/{{htmlelement("audio")}} 要素の中にある {{htmlelement("source")}} 要素の media 属性内では、メディアクエリーを使用することができ、様々な端末に最適な映像/音声ファイルを提供することができます。

## レスポンシブ書体

レスポンシブ書体は、メディアクエリー内やビューポート単位を使用してフォントサイズを変更し、画面の面積の大小を反映する記述です。

### レスポンシブ書体のためにメディアクエリーを使用

この例では、レベル 1 の見出しを `4rem` に設定します。 つまり、基本フォントサイズの 4 倍になります。それは本当に大きな見出しです。このジャンボ見出しは大きな画面サイズでのみ使用します。 したがって、まず小さな見出しを作成し、ユーザーが少なくとも `1200px` の画面サイズを持っていることがわかった場合は、メディアクエリーを使用して大きな見出しで上書きします。

```css
html {
  font-size: 1em;
}

h1 {
  font-size: 2rem;
}

@media (min-width: 1200px) {
  h1 {
    font-size: 4rem;
  }
}
```

上記のレスポンシブグリッドの例を編集して、説明した方法を使用してレスポンシブ書体も含めました。 レイアウトが 2 段バージョンになると、見出しのサイズがどのように切り替わるかを確認できます。

モバイル端末では見出しが小さくなりますが、デスクトップでは見出しが大きなサイズになります。

```html-nolint live-sample___type-rwd
<div class="wrapper">
  <div class="col1">
    <h1>このサイズを見てください</h1>
    <p>
      このレイアウトはレスポンシブです。ブラウザーウィンドウの幅を広げたり狭めたりしたら何が起こるかを確認してください。
    </p>
  </div>
  <div class="col2">
    <p>
      1782 年 11 月のある夜、 2 人の兄弟がフランスの小さな町アノネーで冬の暖炉の火を囲み、暖炉から立ち上る灰色の煙が広い煙突を登っていくのを見ながら、話を実行したという話があります。彼らの名前はステファンとジョセフ・モンゴルフィエで、紙職人でした。また、思慮深い頭脳を持ち、科学的な知識と新しい発見すべてに深い関心を持っていることで知られていました。
    </p>
    <p>
      その夜（後に証明されるように、記憶に残る夜）の何百万人もの人々は、発行される煙の輪を眺めていましたが、その事実から何か特別なインスピレーションを何も感じていませんでした。
    </p>
  </div>
</div>
```

```css live-sample___type-rwd
html {
  font-size: 1em;
}

body {
  font:
    1.2em Helvetica,
    Arial,
    sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}
.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

h1 {
  font-size: 2rem;
  margin: 0;
}

.col1,
.col2 {
  background-color: #fff;
}

@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }

  h1 {
    font-size: 4rem;
  }
}
```

{{EmbedLiveSample("type-rwd", "", "550px")}}

この書体へのアプローチが示すように、メディアクエリーをページのレイアウトの変更のみに制限する必要はありません。 これらを使用して任意の要素を微調整し、代わりとなる画面サイズでより使いやすく魅力的にすることができます。

### レスポンシブ書体にビューポート単位を使用

興味深いアプローチは、ビューポート単位 `vw` を使用してレスポンシブ書体を有効にすることです。 `1vw` はビューポートの幅の 1% に等しいため、`vw` を使用してフォントサイズを設定すると、常にビューポートのサイズに関連付けられます。

```css
h1 {
  font-size: 6vw;
}
```

上記の問題は、`vw` 単位を使用するとユーザーがテキストをズームする機能を失うことです。 そのテキストは常にビューポートのサイズに関連しているためです。**したがって、ビューポート単位を単独で使用してテキストを設定しないでください**。

[`calc()`](/ja/docs/Web/CSS/calc) を使用するという解決策があります。 `em` や `rem` などの固定サイズを使用した値に `vw` 単位を加算しても、テキストはズーム可能です。次のように基本的に、`vw` 単位はズームした値に加算します。

```css
h1 {
  font-size: calc(1.5rem + 4vw);
}
```

これは、見出しのフォントサイズを指定する必要があるのは一度だけで、モバイル用にメディアクエリーで再定義せずともよいことを意味します。ビューポートのサイズを大きくするにつれて、フォントは徐々に大きくなります。

```html-nolint live-sample___type-vw
<div class="wrapper">
  <div class="col1">
    <h1>このサイズを見てください</h1>
    <p>
      このレイアウトはレスポンシブです。ブラウザーウィンドウの幅を広げたり狭めたりしたら何が起こるかを確認してください。
    </p>
  </div>
  <div class="col2">
    <p>
      1782 年 11 月のある夜、 2 人の兄弟がフランスの小さな町アノネーで冬の暖炉の火を囲み、暖炉から立ち上る灰色の煙が広い煙突を登っていくのを見ながら、話を実行したという話があります。彼らの名前はステファンとジョセフ・モンゴルフィエで、紙職人でした。また、思慮深い頭脳を持ち、科学的な知識と新しい発見すべてに深い関心を持っていることで知られていました。
    </p>
  </div>
</div>
```

```css live-sample___type-vw
body {
  font: 1.2em / 1.5 sans-serif;
  margin: 20px;
  padding: 0;
  background-color: #eee;
}

.wrapper {
  max-width: 960px;
  margin: 2em auto;
}

h1 {
  font-size: calc(1.5rem + 4vw);
  margin: 0;
}

.col1,
.col2 {
  background-color: #fff;
}

@media screen and (min-width: 600px) {
  .wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    column-gap: 5%;
  }
}
```

{{EmbedLiveSample("type-vw", "", "550px")}}

## ビューポートメタタグ

レスポンシブなページの HTML ソースを見ると、通常、次の {{htmlelement("meta")}} タグが文書の `<head>` の中にあります。

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

この[ビューポート](/ja/docs/Web/HTML/Guides/Viewport_meta_element)メタタグは、モバイルブラウザーに、ビューポートの幅を端末の幅に設定し、文書を意図したサイズの 100% にスケーリングするよう指示します。 これにより、文書はモバイル向けに最適化されたサイズで表示されます。

なぜこれが必要なのでしょうか？ モバイルブラウザーは、ビューポートの幅について嘘をつく傾向があるためです。

このメタタグが存在するのは、スマートフォンが登場した当初、ほとんどのサイトがモバイルに最適化されていなかったからです。そのため、モバイルブラウザーはビューポート幅を 980 ピクセルに設定し、その幅でページをレンダリングし、その結果をデスクトップレイアウトの拡大版として表示していました。ユーザーはウェブサイトを拡大したり、パンしたりして、関心のある部分を見ることができましたが、見た目が悪いものでした。

`width=device-width` を設定することで、モバイル端末の既定値（Apple の既定値 `width=980px` など）を実際の端末の幅で上書きすることになります。これがないと、ブレークポイントやメディアクエリーを使ったレスポンシブデザインがモバイルブラウザーで意図通りに動作しない可能性があります。ビューポートの幅が 480px 以下で動作する狭い画面レイアウトを取得していても、端末が 980px の幅だと言えば、そのユーザーには狭い画面用のレイアウトは表示されません。

**したがって、ビューポートメタタグを文書の先頭に常に含める必要があります**。

## まとめ

レスポンシブデザインとは、閲覧環境に応答するサイトやアプリケーションのデザインを指します。CSSと HTML の機能やテクニックを包括し、既定値ではこれでウェブサイトを構築することができます。携帯電話で閲覧するサイトを考えてみましょう。デスクトップ版を縮小したサイトや、ものを探すのに横スクロールが必要なサイトに出会うのは、かなり珍しいことでしょう。これは、ウェブがレスポンシブデザインという手法に移行されたからです。

また、これらのレッスンで学んだレイアウト方法の助けを借りて、レスポンシブデザインを実現することがはるかに容易になりました。今日、ウェブ開発を新しく始めた方は、レスポンシブデザインの初期の頃よりも自由に使えるツールがたくさんあります。そのため、使用している素材の古さを調べる価値があります。過去の記事は今でも有用ですが、現代の CSS と HTML を使用することで、訪問者がどんな端末でサイトを閲覧しても、エレガントで有益なデザインをはるかに簡単に作成することができます。

次に、メディアクエリーについてさらに詳しく検討し、よくある問題の解決に役立てる方法を紹介します。

## 関連情報

- タッチ画面端末での作業:
  - [タッチイベント](/ja/docs/Web/API/Touch_events)は、タッチ画面やトラックパッド上の指（またはスタイラス）の動きを解釈する機能を提供し、複雑なタッチベースのユーザーインターフェイスの高品質な対応を可能にします。
  - [pointer](/ja/docs/Web/CSS/@media/pointer) または [any-pointer](/ja/docs/Web/CSS/@media/any-pointer) メディアクエリーを使用すると、タッチ対応端末で異なる CSS を読み込むことができます。
- [CSS-Tricks guide to media queries](https://css-tricks.com/a-complete-guide-to-css-media-queries/)

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/CSS_layout")}}
