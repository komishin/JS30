# JavaScript30 学習記録

## Day 13: Vanilla JavaScript Slide In on Scroll (2026/03/16)
## 学んだこと

### 毎回画面に表示されたときスライドして入ってくることが繰り返せる
### アクションが多くなって動作に支障を来さないようにする隠れコードの説明

- debounce（デバウンス）という関数は、「短時間に何度も起きるイベントを、間引いて実行する」
  ための非常に重要なテクニック。
  例えば、スクロールやタイピングのたびに重い処理が走るとブラウザが固まってしまうが、これを使う
  と「ここが大事！！」=>「動きが止まってから1回だけ実行する」といった制御ができる。

- function debounce(func, wait = 20, immediate = true)
  func: 実行したいメインの処理。
  wait: 何ミリ秒待つか（デフォルトは 20ms）。
  immediate: 「最初にすぐ実行するか」のフラグ（デフォルトは true）。

  var timeout;
  タイマーの情報を保存しておくための変数。

  return function()
  実際にイベント（スクロールなど）が発生したときに呼ばれる「新しい関数」を返す。

  var context = this, args = arguments;
  その時の this（実行環境）や arguments（引数）を保存し、後で本来の関数に引き継げるようにする。

  var later = function()
  指定した待ち時間が経過した後に実行される「予約用」の関数。

  timeout = null;
  待ち時間が経過したので、タイマーをリセット。

  if (!immediate) func.apply(context, args);
  「後で実行」モードなら、ここで本来の処理（func）を実行。

  var callNow = immediate && !timeout;
  「今すぐ実行」モードであり、かつ「まだタイマーが動いていない（＝最初の1回目）」かを判定。

  clearTimeout(timeout);
  動いているタイマーを一旦キャンセル。これで連続した入力を「リセット」し続ける。

  timeout = setTimeout(later, wait);
  新しくタイマーをセットし直す。「最後にイベントが起きてから wait 秒後」に later を実行する予約

  if (callNow) func.apply(context, args);
  9行目の判定がOKなら、即座に本来の処理を実行。

  🎨 視覚的なイメージ
  デバウンスは、**「エレベーターのドア」**に例えると分かりやすい。
  誰かがボタンを押す（イベント発生）たびに、閉まりかけたドアが開く（タイマーのリセット）。
  誰も来なくなってから数秒後（wait）、ようやくエレベーターが動き出す（関数の実行）。

- 🔍 実行回数が膨れ上がる理由
  高頻度なイベント発生
  ブラウザの scroll イベントは、ピクセル単位の移動に対して発生。滑らかなスクロール（スマホのスワイプやトラックパッド）では、1秒間に 60回〜100回以上 この checkSlide 関数が呼び出されることも珍しくない。

  ループ処理の重なり
  スクロールのたびに sliderImages.forEach(...) が実行される。

  もし画像が 10枚 あれば、1回のスクロール（微細な動き）で 10回の計算 が行われる。

  1秒間に 60回 スクロールイベントが発生すると、60回 × 10枚 ＝ 600回 の計算が毎秒走り続ける
  ことになる。

- window.scrollY
  ページの一番上から、今どれくらい下にスクロールしたかという距離（ピクセル）。
  スクロールしていないときは 0 。

  window.innerHeight
  今見えているブラウザの表示画面（ウィンドウ）自体の高さ。
  画面を大きくしたり小さくしたりすると変わる。

- window.scrollY + window.innerHeight
  今見えている画面の、一番下のライン（底）が、ページ全体のどこに位置しているかを指している。

- ブラウザが常に「この画像は、ページの一番上から数えて何ピクセルの地点に配置されているか」
  という正確な位置データを保持しており、コードはそれをいつでも取り出すことができるので
  -sliderImage.height / 2;
  で画像の半分通過を感知できる。

- const imageBottom = sliderImage.offsetTop + sliderImage.height;
  は、画像が見えなくなったときは、表示を解除しておきたいから定義している。

- window.scrollY は、「画面の上端が、ページ全体のどこにいるか」を示す現在地

- .offsetTopは、上からの距離という「決まり文句（プロパティ）」

- isHalfShown && isNotScrolledPast
  「画像が下から顔を出した」 かつ 「まだ上へ消え去っていない」 という、まさに画像が画面の中に
   存在している瞬間

## Day 12: CJavaScript KONAMI CODE (2026/03/16)
## 学んだこと

- window.addEventListener("keydown", ...)
  ブラウザ画面全体で、「キーが押された（keydown）」瞬間をずっと見張る。
  (e) => { ... }
  キーが押された時に実行される関数（処理のまとまり）です。event には「どのキーが押されたか」などの情報が入っている。

- 配列の先頭から、(今の長さ - 秘密の言葉の長さ) 分だけ削除する
  pressed.splice(-secretCode.length - 1, pressed.length - secretCode.length);

- pressed.join("")
  pressed は ['w', 'e', 's', 'b', 'o', 's'] のようなバラバラの文字のリスト（配列）。
  .join("") を使うことで、これらをくっつけて "wesbos" という一つの「文字列」 に変換。
  .includes(secretCode)
  変換した文字列の中に、あなたが決めた合言葉（"wesbos" など）が含まれているかを調べます。
  if (...) { console.log("DING DING DING!"); }
  もし含まれていたら（一致したら）、コンソールに 「DING DING DING!（ピンポンピンポーン！）」 と当たりを知らせる文字を出します。

- figletというツールをインストール
  brew install figlet
  文字を出力
  figlet CORNIFY
  これで、アート的な文字をコードエディタに書くことができる


  <!-- ____ ___  ____  _   _ ___ _______   __
 / ___/ _ \|  _ \| \ | |_ _|  ___\ \ / /
| |  | | | | |_) |  \| || || |_   \ V /
| |__| |_| |  _ <| |\  || ||  _|   | |
 \____\___/|_| \_\_| \_|___|_|     |_| -->
 

## Day 11: Custom HTML5 Video Player (2026/03/15)
## 学んだこと

- playerとつけるのは、player_のようなクラスを持つものに限定するため

- const method = video.paused ? 'play' : 'pause';
  ここでは、三項演算子（if文の短い書き方）を使って、次に実行すべき「命令の名前」を決めている。
  video.paused: 動画が停止しているかどうかを確認します（停止中なら true）。
  ? 'play': もし停止中（true）なら、変数 method に 'play' という文字列を入れる。
  : 'pause': もし再生中（false）なら、変数 method に 'pause' という文字列を入れる。
  video[method]();
  JavaScriptでは、オブジェクトのメソッド（機能）を 点（.） ではなく ブラケット（[ ]） で呼び出すことができる。
  通常は
  再生したいとき： video.play()
  止めたいとき： video.pause()
  しかし、このコードでは method という変数の中に 'play' か 'pause' という文字列が入っているので、video['play']() または video['pause']() となり、結果として適切なメソッドが実行されます。

- function updateButton() {
  const icon = this.paused ? '►' : '❚ ❚';
  toggle.textContent = icon;
  }
  は、アイコンが押されたときの入れ替え用

- 1. this.dataset.skip （値の読み取り）
  HTMLの方を確認すると、ボタンには data-skip="-10" や data-skip="25" と書かれているはず。
  this: クリックされた「そのボタン」自身を指す。
  .dataset: HTMLの data- から始まる属性（カスタムデータ属性）にアクセスする。
  .skip: data-skip の「skip」の部分を読み取る。
  つまり、10秒戻るボタンを押したときは "-10" という文字、25秒進むボタンを押したときは "25" という文字を連れてくる。
  2. parseFloat(...) （数値への変換）
  dataset で取ってきた値は、見た目が数字でもプログラム上は文字列（テキスト）として扱われている。
  そのまま足し算をしようとするとエラーや予期せぬ動きになるため、parseFloat を使って 「計算ができる数字」 に変換している。
  3. video.currentTime += ... （時間の書き換え）
  video.currentTime: 動画の現在の再生位置（何秒目か）を表す。
  +=: 「今の数字に、右側の数字を足して上書きする」という意味。

- 1. this.name （どの設定を変えるか）
     HTMLのスライダー（<input type="range">）を見ると、それぞれ name 属性がついているはず。
     音量スライダー: name="volume"
     再生速度スライダー: name="playbackRate"
     この name の値は、実は先ほどお話しした 「ブラウザの決まりごと（標準プロパティ名）」 と全く同じ名前に設定されている。
     this.name と書くことで、今動かしているのが「音量」なのか「速度」なのかを自動的に判別。

  2. this.value （数字はいくつか）
     これはスライダーが今指している「数字」。
     音量なら 0 〜 1 の間
     速度なら 0.5 〜 2 などの間

  3. video[this.name] = this.value;
     これらを組み合わせると、裏側ではこのようなことが起きている。
     音量スライダーを動かしたとき：
     video['volume'] = 0.5; （＝ video.volume = 0.5; と同じ意味）
     再生速度スライダーを動かしたとき：
     video['playbackRate'] = 2; （＝ video.playbackRate = 2; と同じ意味）

- 1. (video.currentTime / video.duration) * 100
     ここで「動画が全体の何パーセントまで進んだか」を計算。
     video.currentTime: 今、何秒目か。
     video.duration: 動画の全体の長さ（合計何秒か）。
     計算の例:
     もし、全体の長さが 100秒 の動画で、今 30秒 まで進んでいたとしたら…
     30 / 100 = 0.3
     これに 100 をかけることで、30% という数字を導き出す。

  2. progressBar.style.flexBasis = ...
     計算して出した % を、実際に見た目の長さに変える部分です。
     flexBasis: これは CSS のプロパティで、要素の「基本の大きさ」を指定するもの。
     ${percent}%: 先ほど計算した数字に % という文字をくっつけてる（例：30%）。
     このコードによって、HTMLの progressBar 要素の CSS がリアルタイムで書き換わり、バーが伸びていく仕組み。

- 1. e.offsetX / progress.offsetWidth
     ここで「バー全体の長さに対して、どのあたりをクリックしたか」という割合（0〜1の間）を出す。
     e.offsetX: クリックされた位置が、バーの左端から何ピクセル目かという数値。
    progress.offsetWidth: プログレスバー（親要素）全体の横幅（ピクセル）。
    計算の例:
    バー全体の長さが 500px で、ちょうど真ん中の 250px のあたりをクリックしたとしたら…
    250 / 500 = 0.5 （つまり、全体の50%の位置）となる。
  2. * video.duration
     出した割合に、動画の総時間をかけ算する。
     計算の例:
     もし動画が 100秒 だった場合、先ほどの 0.5 をかけると…
     0.5 * 100 = 50 （50秒目）という時間が導き出される。
  3. video.currentTime = scrubTime;
     最後に、計算で出した秒数を video.currentTime に代入します。これで、動画がクリックした位置までパッと飛ぶ。
   4. マウスを動かしながら（ドラッグで）操作する工夫
     JavaScript30では、クリックした時だけでなく「マウスを押しながら動かした時」も動画が連動するように、以下のような高度な設定をしている。

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
