# Hopscotch 日本語ドキュメント

本ドキュメントは [Hopscotch](http://linkedin.github.io/hopscotch/) のドキュメントを日本語に翻訳したものです。  
原文は [hopscotch](https://github.com/linkedin/hopscotch) にあります。


Hopscotch
=========

Hopscotch は開発者が簡単にプロダクトツアーをページに追加できるようになるフレームワークです。 Hopscotch は入力としてツアー JSON オブジェクトを受け取り、ツアー画面のレンダリング、ツアーの進行状況の管理を行なう開発者向けの API を提供します。百聞は一見に如かず、[デモ](http://linkedin.github.io/hopscotch)を見てみましょう！


特徴
========

* ツアーイベントのコールバック（例： OnStart 、 onEnd 、 onShow 、 onNext 、 onPrev 、 OnClose ）
* フォールバックとしてクッキーを使う HTML5 sessionStorage を使った複数ページの永続化
* I18N のサポート
* 軽量で単一な吹き出し


一般的な使い方
===============

Hopscotch フレームワークを使うには、 `hopscotch.css` と `hopscotch.js` をページ上に含めるだけです。次はグローバルの window オブジェクトに Hopscotch オブジェクトをロードしています。

```html
<html>
  <head>
    <title>My First Hopscotch Tour</title>
    <link rel="stylesheet" href="css/hopscotch.css"></link>
  </head>
  <body>
    <h1 id="header">My First Hopscotch Tour</h1>
    <div id="content">
      <p>Content goes here...</p>
    </div>
    <script src="js/hopscotch.js"></script>
    <script src="js/my_first_tour.js"></script> <!-- define and start your tour in this file -->
  </body>
</html>
```

次に、 `my_first_tour.js` ファイルを定義してツアーを開始します。

```javascript
// Define the tour!
var tour = {
  id: "hello-hopscotch",
  steps: [
    {
      title: "My Header",
      content: "This is the header of my page.",
      target: "header",
      placement: "right"
    },
    {
      title: "My content",
      content: "Here is where I put my content.",
      target: document.querySelector("#content p"),
      placement: "bottom"
    }
  ]
};

// Start the tour!
hopscotch.startTour(tour);
```

とても簡単です！


ツアーの定義
===============

Hopscotch ツアーは、ツアー id 、 JSON オブジェクトとして定義されたツアーの steps 配列、および、いくつかのツアー固有のオプションで構成されています。ツアー id は単なるユニークな識別子となる文字列です。最も簡単なツアーは、 id と1つ以上のステップの配列だけで構成されています。

基本的なステップオプション
---------------------------

以下のステップオプションは、最も基本的なオプションです。

```javascript
{
  id: {STRING - id of the tour},
  steps: [
    {
      target:         STRING/ELEMENT - id of the target DOM element or DOM element itself,
      placement:      STRING         - ["top", "bottom", "right", "left"]
      title:          STRING         - step title,
      content:        STRING         - step content
    },
    ...
  ]
}
```

title のみ、 content のみ、または title と content の両方を持ったステップを選ぶことができます。このため、 title と content は任意であることに注意してください。

次は、基本的なステップだけで定義されたツアーの一例です。

```javascript
{
  id: "welcome_tour",
  steps: [
    {
      target: "header",
      placement: "bottom",
      title: "This is the navigation menu",
      content: "Use the links here to get around on our site!"
    },
    {
      target: "profile-pic",
      placement: "right",
      title: "Your profile picture",
      content: "Upload a profile picture here. This is the image that others will see next to your activity."
    },
    {
      target: "inbox",
      placement: "bottom",
      title: "Your inbox",
      content: "Messages from other users will appear here."
    }
  ]
}
```

**重要** -- `element.innerHTML` を使用して title と content を設定する場合、リンクやリストのような非常に基本的なマークアップを含めることができます。しかし、それは不適切に使用され、悪意のあるスクリプトインジェクションを含めることもできます。そのため、ユーザー生成のコンテンツを Hopscotch ツアーに表示しないことを強くお勧めします。それが絶対に必要な場合は、入力を適切にエスケープする必要があります。

すべてのステップオプション
----------------

ステップオプションの包括的なリストは以下の通りです：

### 必須のオプション ###

* `target` [STRING / ELEMENT / ARRAY] - ターゲットのDOM要素の id またはDOM要素自体。いくつかのターゲットの配列を定義することも可能です。配列の場合、 Hopscotch はページ上に存在する最初のターゲットを使用し、残りの部分は無視します。

* `placement` [STRING] - 吹き出しがターゲットのどの位置に表示されるかを指定します。有効な値は "top" 、 "bottom" 、 "right" 、 "left" です。

### 任意のオプション ###

* `title` [STRING] - ステップのタイトル。 title は任意ですが、少なくとも title か content のいずれかが存在しなければならないことに注意してください。

* `content` [STRING] - ステップの内容。 content は任意ですが、少なくとも title か content のいずれかが存在しなければならないことに注意してください。

* `width` [INT] - 吹き出しの幅

* `padding` [INT] - 吹き出しのパディング

* `xOffset` [INT] - 吹き出しの水平方向の位置調整。値はピクセル数または "center" が指定できます。*デフォルト値*： 0。

* `yOffset` [INT] - 吹き出しの垂直方向の位置調整。値はピクセル数または "center" が指定できます。*デフォルト値*： 0。

* `arrowOffset` [INT] - 吹き出しの矢印のオフセット。値はピクセル数または "center" が指定できます。*デフォルト値*： 0。

* `delay` [INT] - ステップを表示する前に待機するミリ秒数。*デフォルト値*： 0。

* `zindex` [INT] - 吹き出しの z-index を設定します。

* `showNextButton` [BOOLEAN] - next ボタンを表示します。*デフォルト値*： true 。

* `showPrevButton` [BOOLEAN] - prev ボタンを表示します。*デフォルト値*： true 。

* `showCTAButton` [BOOLEAN] - call-to-action ボタンを表示します。これはどんな目的にも使用できるカスタムボタンです。詳細は onCTA オプションを参照してください。*デフォルト値*： false 。

* `ctaLabel` [STRING] - call-to-action ボタンのラベル。

* `multipage` [BOOLEAN] - 次のステップが別のページにあることを示します。*デフォルト値*： false 。

* `showSkip` [BOOLEAN] - true の場合、'Next' ボタンが 'Skip' ボタンになります。*デフォルト値*： false 。

* `fixedElement` [BOOLEAN] - ターゲット要素の position が fixed の場合は、 true に設定します。*デフォルト値*： false 。

* `nextOnTargetClick` [BOOLEAN] - ターゲットをクリックすると nextStep() をトリガーします。*デフォルト値*： false 。

* `onPrev` [FUNCTION] - 'Previous' ボタンがクリックされたときのコールバック

* `onNext` [FUNCTION] - 'Next' ボタンがクリックされたときのコールバック

* `onShow` [FUNCTION] - ステップが最初に表示されたときのコールバック

* `onCTA` [FUNCTION] - 任意の call-to-action ボタンのコールバック

ツアーオプションの設定
--------------------

ツアー JSON オブジェクトか、 hopscotch.configure（） の呼び出しを通して、ツアーオプションを指定することができます。これらのオプションはツアー全体に適用されます。ツアーのオプションとステップの定義の両方（例えば "showPrevButton" ）で指定された値がある場合、ステップの定義が優先されます。複数のコールバックがステップやツアーオプションの両方で定義されている場合、ステップのコールバックはツアー全体のコールバックの前に呼び出されます。

* `id` [STRING] - *必須*。ツアーのためのユニークな識別子の文字列。状態を維持するために使用します。

* `bubbleWidth` [NUMBER] - デフォルトの吹き出しの幅。*デフォルト値*： 280。

* `bubblePadding` [NUMBER] - デフォルトの吹き出しのパディング。*デフォルト値*： 15。

* `smoothScroll` [BOOLEAN] - 次のステップへのページスクロールをスムーズにする必要がありますか？*デフォルト値*： true 。

* `scrollDuration` [NUMBER] - ミリ秒単位でのページスクロールの時間。 smoothScroll が true に設定されている場合のみ有効。*デフォルト値* ： 1000。

* `scrollTopMargin` [NUMBER] - ページをスクロールしたときに、吹き出しや targetElement と Viewport の上端との間にどのくらいのスペースが必要ですか？*デフォルト値* ： 200。

* `showCloseButton` [BOOLEAN] - ツアーの吹き出しの近くに （X） ボタンを表示する必要がありますか？*デフォルト値*： true 。

* `showPrevButton` [BOOLEAN] - 吹き出しは prev ボタンを持っている必要がありますか？*デフォルト値*： false 。

* `showNextButton` [BOOLEAN] - 吹き出しは next ボタンを持っている必要がありますか？*デフォルト値*： true 。

* `arrowWidth` [NUMBER] - デフォルトの矢印の幅（吹き出しと targetEl の間のスペース）。吹き出しの位置計算のために使用。このオプションは開発者が矢印の大きさを調整するカスタム CSS を使用するために設けられている。*デフォルト値*： 20。

* `skipIfNoElement` [BOOLEAN] - 指定されたターゲット要素が見つからなかった場合、次のステップに進む必要がありますか？*デフォルト値*： true 。

* `nextOnTargetClick` [BOOLEAN] - ユーザーがターゲットをクリックするとき、次のステップに進めるべきですか？*デフォルト値*： false 。

* `onNext` [FUNCTION] - "Next" ボタンをクリックしてすべての後に呼び出されます。

* `onPrev` [FUNCTION] - "Prev" ボタンをクリックしてすべての後に呼び出されます。

* `onStart` [FUNCTION] - ツアーが開始されたときに呼び出されます。

* `onEnd` [FUNCTION] - ツアーが終了したときに呼び出されます。

* `onClose` [FUNCTION] - ユーザーがツアーを閉じたときに終了する前に呼び出されます。

* `onError` [FUNCTION] - 指定されたターゲット要素がページ上に存在しないときに呼び出されます。

* `i18n` [OBJECT] - i18n のため。ボタンラベルのテキストとステップ番号を変更することができます。

* `i18n.nextBtn` [STRING] - next ボタンのラベル

* `i18n.prevBtn` [STRING] - prev ボタンのラベル

* `i18n.doneBtn` [STRING] - done ボタンのラベル

* `i18n.skipBtn` [STRING] - skip ボタンのラベル

* `i18n.closeTooltip` [STRING] - close ボタンツールチップのテキスト

* `i18n.stepNums` [ARRAY] - 配列のインデックス順に、ステップ番号として表示される文字列のリストを提供します。Unicode がサポートされています（例えば、['一'、'二'、'三']）。提供された番号よりもステップが多い場合は、デフォルトとしてアラビア数字（'4'、'5'、'6'、など）が使用されます。

 
API メソッド
===========

Hopscotch フレームワークには、ツアーの実行と管理ができる API 呼び出しの簡単なセットが付属しています：

* `hopscotch.startTour(tour[, stepNum])` - 実際にツアーを開始します。オプションの引数 stepNum はどのステップから開始かを指定します。

* `hopscotch.showStep(idx)` - ツアーで指定されたステップにスキップします

* `hopscotch.prevStep()` - ツアーで一つ前のステップに戻る

* `hopscotch.nextStep()` -ツアーで一つ次のステップに進む

* `hopscotch.endTour([clearState])` - 現在のツアーを終了します。 clearState が false に設定されている場合、ツアーの状態は保持されます。それ以外の場合は、デフォルトでツアーの状態はクリアされます。

* `hopscotch.configure(options)` - ツアーを実行するためのオプションを設定します。注意：ツアーをロードした後に、このメソッドが呼び出された場合、指定されたオプションはツアーに定義されたオプションを上書きします。設定できるオプションのリストについては、上の"ツアーオプションの設定"を参照してください。

* `hopscotch.getCurrTour()` - 現在実行中のツアーを返します。

* `hopscotch.getCurrStepNum()` - 現在実行中のツアーのステップ数（ゼロベース）を返します。

* `hopscotch.getState()` - sessionStorage またはクッキーに保存された状態をチェックし、存在する場合はその状態を返します。ツアーを再開するかどうかの判断をするためにこのメソッドを使用します。

* `hopscotch.listen(eventName, callback)` - いずれかのイベントタイプのコールバックを追加します。有効なイベントタイプは次のとおりです： *start* 、 *end* 、 *next* 、 *prev* 、 *show* 、 *close* 、 *error*

* `hopscotch.unlisten(eventName, callback)` - いずれかのイベントタイプのコールバックを削除します。

* `hopscotch.removeCallbacks(eventName[, tourOnly])` - Hopscotch イベントのコールバックを削除します。tourOnly が true に設定されている場合は、ツアーによって指定されたコールバックのみを削除します（ hopscotch.configure または hopscotch.listen によって設定されたコールバックは残ります）。eventName が null または undefined の場合は、すべてのイベントのコールバックが削除されます。

* `hopscotch.registerHelper(id, fn)` - コールバックヘルパーを登録します。ヘルパーについては以下のセクションを参照してください。

* `hopscotch.unregisterHelper(id)` - コールバックヘルパーの登録を解除します。ヘルパーについては以下のセクションを参照してください。

* `hopscotch.resetDefaultI18N()` - i18n の文字列を元のデフォルト値にリセットします。

* `hopscotch.resetDefaultOptions()` - 設定したすべてのオプションを元の値にリセットします。

 
 コールバックの定義
==================

Hopscotch はコールバックを割り当てることができるいくつかのイベントを持っています。これらのイベントには、 *start* 、 *end* 、 *next* 、 *prev* 、 *show* 、 *close* 、 *error* があります。 *next* 、 *prev* および *show* イベントは、ステップ定義内だけでなく、ツアー自体にコールバックを割り当てることができます。

イベントコールバックを定義する方法は2つあります：

関数リテラル
-----------------

JavaScript でオブジェクトリテラルとしてツアーを指定する場合は、コールバックの値として関数リテラルを提供することができます。これは次のようになります：

```javascript
var tour = {
  id: 'myTour',
  steps: [
    {
      target: 'firststep',
      placement: 'bottom',
      title: 'My First Step',
      onNext: function() {
        $('#firststep').hide();
      }
    }
  ],
  onStart: function() {
    $('#article').addClass('selected');
  }
};
```

コールバックヘルパー
----------------

いくつかの状況では、 JSON でツアーを指定することもできます。これは動的にサーバー上でツアーを作成しているからです。関数が有効な JSON ではないとき、関数リテラルを指定しても動作しません。その代わりに、コールバックを指定する Hopscotch ヘルパーを使用することができます。ヘルパーを使用すると、次のようになります。

最初に Hopscotch にヘルパーを登録します。

```javascript
hopscotch.registerHelper('selectArticle', function() {
  $('#article').addClass('selected');
});
```

渡された引数を使用した例：

```javascript
hopscotch.registerHelper('fillText', function(textFieldId, textStr) {
  document.getElementById(textFieldId).value = textStr;
});
```

この時、ツアーを定義するには、次の形式の配列としてコールバックを指定します： `[helperId, arg, arg, ...]`。例えば：

```javascript
{
  id: "myTour",
  steps: [
    {
      target: "firststep",
      placement: "bottom",
      title: "My First Step",
      onNext: ["fillText", "searchField", "Example search query"]
    }
  ],
  onStart: ["selectArticle"]
}
```

上記の例では、 OnStart コールバックは引数を持っていないので、一つの要素の配列にラップされる代わりに、単純な文字列  "selectArticle" として定義することができます。

特定のイベントのためのいくつかのヘルパーを指定するには：

```javascript
{
  id: "myTour",
  steps: [
    ...
  ],
  onStart: [["fillText", "searchField", "Example search query"], "selectArticle"]
}
```

コールバックは指定された順序で呼び出されます。

ヘルパーを削除するには、単に `hopscotch.unregisterHelper('myHelperId')` を呼び出します。


ツアーの例
============

Hopscotch ツアーの例です。

```javascript
{
  id: "hello-hopscotch",
  steps: [
    {
      target: "hopscotch-title",
      title: "Welcome to Hopscotch!",
      content: "Hey there! This is an example Hopscotch tour. There will be plenty of time to read documentation and sample code, but let\'s just take some time to see how Hopscotch actually works.",
      placement: "bottom",
      arrowOffset: 60
    },
    {
      target: document.querySelectorAll("#general-usage code")[1],
      title: "Where to begin",
      content: "At the very least, you\'ll need to include these two files in your project to get started.",
      placement: "right",
      yOffset: -20
    },
    {
      target: "my-first-tour-file",
      placement: "top",
      title: "Define and start your tour",
      content: "Once you have Hopscotch on your page, you\'re ready to start making your tour! The biggest part of your tour definition will probably be the tour steps."
    },
    {
      target: "start-tour",
      placement: "right",
      title: "Starting your tour",
      content: "After you\'ve created your tour, pass it in to the startTour() method to start it.",
      yOffset: -25
    },
    {
      target: "basic-options",
      placement: "top",
      title: "Basic step options",
      content: "These are the most basic step options: <b>target</b>, <b>title</b>, <b>content</b>, and <b>placement</b>. For some steps, they may be all you need.",
      arrowOffset: 120,
      xOffset: 100
    }
  ],
  showPrevButton: true,
  scrollTopMargin: 100
}
```


Hopscotch の吹き出し
==================

時には、シンプルな吹き出しが必要な場合があります。これを達成するために Hopscotch Callout を使用することができます。

```javascript
var calloutMgr = hopscotch.getCalloutManager();
calloutMgr.createCallout({
  id: 'attach-icon',
  target: 'attach-btn',
  placement: 'bottom',
  title: 'Now you can share images &amp; files!',
  content: 'Share a project you\'re proud of, a photo from a recent event, or an interesting presentation.'
});
```

吹き出しには、 width 、 placement 、 offset 、 z-index のようなものを指定することもできるため、ツアーのステップとして使用できる同じオプションが付属しています。ツアーステップと吹き出しの最も重要な違いは、コールアウトを作成するときは後で参照するために `id` を提供する*必要がある*ことです。

吹き出しのすべての管理は、 Hopscotch Callout Manager を介して行われます。吹き出しの管理の仕事は非常に単純であり、ほんの一握りの API メソッドが付属しています。

* `calloutMgr.createCallout(options)` - id によって参照される吹き出しを作成します。オプションは該当するツアーステップのオプションの場合と同じです。
* `calloutMgr.getCallout(id)` - 指定された id の callout オブジェクトを返します。
* `calloutMgr.removeCallout(id)` - ページから指定された id を持つ吹き出しを削除します。
* `calloutMgr.removeAllCallouts()` - ページから登録されたすべての吹き出しを削除します。

