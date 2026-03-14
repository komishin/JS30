# JavaScript30 学習記録

## Day 10: JS Checkbox Challenge! (2026/03/14)
## 学んだこと

### 複数の項目を消したいときに活用できるコード

- inputの次のtypeがなくても良さそうだが、CheckBoxを指定していたほうが丁寧

- すべてのチェックボックスに対して...
  checkboxes.forEach(checkbox => {
  クリックされたら「handleCheck」という関数を動かしてね！と予約する
  checkbox.addEventListener('click', handleCheck);
});

- let lastCheckedで、letを使うのは、最後のチェックは常に変わる可能性があるため。

- let lastChecked;が関数の外にあることによって、毎回、最後のlastChecked = this;が変数
  内に導入され、前のクリックしたものが記憶される。
  (「グローバルスコープ（または親のスコープ）」で保持すると言う)
  ずっと覚えておきたいこと（前回のクリック場所など） → 関数の外
  その場限りの計算に使いたいこと（今から塗る範囲のフラグなど） → 関数の中

- e.shiftKey	Shiftキー が押されているか
  e.ctrlKey	Controlキー が押されているか
  e.altKey	Altキー（MacならOption）が押されているか
  e.metaKey	Commandキー（Macの⌘）が押されているか

- 最初は inBetween = false（オフ）
  まだ「範囲内」ではないので、誰もチェックしない。

  「最初にチェックした場所」に到着！
  ここで inBetween = !inBetween が実行され、false が true（オン） に切り替わる。

  そこから先は「範囲内」
  スイッチが true になっている間、通過するチェックボックスを次々と true（チェックあり）に書き換えていく。

  「今クリックした場所」に到着！
  ここで再び inBetween = !inBetween が実行され、true が false（オフ） に戻る。

それ以降は「範囲外」
スイッチが false に戻ったので、それより下にあるチェックボックスには何もしない。

- なぜ checkbox === this || checkbox === lastChecked なのか？
  この条件は、「ここがスタート地点、またはゴール地点ですか？」と聞いていることになる。

  this: 今クリックした場所

  lastChecked: 前回クリックして保存しておいた場所

  この2つのどちらかにぶつかるたびに、「ここからが範囲内だよ！」「ここでおしまいだよ！」と、スイッチをパチパチ切り替えている。

- checkboxes.forEach(checkbox => checkbox.addEventListener('click', handleCheck));
  が一番下にあるのは、シフトが押されなければ、上の条件は発生せず、シフトを押してときは、上のコード
  の条件で、このコードが実行されるようになるため。

- これは、まず最初は何もない状態ですとinBetweenをfalseの状態にしておいて
  次にもし、最初にチェックしたものかもしくは、最後にチェックしたものであったら
  inBetweenをtrueにして、
  if(inBetween){
    checkbox.checked = true;
  }
  のこーどでチェックを入れていき、次にもし最初にチェックしたものか最後にチェックしたものにきたら
  そこでinBetweenをfalseにして実行をやめなさいとしている。

- おまけ
  if 文の「省略形」というルール
  実は、プログラミングに慣れていない方はよくこのように書く

  これでも正解ですが、少し「丁寧すぎる」書き方
  if (inBetween === true) {
    checkbox.checked = true;
  }
  しかし、JavaScript（や他の多くの言語）では、「その変数が true かどうかを確認したいだけなら、変数名を書くだけでいい」 という決まりがある。
  if (inBetween) ＝ 「inBetween は true ですか？」と聞いている。
  if (!inBetween) ＝ 「inBetween は false ですか？」と聞いている。

- if(inBetween)とかくだけで、もしinBetweenがtrueのときと表現になる.


## Day 9:  Must Know Chrome Dev Tools Tricks (2026/03/14)
## 学んだこと

- %s：文字列（テキスト）用
  %d または %i：整数（数字）用
  %f：浮動小数点（小数点のある数字）用
  %o：オブジェクト用（中身を詳しく見たいとき）
  %c：CSS（スタイル）用（文字の色や大きさを変えたいとき）

- text-shadowは、今後使っていきたいコード！

- console.log開発用のメモ「変数の中身、今こうなってるよ（確認用）」
  console.info公式な報告「〇〇の処理が正常に終わったよ（報告用）」
  console.warn注意喚起「動くけど、このままだと危ないよ（黄色信号）」
  console.error異常事態「エラーで止まったよ！直して！（赤信号）」

- console.assert(1 === 2, 'That is wrong!');は、自分でアラートを埋め込める

- console.clear()は、個人開発のみ使うようにするべき
  (人の書いた注意事項まで消してしまうことになるから)

- HTMLの構造が合っているか見たいとき → console.log()
  JSで操作できる「値」や「名前」を調べたいとき → console.dir()

- console.dir(p) は「中身」を表示
  こちらはHTMLとしての見た目ではなく、その要素が持っている 膨大な「プロパティ（設定値）」のリスト
  を表示。
  クリックして中身を展開すると、以下のような情報がずらりと並んでいるのが見える。
  attributes: 設定されている属性
  classList: 設定されているクラスの一覧
  childNodes: 中に入っている子要素
  innerText / innerHTML: 中のテキスト
  style: 適用されているスタイル
  onclick: 設定されているイベント
  ...その他、数百種類のデータ

- console.groupCollapsed() という命令を使うと、最初から「折りたたまれた状態」で表示される
  ので、データ量が多いときはさらにコンソールがスッキリする。

- console.groupEnd(`${dog.name}`);がないと、次の情報が入れ子になってしまう

- 途中で数え直したい場合は console.countReset('wes') を使えば、また 1 から数え直す。

- console.time("fetching data") (計測開始)
  ストップウォッチのボタンをポチッと押すイメージです。引数の "fetching data" は、
  この計測に付けた「名前」。
  fetch(...) (実際の処理)
  GitHubのサーバーへデータをとりに行っています（これには時間がかかります）。
  console.timeEnd("fetching data") (計測終了)
  ストップウォッチを止めます。開始時と同じ名前を指定することで、その間の時間を計算してコンソールに
  表示する。

- then は、「（時間がかかる処理が）終わったら、次はこの作業をやってね！」という予約を意味する。

## Day 8: Let's build something fun with HTML5 Canvas (2026/03/13)
## 学んだこと

- getContext('2d'): その場所で「2D（二次元）の絵筆」を使えるように準備する。
- 「3Dのバリバリしたグラフィックスを描きたい」となった場合は、getContext('webgl') と書く

- ctx.strokeStyle = '#BADA55';
  線の色を指定しています。#BADA55 は少し明るい黄緑色。

- ctx.lineJoin = 'round';
  線のつなぎ目の設定です。round にすることで角がカクカクせず丸みを帯びた滑らかな仕上がりになる。

- ctx.lineCap = 'round';
  線の端（描き始めと終わり）の設定です。ここも round にすることで、線の先端が丸くなります。

- ctx.lineWidth = 100;
  線の太さを100ピクセルに設定しています。かなり太いマジックのような筆になります。

- let isDrawing = false;
  「今、マウスを押して描いているか？」を判定するスイッチ（フラグ）です。クリックしている間
   だけ true にして描画するように制御します。

- let lastX = 0; / let lastY = 0;
 「最後に描いた場所*の座標（X, Y）を保存します。Canvasで線を引くには、「前回の位置から
  今の位置まで線を結ぶ」という処理を繰り返すため、この「前回の位置」を覚えておく必要があります

- mousedown : マウスのボタンを押した瞬間（描き始め）
- mousemove : マウスを動かしている間（描画中）
- mouseup   : マウスのボタンを離した瞬間（描き終わり）
- mouseout. : マウスがキャンバスの外に出た瞬間（中断）

- canvas.addEventListener('mousedown', (e) => {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
  });はマウスのクリックした場所を記憶するもの

- 「hue（ヒュー）」ってなに？
   プログラミングやデザインの世界では、色のことを HSL という3つの数字で表すことがよくあります。
   H (Hue)：色相（しきそう） ← これが今回の hue です！
   s (Saturation)：彩度（鮮やかさ）
   L (Lightness)：明度（明るさ）

- if (hue >= 360) {
    hue = 0;
  }
  if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
    direction = !direction;
  }

  if (direction) {
    ctx.lineWidth++;
    } else {
    ctx.lineWidth--;
  }
  は、
  最初は細いペンで描き始める。
  描きながら、だんだん太くなっていく。
  太さが「100」になった瞬間、「あ、太すぎ！」と思って、今度はだんだん細くしていく。
  太さが「1」になった瞬間、「消えちゃう！」と思って、また太くしていく。

## Day 7:.some().every(),.find()  [...SPREADS] — Array Cardio Day 2 (2026/03/13)
## 学んだこと

- .some()（サム）は、JavaScriptの合言葉で、「リストの中に、この条件に当てはまる人が一人でも
 （some）いたら、YES（true）と答えて！」という命令。
- new Date().getFullYear()は、今の年を取得
- everyは全員かどうか
- splice（スプライス）は、リストの途中の要素を切り取ったり入れ替えたりする強力な合言葉。
- comments.slice(0, index):
  「0番目（最初）」から「消したい場所（index）」の手前までを切り抜きます。
- comments.slice(index + 1):
  「消したい場所の次の番号（index + 1）」から「最後まで」を切り抜きます。



## Day 6: Ajax Type Ahead with fetch()   (2026/03/12)
## 学んだこと

## https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json
を使ってjsonファイルを取得

- fetchんは非同期
- 普通のプログラムは、何かが終わるまで次の仕事ができませんが、fetch は非同期という特別な仕組み
  を持っている
- blobもなんでもいい名前
- dataもなんでもいい名前
  ２つの定義が必要なのは、塊を取ってきて、バラバラに並べるというお決まりのパターンで「そうするんだ！しょうがないそう書こう！」と思っておく！
- .then(翻訳) : まだ「生のデータ」なので、JavaScriptが読める「JSON形式」に翻訳する。
- .then(実行) : 翻訳が終わったら、ようやくあなたの手元にデータが届く！
- newは、決まりごとで〇〇を作ってという意味
- RegExp は Regular Expression（正規表現） の略
- g (global): 「1つ見つけたら終わり」ではなく、「全部見つけてね！」という意味。
  i (insensitive): 「大文字と小文字を区別しないでね！」という意味。
  (これがあるおかげで、new york と打っても New York をちゃんと見つけてくれます。)
- const searchInput = document.querySelector('.search');
  const suggestions = document.querySelector('.suggestions');
  は、探して名前をつけるコード
- 'change' ってどんなとき？
   ここでいう 'change' は、検索ボックスに文字を打ち込んで、マウスで別の場所をクリックしたり、Enterキーを押したりして「入力が終わったよ！」となった瞬間のこと。
  もし「1文字打つたびにすぐ反応してほしい」ときは、'change' の代わりに 'keyup'（キーを離したとき）という言葉を使ったりします。
- value: 入力欄の中に書いてある「文字」を指す、世界共通の名前。
- this.value: 「今動かしているこの検索ボックスの、中身」という意味。
- matchArray: 都市データが入ったカゴ（例：[大分市, 別府市]）
- .map: 「中のデータを一つずつ取り出して、加工するよ！」
- place: 今取り出している「1つ分の都市データ」につけた一時的なあだ名
- join: バラバラのタグを「1つにつなげる」。
- innerHTML: 画面のリストに「ペタッと貼り付ける」。
- suggestions.innerHTML = html;は、suggestionというクラスの所に貼り付けなさいということ
- hl = highlightのこと
- return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
  x.toString(): 「数字の x を、文字に直してね」
  『文字列でないと、隙間ができない』
  replace(...): 「特定の場所を、カンマ（,）に置き換えてね」
  /\B(?=(\d{3})+(?!\d))/g: これが一番難しい「正規表現（せいきひょうげん）」という呪文。
  「数字を後ろから数えて、3つ並ぶたびにその隙間（すきま）を見つけて！」と命令している。


## Day 5: Flexbox + JavaScript Image Gallery  (2026/03/11)
## 学んだこと

### 上下から文字が出てくるように実装していく
### まず最初に画像が表示されないため、サービス中の画像を表示するように変更

- flex: 1;指定すると均等に画像が並ぶ
- flex: 5;上の５倍になる
- transition:
  font-size 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
  flex 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
  background 0.2s;
  は、フレックスと合わせるとふわっとした動きが可能になる
- const panels = document.querySelectorAll('.panel');
  はpanelというなまえのクラスを見つけて名前をつける
- e.propertyNameは、変化した要素という意味
  なので、変化したものにflexという文字があったらということになる
  (ここでは、flexを変化させている！コンソールするとわかる)
- toggleOpenは自分で定義したなまえ
- toggleActiveは自分で定義したなまえ
-  panels.forEach(panel => panel.addEventListener('transitionend', toggleActive));
  は開ききった動きが終わってから、toggleActiveを実行するという指示
  動きの段階をつけている
- panels.forEach(panel => panel.addEventListener('click', toggleOpen));
  panels.forEach(panel => panel.addEventListener('transitionend', toggleActive));
  のpanelはどんななまえでも良いがわかりやすくしているだけ
- あくまでも上から順に動いていくと考えるとわかりやすい

## Day 4: Cardio Practice (2026/03/09〜10)
## 学んだこと

### １行でかけるときは「if文は省略できる」

### ①
- const fifteen = inventors.filter((inventor) =>
    (inventor.year >= 1500 && inventor.year < 1600))
  console.table(fifteen);
- filterでinventorsの中のfifteenを探す処理
- tableを使うことによって、表の形で出力される


### ②
- mapは配列を受け取る

### ③
- sortは、比較するということ
- a と b は「特定の誰か」を指しているわけではなく、「sort メソッドが中身を1つずつ順番に取り出
  して比較する際に、そのペアに付ける一時的な名前」だとイメージすると一番しっくりくる。
- ここでは、年齢順に並べる指示

### ④
- reduceは、「たくさんの要素を、ルールに従って1つにまとめる役目」
- inventor = 発明家（定義されている）
- passed = 没年
- 0を書くのは、配列の最初を初期値とするのを防止するため

### ⑤
- return lastInventor > nextInventor ? -1 : 1;は、前の式がtrueだったら-1
     falseだったら 1 を返すということ

### ⑥
- ...は、モダンな書き方
- querySelectorAll('a'): ページ内のリンク（<a>タグ）を全部集めて「NodeListという箱」に入れる。
...: その箱をひっくり返して、中のリンクをバラバラに外に出す。
[ ]: 外に出たリンクたちを、新しい「配列（Array）という箱」に入れ直す。

- includesは、配列に含まれているかを判定

### ⑦
- 「alpha」は、アルファベットの役
- リストにある名前を感まで切り分けて前の方の名前でアルファベット順に並べなさいとしている
- 「lastOne, nextOne」は、変数なので自由に決められる
- 2行目で変数を配列で定義するのは、名前を切り分けたので2個の返り値ができてしまうため

### ⑧
- {}を記述するのは、最初は空という指示
- なぜ !obj[item] というチェックが必要か
  いきなり「car を +1 しろ」と命令しても、ノートにまだ car という項目がなかったら、
  コンピューターはどこに足せばいいか分からずエラー（または変な結果）になってしまう。
  そのため、「無ければ作る」という準備作業が最初の一回だけ必要になる。

- objは、通例で今回のようなキーと値で呼び出したものをオブジェクトと呼ぶから命名
  （なんでもいいっちゃなんでもいい）

- map: 全員に同じ魔法をかける（例：名前の後に「さん」をつける）
  filter: 条件に合う人だけ残して、あとは追い出す（例：'de' があるものだけ残す）
  reduce: 全員分の情報を一つのノートにまとめ上げる（例：乗り物を数える）
  (配列内のすべてを検索してくれる)

- returnを書かないと、次に進めず、即終了でundefinedとなる

- data.reduce((obj, item))の中の変数の意味は、objの中にitemを入れなさいということ
  よって、前の変数は、〇〇：〇〇というものをもったオブジェクトになる


## Day 3: CSS Variables (2026/03/08)
##　学んだこと
- document: ブラウザが表示している「画面全体」を指す。
- .querySelectorAll(): 「条件に合うものを全部探して持ってきて！」という命令
- ('.controls input'): 探すための「条件（CSSセレクタ）」
  controls というクラスがついた要素の中にある、input タグ（スライダーや色選択）をすべて選ぶ。
  const inputs: 見つけてきた「inputの集まり」に inputs という名前をつけて保存。

- 画像読み込みが、サービス停止になっていたため「(https://picsum.photos/800/500)」というサイトを活用
- rangeはぼかし効果
- colorはカラーピッカー
- :root{
        --spacing: 10px;
        --blur: 10px;
        --base: #ffc600;
    }
    HTML内でで使いまわしのできる変数を定義
    css-fileで定義して呼び出すことも可能

- img{
        padding: var(--spacing);
        filter: blur(var(--blur));
        background: var(--base);
    }
    で上で定義した変数を画像に適用できるようにする

- 1. 各パーツの役割を分解
    ① inputs.forEach(...)
    意味: 「捕まえた入力フォーム（input）のリストを、端から順番に1つずつ取り出すよ」という命令。

    イメージ: 出席番号1番から順番に指名していく先生のような役割。

    ② (input) => ...
    意味: 取り出した「今の1つ」を、とりあえず input という名前で呼びます、という約束。

    1回目: input は「余白（spacing）」のスライダー。

    2回目: input は「ぼかし（blur）」のスライダー。

    3回目: input は「色（base）」のカラーピッカー。

    ③ input.addEventListener("change", handleUpdate)
    意味: その「1つ」に対して、**「"change"（値が確定した時）というイベントが起きたら、handleUpdate という関数を動かして！」**と耳打ちして予約

- inputs.forEach((input) => input.addEventListener("mousemove", handleUpdate));
  はマウスを動かしたときに連動するようにした実装

- suffix があるおかげで、1つの関数で2パターンの合体が完成する。
  余白の場合: 10 + "px" = 10px
  しかし、色の場合は、単位がつくとまずいので「|| ""」をつけることで、バグを防ぐ
  色の場合: #ffc600 + "" = #ffc600 （余計な文字がつかない！）

## Day 2: JS + CSS Clock (2026/03/06)
##　学んだこと
- 線を中心に回転させる変更点
- `setInterval` の使い方（引数は、2個になる、3個にもできる）
- 角度の計算式 `(s / 60) * 360`
- 時計の針は、角度で表して適用する
- 角度の計算式 に +90をして調整する方法
- 現在の時間を呼び出す方法
- CSSで実装したtransformの変更方法

## Day 1: JavaScript Drum Kit (2026/03/05)
##　学んだこと
- キーボードのイベント取得。
- 音を鳴らすコード
- 実装による期待されたものが終了したらもとに戻す方法
- キーボードをターゲットにする方法
