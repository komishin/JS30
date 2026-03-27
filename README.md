# JavaScript30 学習記録

## Day 29: Vanilla JS Countdown Timer (2026/03/27)
## 学んだこと

#### 最初の変数は、HTMLにJSから伝えるためにある

### 用語解説

`setInterval` 「1秒おきにこの処理をずっとやって！」と命令する。
`clearInterval` 「さっきの繰り返し、もうやめていいよ！」と終了させる。
`secondsLeft` 日本語で残りの時間の秒数
`'button'`	タグ名で探す	document.querySelectorAll('button')
`'.btn'`	クラス名で探す	document.querySelectorAll('.btn')
`'[data-time]'`	属性名で探す	document.querySelectorAll('[data-time]')



### コード解説

`let countdown;`
  * タイマーのID（番号）を入れる箱を用意しています。最初は空で、後から `setInterval` が返す番号を入れます。
  一番上に書いておくことで、すべての関数が同じ countdown という変数にアクセスできるようになります。

`const timerDisplay = document.querySelector('.display__time-left');`
  * 「残り時間」を表示する画面上の要素を取得しています。

`const endTime = document.querySelector('.display__end-time');`
  * 「何時に戻ってくるか」を表示する要素を取得しています。

`const buttons = document.querySelectorAll('[data-time]');`
  * `data-time` という属性を持つボタン（例：20分、5分など）を全部まとめて取得しています。

`function timer(seconds) {`
  * カウントダウンを開始する関数です。「何秒カウントするか」を引数で受け取ります。

`clearInterval(countdown);`
  * 前のタイマーが動いていたら止めます。ボタンを連続で押したとき、二重に動かないようにするための処理です。

`const now = Date.now();`
  * 今この瞬間の時刻をミリ秒（数字）で取得しています。

`const then = now + seconds * 1000;`
  * 「今 + 秒数 × 1000」で、タイマーが終わる時刻（ミリ秒）を計算しています。1000をかけるのはミリ秒に変換するためです。

`displayTimeLeft(seconds);`
  * 最初の残り時間をすぐ画面に表示します。`setInterval` は1秒後に動くため、最初の1秒間を埋めるために先に呼びます。

`displayEndTime(then);`
  * 終了時刻（「〇時〇分に戻ります」）を画面に表示します。

`countdown = setInterval(() => { ... }, 1000);`
  * 1000ミリ秒（1秒）ごとに中の処理を繰り返します。返ってきたIDを `countdown` に入れておきます。

`const secondsLeft = Math.round((then - Date.now()) / 1000);`
  * 「終了時刻 − 今の時刻」でミリ秒の差を求め、1000で割って秒数に変換しています。`Math.round` で四捨五入しています。

`if(secondsLeft < 0) { clearInterval(countdown); return; }`
  * 残り時間がマイナスになったら、タイマーを止めて処理を終了します。

`function displayTimeLeft(seconds) {`
  * 残り秒数を「分:秒」の形に変換して画面に表示する関数です。

`const minutes = Math.floor(seconds / 60);`
  * 秒数を60で割り、小数点を切り捨てて「分」を求めています。

`const remainderSeconds = seconds % 60;`
  * 秒数を60で割ったあまりが「秒」の部分です。

`const display = \`${minutes}:${remainderSeconds < 10 ? '0' : ''}${remainderSeconds}\`;`
  * 1桁の秒数には先頭に「0」を付けて「1:05」のような表示にしています。

`document.title = display;`
  * ブラウザのタブのタイトルも残り時間に変えています。タブを見るだけで残り時間がわかります。
    HTMLのtitleで上のタブの表示

`timerDisplay.textContent = display;`
  * 画面上の残り時間表示エリアに文字を書き込んでいます。

`function displayEndTime(timestamp) {`
  * タイマー終了時刻（ミリ秒）を受け取り、「Be Back At 〇:〇〇」という形で画面に表示する関数です。

`const end = new Date(timestamp);`
  * ミリ秒の数値を `Date` オブジェクト（日時情報）に変換しています。

`const hour = end.getHours();`
  * 終了時刻の「時」を取り出しています（0〜23の数値）。

`const adjustedHour = hour > 12 ? hour - 12 : hour;`
  * 12時間表示に変換しています。13時なら13−12＝1時、のように変換します。

`const minutes = end.getMinutes();`
  * 終了時刻の「分」を取り出しています。

`endTime.textContent = \`Be Back At ${adjustedHour}:${minutes < 10 ? '0' : ''}${minutes}\`;`
  * 「Be Back At 1:05」のような文字を画面に書き込みます。1桁の分には「0」を付けます。

`function startTimer() {`
  * ボタンがクリックされたときに呼ばれる関数です。`this` はクリックされたボタンを指します。

`const seconds = parseInt(this.dataset.time);`
  * クリックされたボタンの `data-time` 属性の値（文字列）を数値に変換して秒数として取得しています。

`buttons.forEach(button => button.addEventListener('click', startTimer));`
  * 全ボタンにクリックイベントを登録しています。どれを押しても `startTimer` が呼ばれます。

`document.customForm.addEventListener('submit', function(e) { ... });`
  * フォームが送信されたときの処理を登録しています。`document.customForm` はフォームの `name` 属性で直接取得しています。

`e.preventDefault();`
  * フォーム送信時にページがリロードされるのを防いでいます。

`const mins = this.minutes.value;`
  * フォームの入力欄（`name="minutes"`）に入力された値を取得しています。

`timer(mins * 60);`
  * 入力された分数を60倍して秒数に変換し、`timer` 関数に渡してカウントダウンを開始します。

`this.reset();`
  * カウントダウン開始後、フォームの入力欄を空に戻しています。



---




## Day 28: Video Speed Scrubber (2026/03/26)
## 学んだこと

### コード解説

`const speed = document.querySelector('.speed');`
  * 速度バーのコンテナ要素（.speed）を取得して speed に入れています。マウスイベントを登録する対象になります。

`const bar = speed.querySelector('.speed-bar');`
  * speed の中にある .speed-bar 要素を取得しています。height や textContent を変化させることで、視覚的なバーとラベルを表示します。

`const video = document.querySelector('.flex');`
  * 動画要素（.flex）を取得しています。再生速度を変更するときに使います。

`function handleMove(e) {`
  * mousemove イベントが発生したときに呼ばれる関数です。`this` はイベントが登録された要素（.speed）を指します。
  **e.pageYがあるから、引数に`e`が必要になる**

`const y = e.pageY - this.offsetTop;`
  * ページ全体の上端からのマウスY座標（e.pageY）から、.speed 要素の上端位置（this.offsetTop）を引くことで、「.speed 要素の内側でのマウスの縦位置」を求めています。

`const percent = y / this.offsetHeight;`
  * y（要素内の縦位置）を要素の高さ（this.offsetHeight）で割ることで、0〜1の割合（パーセント）に変換しています。上端が0、下端が1になります。

`const min = 0.4;`
  * 再生速度の最小値を 0.4 倍速に設定しています。

`const max = 4;`
  * 再生速度の最大値を 4 倍速に設定しています。

`const height = Math.round(percent * 100) + '%';`
  * percent（0〜1）を 100 倍してパーセント文字列に変換しています。これをバーの height に使って、マウス位置に応じてバーの高さが変わるようにします。
  `%がないと`
  1. 「数字」だけではブラウザが困ってしまう
    * height という言葉は、人間にとっては「高さ」だと分かりますが、ブラウザにとってはただの「箱の名前」に過ぎません。

    * Math.round(percent * 100) だけの場合
     中身は 50 や 80 といった単なる**「数字」になります。
     ブラウザに bar.style.height = 50; と命令しても、ブラウザは「50って何？ 50ピクセル？ 50ミリ？ それとも50%？」と迷ってしまい、結局何も動いてくれません。**

  * + '%' を付けた場合
    中身は "50%" や "80%" という**「単位付きの文字（文字列）」**になります。
    これならブラウザも「あ、高さ（height）を50%分だけ伸ばせばいいんだな！」と理解して、ビヨーンとバーを伸ばしてくれます。

`const playbackRate = percent * (max - min) + min;`
  * percent（0〜1）を min〜max の範囲にマッピングしています。上端（percent=0）なら 0.4 倍速、下端（percent=1）なら 4 倍速になります。

`bar.style.height = height;`
  * バー要素の高さをマウス位置に合わせて変更し、視覚的なフィードバックを与えています。

`bar.textContent = playbackRate.toFixed(2) + '×';`
  * バーに表示するテキストを更新しています。`toFixed(2)` で小数点2桁に丸めて "1.25×" のように表示します。

`video.playbackRate = playbackRate;`
  * 動画の再生速度を実際に変更しています。`playbackRate` はブラウザ標準のプロパティで、1 が通常速度です。

`speed.addEventListener('mousemove', handleMove);`
  * .speed 要素上でマウスを動かすたびに handleMove を呼び出すよう登録しています。




---




## Day 27:JavaScript Interface Challenge: Click and Drag to Scroll (2026/03/27)
## 学んだこと

### 用語解説
  `mousedown`	マウスのボタンを押し下げた瞬間
  `mousemove`	マウスを動かしている間ずっと
  `mouseup`	マウスのボタンを離した瞬間
  `e.pageX`	ページ全体の左端	スクロールしていても、ページの一番左上からの距離を測れる。
  `e.clientX`	ブラウザで見えている範囲	スクロールしても、今見えている画面の左端からの距離しか測れない。
  `e.screenX`	PCモニターの画面全体	ブラウザの外枠も含めた、モニター自体の左端からの距離。
  `offsetLeft`	左端からの距離
  `offsetTop`	上端からの距離
  `offsetWidth`	要素自体の横幅（ボーダーなども含む）
  `offsetHeight`	要素自体の高さ
  `scrollLeft`	横にどれくらいスクロールしたか
  `scrollTop`	縦にどれくらいスクロールしたか
  `scrollWidth`	スクロールできる中身の「全体の横幅」
  `scrollHeight`	スクロールできる中身の「全体の高さ」
  `e.preventDefault()` ここでは、「ブラウザ標準の挙動」を一時的にオフにしています。


### コード解説

`const slider = document.querySelector('.items');`
  * スライドさせる対象となる親要素（.items）を取得して slider に入れています。

`let isDown = false;`
  * 「今、マウスがクリック（長押し）されているか？」を覚えておくためのスイッチです。

`let startX;`
  * マウスを**押し下げた瞬間の横位置（X座標）**を保存しておくための変数です。

`let scrollLeft;`
  * マウスを押し下げた瞬間のスライダーのスクロール位置を保存しておくための変数です。

`slider.addEventListener('mousedown', (e) => {`
  `isDown = true; // スイッチをONにする`
  `slider.classList.add('active'); // CSSで見た目を変えるためにクラスを付ける`
  `startX = e.pageX - slider.offsetLeft; // 画面全体からスライダーの左端を引いた「スライダー内での開始位置」を計算`
  `scrollLeft = slider.scrollLeft; // その時のスクロールの状態を記録しておく`
`});`

`slider.addEventListener('mouseleave', () => {`
  `isDown = false; // マウスが外に出たらスイッチをOFFにする`
  `slider.classList.remove('active'); // クラスを外す`
`});`

`slider.addEventListener('mouseup', () => {`
  `isDown = false; // マウスを離したらスイッチをOFFにする`
  `slider.classList.remove('active'); // クラスを外す`
`});`

`slider.addEventListener('mousemove', (e) => {`
  `if (!isDown) return;  // スイッチがOFF（クリック中じゃない）なら、何もしないで終了`
  `e.preventDefault(); // ブラウザが勝手にテキストを選択したりするのを防ぐ`
  `const x = e.pageX - slider.offsetLeft; // 今現在のマウスの位置を計算`
  `const walk = (x - startX) * 3; // 「最初に押した場所」から「今の場所」までの距離を計算（3倍速で動かす設定）`
  `slider.scrollLeft = scrollLeft - walk; // 計算した距離を、元のスクロール位置から引き算して画面を動かす`
`});`

#### 💡 ポイントの解説：なぜ scrollLeft - walk なのか？

  * 右にマウスを動かすと、(x - startX) は プラス になります。

  * スライダーを**右に流す（左側の隠れている部分を見る）**ためには、スクロールの数値は マイナス に動かす必要があります。

  * なので scrollLeft - walk とすることで、直感的にマウスと同じ方向にコンテンツが動くようになっています。

  * この * 3 という数字を大きくすると、少しのマウス移動でたくさんスクロールするようになる。




---




## Day 26:Stripe Follow Along Dropdown Navigation (2026/03/26)
## 学んだこと

#### 用語解説

  `mouseenter`	マウスカーソルが要素の中に入った瞬間	背景を表示し、位置を計算して移動させる
  `mouseleave`	マウスカーソルが要素の外に出た瞬間	背景を非表示にし、クラスを削除する


#### コード解説

  * .cool > li: ナビゲーションの各メニュー項目（リスト要素）をすべて取得し、triggers という変数に入れています。

  * .dropdownBackground: メニューの背後で動く「白い背景ボックス」を取得しています。

  * .top: ナビゲーション全体の親要素です。座標計算の「基準点」として使います。

  * マウスが乗った項目（this）に trigger-enter クラスを付けます。これでメニューが「表示」状態（display: block など）になります。

  1. setTimeout との関係

    - `setTimeout(() => this.classList.contains('trigger-enter') && this.classList.add('trigger-enter-active'), 150);`
    (**アロー関数でないとエラーになる**)

    まず、マウスが乗った瞬間に trigger-enter クラスがつきます。

    次に「150ミリ秒待ってね」というタイマー（setTimeout）が動きます。

    150ミリ秒経った後、この () => ... の中身が実行されます。

  2. なぜ contains でチェックするのか？
     ユーザーが「メニューを高速でシャカシャカ動かしたとき」を想像してみてください。

     `0ms` マウスが乗る（trigger-enter 付与、タイマー開始）

    `50ms` すぐにマウスが外れる（handleLeave が実行され、trigger-enter が削除される）

    `150ms` タイマー発動！

    もし、このチェック（contains）がないと、マウスがもうそこには無いのに、遅れてやってきたタイマーが trigger-enter-active を付けてしまいます。
    その結果、メニューが消えきらずに中途半端に表示されたり、表示がバグったりする原因になる。

  3. &&（短絡評価）のテクニック
    A && B という書き方は、**「Aが本当（true）なら、Bを実行する」**というルールがあります。

    もし this.classList.contains('trigger-enter') が true なら
    （＝まだマウスが乗っているなら）
    → 右側の this.classList.add(...) を実行して、アニメーションを完了させる。

    もし false なら
    （＝すでにマウスが外れてクラスが消えているなら）
    → 右側は無視される。


  * 150ミリ秒後に trigger-enter-active クラスを付けます。
    `this.classList.contains(...)` を確認しているのは、**「150ミリ秒経つ前にマウスが外に出ちゃった場合」**に、アニメーションが始まらないようにするためです。

  * `background.classList.add('open');`
     背景ボックス（動く白い四角）を表示させます。

  * `const dropdown = this.querySelector('.dropdown');`
    今マウスが乗っている項目の中にある、ドロップダウンメニュー（実際の中身）を探します。

  * `const dropdownCoords = dropdown.getBoundingClientRect();`
    `const navCoords = nav.getBoundingClientRect();`
    getBoundingClientRect(): ブラウザ画面上の正確な位置（上下左右）とサイズ（幅・高さ）を数値で取得します。

  * `const coords = {`
    `height: dropdownCoords.height,`
    `width: dropdownCoords.width,`
    `top: dropdownCoords.top - navCoords.top,`
    `left: dropdownCoords.left - navCoords.left`
    `};`
    - ここで座標のズレを計算しています。
    - ブラウザ全体の座標から「親ナビゲーションの座標」を引くことで、「親ナビの左上から見て何ピクセル目か」という数値を割り出します。

    * `引き算するとき` 「親要素の中での相対的な位置」を求めたいとき（今回はこちら）。
    * `足し算するとき` absolute 配置の要素を、スクロールしてもずれないように「ページの一番上からの絶対位置」に置きたいとき（window.scrollY を足すなど）。

  * `background.style.setProperty('width', ``${coords.width}px``);`
    `background.style.setProperty('height', ``${coords.height}px``);`
    計算したサイズを、背景ボックスに適用します。これで背景がメニューと同じ大きさになります。

  * `background.style.setProperty('transform',` `translate(${coords.left}px, $. {coords.top}px)``);`
    `}`
    背景ボックスを translate（移動）させます。これで、背景が現在のメニューの位置まで滑らかに飛んでいきます。

  *  `function handleLeave() {`
     `this.classList.remove('trigger-enter', 'trigger-enter-active');`
     付与していたクラスをすべて削除し、メニューを非表示にします。

  *  `background.classList.remove('open');`
      動く背景ボックスも非表示（透明など）にします。

  *  `triggers.forEach(trigger => trigger.addEventListener('mouseenter', handleEnter));`
    `triggers.forEach(trigger => trigger.addEventListener('mouseleave', handleLeave));`
    すべてのメニュー項目（triggers）に対して、「マウスが入ったとき」と「出たとき」の処理を紐付けています。




---




## Day 25:JavaScript Event Capture, Propagation and Bubbling(2026/03/25)
## 学んだこと

1. 全体の動き：何が起きるのか
  * HTML内に複数の div が入れ子（親子関係）になっている場合、一番内側の div をクリックすると、**「外側の div も順番にクリックされたこと」**として処理が進みます。

  * `ログの出力`: 各 div に設定された logText 関数が実行され、その要素のクラス名
                （one, two, three など）がコンソールに表示されます。

  * `バブリング現象`: capture: false（デフォルト）なので、クリックした地点から
                   「内側から外側へ」向かって順番にログが出ます。

2. once: true の強力な効果
  * コード内で div と button の両方に { once: true } が指定されています。これがこのコードの面白いポイントです。

  * 1回限定の反応: 各要素は一度クリックされると、そのイベント設定が自分自身で消滅します。

`具体例`

  * 一番内側の div をクリックする。

  * バブリングにより、親の div たちのログもすべて出る。

  * しかし、もう一度同じ場所をクリックしても、すでに once によってイベントが解除されているため、二度目は何も起きません。

3. コメントアウトされている e.stopPropagation()
  ここがイベント制御の鍵となる部分です。

  * もし e.stopPropagation() のコメントを外すと、イベントの連鎖（バブリング）がそこでストップします。

  * 一番内側をクリックしても、その要素のログだけが出て、親要素にはイベントが伝わらなくなります。

まとめると
  * このコードは、**「クリックイベントが親要素へと連鎖していく様子（バブリング）を確認しつつ、それを一度きりで無効化する」**という実験的な処理を行っています。

  * `豆知識`: once: true は、ボタンの連打による二重送信防止（決済ボタンなど）に
             非常に便利な最新の書き方です（以前は手動で removeEventListener を書く必要がありました）。


1. バブリング (capture: false)
  * イベントが 「発生した場所（ターゲット）」から「外側の親要素」へ と泡（Bubble）のように上がっていく仕組みです。

  もし div が 3 (内側) < 2 < 1 (外側) と重なっていた場合、一番内側の 3 をクリックすると：
  div 3 のイベントが動く
  div 2 のイベントが動く
  div 1 のイベントが動く
  という順番で実行されます。

2. キャプチャリング (capture: true)
  * 逆に、イベントを 「外側の親要素」から「内側のターゲット」へ と向かって捕まえて（Capture）いく仕組みです。

  * もし true に変えると:
  * 一番内側の 3 をクリックしたとしても：

  div 1 (外側) が先に反応する
  div 2 が反応する
  div 3 (クリックした本人) が最後に反応する
  という「外から中へ」の順番になります。

  * なぜ false（バブリング）が一般的なのか？
    直感的に「クリックしたそのもの」がまず反応してほしいことが多いからです。

**例えば、リストの中の「削除ボタン」を押したとき、まずボタンの処理（削除）が走り、そのあとで「リスト全体がクリックされた」という親の処理が走るのが自然な流れですよね。**

**この「バブリング」を途中で止めたいときに使うのが、 e.stopPropagation() です。**

#### foreachを使うのは
##### これを自動化してくれるのが forEach

  `divs[0].addEventListener('click', logText, { once: true });`
  `divs[1].addEventListener('click', logText, { once: true });`
  `divs[2].addEventListener('click', logText, { once: true });`
  だから

#### モーダルやメニューを「外側クリック」で閉じたい場合

  * よくある「画面のどこかをクリックしたらメニューを閉じる」という挙動です。
  * document（一番外側）にクリックイベントを置きます。
  * バブリングのおかげで、画面上のどこをクリックしても最終的に document までイベントが届く。
  * これを利用して「クリックされたのがメニュー内じゃなければ閉じる」という判定が1か所で済む。

#### 逆に「バブリングしてほしくない」のはどんな時？
  * 例えば、「削除ボタン」が「カード全体」の中にあるようなデザインです。
  * カード全体をクリックすると「詳細画面へ移動」
  * カード内の小さなゴミ箱ボタンをクリックすると「削除」
  * この時、ゴミ箱を押したのにバブリングで親（カード全体）まで反応してしまうと、**「削除した瞬間に詳細画面へ移動する」**というおかしな挙動になります。
  * ここで登場するのが、あなたのコードにもあった e.stopPropagation() です。「ここでストップ！親には伝えないで！」と命令することで、この競合を防ぎます。




---



## Day 24:Vanilla JavaScript Sticky Nav   (2026/03/24)
## 学んだこと

#### CSSでwidthを０　→　autoとしないのは、これでは、`transition: all 0.5s;`が効かないから

#### transform: scale(0.98); から transform: scale(1); への変化は、見た目には「一瞬だけわずかに小さくなっていたものが、元の大きさに戻る」という動きになります。

  数値でいうと、2%分だけ拡大される計算です。

  具体的にどう見えるか？
  scale(0.98) (変化前)

  元のサイズの98%の状態です。

  ほんの少しだけ「ギュッ」と内側に縮んでいるように見えます。

  scale(1) (変化後)

  100%（等倍）、つまり元のサイズに戻ります。

  ##### よく使われるシーン：ボタンの「押し込み」演出
  この数値の変化は、Webサイトのボタンやカード型の要素で「クリックした感（フィードバック）」を出すために非常によく使われます。

  ボタンを押した瞬間（:active） scale(0.98) にする

  指を離した瞬間 scale(1) に戻る

  これにより、ユーザーは「あ、今ちゃんとボタンを押せたな」という感触を視覚的に得ることができます。1.0から0.95〜0.98くらいに設定するのが、最も自然で心地よい「沈み込み」に見えると言われています。

  アニメーションを滑らかにするコツ
  ただ数値を変えるだけだと「パッ、パッ」と切り替わってしまいますが、CSSに transition を加えると、ムニュッとした柔らかい動きになります。

  CSS
  .nav-item {
    transition: transform 0.1s ease-in-out; /* 0.1秒かけて変化させる */
  }

  クリック時や特定のクラスがついた時
  .nav-item.active {
    transform: scale(1); /* 0.98から1へスムーズに戻る */
  }


`let topOfNav = nav.offsetTop;`

  * `nav`: 対象となるHTML要素（ナビゲーションバーなど）を指します。

  * `.offsetTop`: その要素の「外枠」から、親要素（通常はページ全体の body）の「上端」
                   までの距離（ピクセル数）を返すプロパティです。

  * `let topOfNav`: 取得した数値を後で使うために記憶させています。

### この関数 fixNav() は、スクロールした際にナビゲーションバーを画面上部にピタッと固定する（スティッキーヘッダー）ためのスイッチのような役割

1. 判定：いつ固定するか？

  * `if (window.scrollY >= topOfNav) {`
     `window.scrollY`: 現在、画面をどれくらい下にスクロールしたか。

  * `topOfNav`: さきほど取得した「ナビの元の位置」。

  *  `意味`: 「ナビの位置までスクロールした（または通り過ぎた）」瞬間に、if の中身を実行。

2. `固定時の処理`：ガタつき（ジャンプ）の防止

`document.body.style.paddingTop = nav.offsetHeight + 'px';`
`document.body.classList.add('fixed-nav');`
  * `classList.add('fixed-nav')`: CSSで position: fixed; を適用し、ナビを浮かせて固定

  * `nav.offsetHeight`: ナビ自体の「高さ」を取得します。

  * `paddingTop の追加`: ナビを fixed（固定）にすると、そのナビが占めていたスペースが
                        ページから消え、下のコンテンツが急に上にズボッと入り込む。
                        （画面がガクッと揺れる原因）。

  * `解決策`: ナビが浮いた分、同じ高さの余白（padding）を body の一番上に追加して、
            表示が崩れないように補っています。

   * JSので`nav.offsetHeight` を使うことで、その瞬間の「正確な高さ」を自動で測って余白を作ってくれます。

3. `解除`：元の位置に戻ったとき

`else {`
  `document.body.classList.remove('fixed-nav');`
  `document.body.style.paddingTop = 0;`
`}`
  * 上にスクロールして元の位置に戻ったら、固定クラスを外し、追加していた余白も 0 に戻します。




---



## Day 23:JavaScript Text-To-Speech   (2026/03/23)
## 学んだこと

### 用語解説
#### ブラウザが用意した「公式窓口」の名前
**これらは「Web Speech API」という仕様で決められている、世界共通の予約語です。**

* `DOMContentLoaded` : 	HTMLの読み込みがすべて完了した時
* `voiceschanged` : 	音声合成に使う「声」の準備が整った時
* `change` :	選択肢が切り替わった時	声の種類を変える(今回の使い道)
* `speechSynthesis` : 音声合成のコントローラー本体です。
* `SpeechSynthesisUtterance` : ブラウザに「喋れ」「止まれ」と命令を出す司令官。
* `getVoices()` : システムから声のリストを取り出すための専用の命令です。
* `speak() / cancel()` : 「喋れ」「止まれ」という実行命令です。
* `includes` : 特定の文字（'ja'など）が含まれているか調べる命令。

#### 自分でつけた名前と目的
* `msg (Utterance)` :	発言依頼書。 喋る内容、声の種類、速さ、高さを書き込む書類。
* `populateVoices` :	名簿作成係。 Mac/PC内の声から「日本語」だけを抜き出し、メニューを作る。
* `setVoice`	声の決定。 ドロップダウンで選ばれた声を、発言依頼書（msg）にセットする。
* `setOption`	微調整。 スライダーを動かした瞬間に、速さや高さを書き換える。
*`toggle`	再生/停止スイッチ。 喋っている最中なら一度止め、最新設定で喋り直す。

`speakButton.addEventListener('click', toggle);`
`stopButton.addEventListener('click', () => toggle(false));`
`引数が必要ない時`: シンプルに 関数名 だけを書く。（例：speakButton）
                 **そのまま実行**
`引数を渡したい時`: () => 関数名(値) と包んで書く。（例：stopButton）
                 **変化をして、他のことを実行！！**

**MDN（Mozilla Developer Network） という公式辞書のようなサイトで「SpeechSynthesis」と検索して、「どんなイベントがあるかな？」と調べます。**

**スイッチのON/OFFを切り替えるような動きを英語で `Toggle（トグル）` と呼ぶので、変数に選定**


###　コード解説

`new SpeechSynthesisUtterance()` は、簡単に言うと **「喋る内容（台本）を書き込むための、真っ白な原稿用紙を1枚用意する」** という処理をしています。
1. なぜ new が必要なのか？ブラウザには「音声を合成して出す機能」がありますが、何を、どんな声で、どのくらいの速さで喋るかは、その都度新しく作る必要があります。

* `SpeechSynthesisUtterance`: 「音声合成のセリフ」という種類のオブジェクト（設計図）。

* `new`: その設計図をもとに、実体（今回のセリフ専用のデータ）を新しく作る、という命令。
これで、`msg` という変数の中に「喋るための準備が整った空のデータ」が入ります。

2. 「原稿用紙（msg）」に書き込めることこの msg（原稿用紙）には、以下のようなプロパティ（設定項目）を書き込むことができます。
`msg.text`   実際に喋らせたい**「文章」**
`msg.lang`   言語（'ja-JP' なら日本語、'en-US' なら英語）
`msg.pitch`  声の高さ（高い声か、低い声か）
`msg.rate`   喋る速さ（ゆっくりか、早口か）
`msg.voice`  声の種類（男性の声、女性の声など）


### `let voices = [];` のように、最初に空の配列（中身が空っぽの箱）を用意するのには、プログラミング上の**「とても重要な理由」**があります。

一言で言うと、**「後からやってくる大量のデータを、いつでも受け取れるように専用の場所を確保しておくため」**です。

1. 「いつデータが届くか分からない」から
    - ブラウザが「使える声のリスト（日本語、英語、男性、女性など）」を準備するのには、ほんの少し時間がかかります。

    - プログラムが動き出した瞬間：まだ声のリストは準備中。

    - 少し経った後 ブラウザから「準備できたよ！」とデータが届く。

もし最初に `let voices = [];` と場所を作っておかないと、データが届いたときに「どこに保存すればいいの？」とプログラムが迷子になってエラーが出てしまいます。

2. 「データの入れ替え」をスムーズにするため
    - ブラウザの設定や言語設定が変わると、使える声のリストも更新されることがあります。

    - 最初に []（空っぽ）にしておく。

    - データが届いたら、その中身を**「最新の声のリスト」で上書き**する。

このように、変数を「最新情報の保管場所」として使い続けるために、まずは空の状態で定義しておくのが決まり文句になっています。

3. 「エラーを防ぐ」ための安全策
    - もし配列を定義せずに、いきなり `voices.map(...)` や `voices.length` といった命令を使ってしまうと、「`voices` なんて知らないよ！」とプログラムが止まってしまいます。

**たとえ中身が空っぽでも、[] と書いておけば「今は中身はないけれど、これは配列（リスト）ですよ」とJavaScriptに教えることができます。**
**そのおかげで、中身が届く前でもエラーを出さずに安全に処理を進められるのです。**


`msg.text = document.querySelector('[name="text"]').value;` の役割
この一行は、「ユーザーが今、画面上のテキストエリアに打ち込んだ文字」を、ブラウザが喋るための「原稿」としてセットする処理です。

1. 右側`document.querySelector('[name="text"]').value`
`document.querySelector('[name="text"]')`: HTMLの中から name="text" という名前がついた要素（通常は <textarea> や <input>）を探し出します。

.value: その入力欄の中に今現在書き込まれている文字（例：「こんにちは、大分へようこそ」など）を抜き出します。

2. 左側`msg.text`
`msg` という箱（SpeechSynthesisUtterance）には、text という名前の「原稿入れ」があらかじめ用意されています。

3. =（代入）
右側で抜き出した文字（原稿）を、左側の「原稿入れ」に放り込んでいます。

**プログラムの動き（イメージ図）**
ユーザーが画面のテキストボックスに 「こんにちは」 と入力して、ボタンを押した瞬間の動きは以下のようになります。

  - 抽出 画面上の <textarea name="text"> から "こんにちは" という文字列を JavaScript 
         が掴み取る。

  - セット msg オブジェクトの text プロパティに、その文字列を書き込む。

  - 実行: この後、speechSynthesis.speak(msg) が実行されると、セットされた "こんにちは" 
         という声がスピーカーから流れる。

#### ブラウザが持っているたくさんの「声」のデータを取り出し、「英語（en）」の声だけを絞り込んで、画面のドロップダウンメニュー（選択肢）に作り替える（macの中に用意されているものをリストアップする）

1. `voices = this.getVoices();`
    `役割` ブラウザから「利用可能なすべての声のリスト」を取得して、変数 voices に代入。

    `ポイント` ここでの this は、イベントを発生させた speechSynthesis 自体を指す。

2. .`filter(voice => voice.lang.includes('en'))`
    `役割` 全種類（日本語、中国語、フランス語など）ある声の中から、英語（en）が含まれる
           ものだけを抽出しています。

    `ビジネス応用` 例えば「日本国内向けのECサイト制作」なら、ここを includes('ja')
                  に書き換えれば、日本語の声だけを表示させることができます。

3. `.map(voice => ...)`
    `役割` 抽出した「声のデータ（オブジェクト）」を、HTMLの <option> タグという
          「文字列」に変換しています。

     `中身`  ${voice.name} の部分が、実際にドロップダウンに表示される「Alex」や
            「Victoria」といった名前になります。

4. `.join('')`
     `役割`  map で作った「タグの配列」を、一つの長い文字列に連結しています。

5. `voicesDropdown.innerHTML`
  `役割`  最終的に出来上がったHTMLの塊を、画面上のドロップダウンメニュー
            の中身としてガバッと流し込んでいます。

1. `msg.voice = voices.find(...)`
    `役割` ブラウザが持っている「声のリスト（voices）」の中から、今ユーザーが選択した
           名前と一致する声を一つだけ探し出し、読み上げ用の箱（msg）の「声の設定」に上書きしています。

    `find` メソッド: 配列の中から、条件に合う最初の1つだけを見つけてくる命令です。

2. `voice => voice.name === this.value`
    `this.value` ドロップダウン（selectタグ）で今選ばれている「声の名前
                  （例：AlexやKyoko）」を指します。

    `意味` 「リストの中にいるたくさんの声たち（voice）の中で、その名前（name）が、
            今選んだ名前（this.value）と**完全に一致（===）**するものはどれ？」と
            一列ずつチェックしています。

3. `toggle();`
    `役割` 声を切り替えた直後に、**「一度音声を止めて、新しい声で喋り直させる」**という
           自作関数（toggle）を呼び出しています。

    `理由` これを入れないと、今喋っている途中の声は古いままになってしまい、次に再生ボタン
           を押すまで新しい声が反映されないからです。

#### 音声の**「停止」と「再生（やり直し）」を一つの関数で賢くコントロールする**役割

1.` toggle(startOver = true)` の意味
    `役割` 引数 startOver に、あらかじめ「標準（デフォルト）では true（やる）」
           という設定をしています。

   `メリット`  toggle() と呼び出せば、自動的に true が入り、
              **「止めてからすぐ再生」**します。

   `toggle(false)` と呼び出せば、**「止めるだけ」**にできます。

2. `speechSynthesis.cancel();`
    `役割` 今現在、ブラウザが喋っている音声を強制終了させます。
           **これがないと、ボタンが漁れたときに前に喋っているものが止まらない**

    `理由` これを入れないと、新しい声やテキストを設定しても、前の音声が終わるまで
           次が流れません。一度リセット（掃除）をするイメージです。

3. `if (startOver) { speechSynthesis.speak(msg); }`
    `役割` もし startOver が true ならば、最新の設定（msg）で喋り始めなさいという命令。
           **もし押されたのがstartOverだったらという意味も含まれている**
    `動き`  声を切り替えた時や、スライダーを動かした時は、この if 文が動いて最新の状態で
           喋り直します。

**「停止ボタン」を押した時は、あえて false を渡すことで、この if 文を通さず、「止めるだけ」の状態を作ります。**


#### 画面上の**「スライダー（速度や高さ）」や「テキスト入力欄」が動かされたときに、その値を一発で読み上げ設定に反映させる**

1. `console.log(this.name, this.value);`
    `役割` 今どのツマミを動かしたのか、デベロッパーツール（検証画面）に表示します。

    `意味` 例えば「速度（rate）」のスライダーを動かせば、コンソールに rate 1.5
           のように表示され、動作確認がしやすくなります。

2. `msg[this.name] = this.value;`（ここが最重要！）
    **`msg[this.name] = this.value;` : これはすべての拾い上げてきた要素をmsgに適用しなさいということ**

    `this.name` 動かした部品の name 属性（rate, pitch, text など）が入ります。

    `this.value` その部品の現在の値（1.5 や 「こんにちは」など）が入ります。

    `msg[...]` ブラウザが「どのプロパティを更新すべきか」を、動的に（自動で）判断。

**もしこの書き方をしなかったら…**
本来なら、以下のように部品ごとに処理を書かなければなりません。

`if (this.name === 'rate') { msg.rate = this.value; }`
`if (this.name === 'pitch') { msg.pitch = this.value; }`
`if (this.name === 'text') { msg.text = this.value; }`
しかし、msg[this.name] と書くことで、**「名前が rate なら msg.rate を、pitch なら msg.pitch を更新してね」**と、一行で全ての部品に対応させているのです。

3. `toggle();`
    `役割` 設定を書き換えた瞬間に、先ほど解説した toggle 関数を呼び出します。

    `理由` これにより、スライダーを動かした瞬間に**「新しい速度や高さ」で即座に
           喋り直し**が始まり、ユーザーは変化をリアルタイムで体感できます。

`options.forEach(option => option.addEventListener('change', setOption));`
  - 前に定義したsetOptionが変更されたらその都度setOptionを実行しなさいという認識.




---




## Day 22:JavaScript Exercise: Follow Along Links  (2026/03/22)
## 学んだこと

### **「マウスを乗せたリンクに、動くハイライト（背景の白い枠など）をぴったり重ねる」**という処理

（まずaタグを探して、そのaタグにCursorが当たったときは、`highlight`を適用してCSSで設定した動きを実行しなさいという認識）

### 用語解説
* `mouseenter` :
  - その要素に入った時だけ反応する。中の子要素（ボタンの中の文字など）にマウスが移動しても、何度も反応しない。
* `mouseover` :
  - 要素の中にある「子要素」にマウスが乗るたびに、何度もイベントが発生してしまう。

**`addEventListener` の書き方の決まり**
コードにあった `triggers.forEach(a => a.addEventListener('mouseenter', highlightLink));` も、JavaScriptの標準的な書き方のルールに従っています。

  1. `triggers`: 対象となる要素のリスト（配列のようなもの）。

  2. `forEach`: 「リストの中身すべてに同じことをしてね」という命令。

  3. `addEventListener`: 「〜という状態になったら、この関数を実行してね」という予約（イベントリスナー）。

  4. `'mouseenter'`: 反応するタイミングの名前（これが決まり文句）。

  5.` highlightLink`: 実行する関数の名前。

### 「なぜスクロールでズレるのか？」

#### 一言で言うと、「定規の目盛り（基準点）」が違うからです。
#### 足し算しかないのは、今写っている文字の今の画面からの位置はわかるので、（今の画面の左上を０，０とするため）隠れた部分の距離を足してあげると今の本当の位置になる。

2種類の「定規」をイメージしてみよう
ブラウザには、位置を測るための「定規」が2種類あります。

1. 「画面（窓）」基準の定規
`getBoundingClientRect()` が使っている定規です。

  - 基準点： 今見ているブラウザの左上角（常に 0, 0）。

  - `特徴`： あなたがどれだけページの下の方を読んでいても、**「今見えている画面の左上」**から測ります。

  - `問題点`： スクロールすると、ページの中身が上にズレていきますよね？そうすると、
              この定規で測った値（top）はどんどん小さくなってしまいます。

2. 「ページ全体（紙）」基準の定規
  - 私たちが本当に知りたい、ハイライトを置くべき場所の定規です。

  - `基準点`： ページの本当の最初（一番上）。

  - `特徴`： スクロールしても値は変わりません。

**なぜ足し算（+ window.scrollY）が必要なのか？**
  - 例えば、あるリンクがページの「上から 1000px」の位置にあるとします。

`スクロールしていない時`：
  - `getBoundingClientRect().top` は 1000 です。

`500px スクロールした時`：
  - リンクは画面の上の方に移動しているので、getBoundingClientRect().top は 500 に減ってしまいます。

このとき、そのまま「500」の位置にハイライトを出そうとすると、本来の場所（1000px地点）より上にズレて表示されてしまいます。

#### そこで算数の出番です！

「今見えている位置（500）」 ＋ 「隠れてしまった分（500）」 ＝ 「本当の位置（1000）」

これが、コードにあった `linkCoords.top + window.scrollY` の正体です。
これで、どれだけスクロールしても、常にページの「本当の場所」にハイライトをピタッと合わせることができるようになります。

#### まとめると
`getBoundingClientRect()` は 「今、画面のどこに見えているか」 を教えてくれる。

`window.scrollY` は 「どれだけ下にスクロールしたか」 を教えてくれる。

この2つを合体させることで、**「ページ上の絶対的な住所」**がわかる

1. `document.createElement('span')`
   ブラウザに対して「新しく `<span></span>`という空のタグを1つ作って」と命令します。

2. メモリ上に保持
   作成された要素は、ブラウザのメモリ（記憶領域）に一時的に置かれます。

3. 変数 `highlight` に代入
   後でこの要素に対して「色をつける」「文字を入れる」「画面に追加する」といった操作ができるように、`highlight` という名前でアクセスできるようにします。

### 処理の大きな流れ
`triggers.forEach(...)`
  - 画面内の対象となるすべてのリンク（<a>タグなど）に対して、「マウスが乗った時（mouseenter）」に highlightLink 関数を実行する準備をしています。

`this.getBoundingClientRect()`
  - 「今マウスが乗った要素（this）」の、画面上の正確な**大きさ（幅・高さ）と位置（上下左右）**を取得します。

`const coords = { ... } (座標の補正)`
- `getBoundingClientRect` で取れる位置は「画面の見えている範囲（ビューポート）」からの距離なので、ページをスクロールしているとズレてしまいます。
そのため、現在のスクロール量（window.scrollY / window.scrollX）を足して、ページ全体の絶対的な位置を計算し直しています。

`highlight.style...` (見た目の反映)
  - 計算したサイズと位置を、あらかじめ作成しておいた highlight 要素に適用します。
    幅・高さ： マウスを乗せたリンクと同じサイズにする。

`transform`： translate を使って、その場所まで highlight 要素を移動させる。

#### イメージ図
この処理によって、1つの <span>（ハイライト）が、マウスの動きに合わせてリンクからリンクへ飛び移るようなエフェクトが完成します。

`コードのポイント`：なぜ translate を使うのか？
  - top や left を直接書き換えるよりも、transform: translate() を使う方がブラウザの描画負荷が軽く、CSSで transition を設定していれば、ハイライトがヌルヌルと滑らかに動くようになります。

  - CSS側でこれを設定しておくと、移動がアニメーションになります
    `.highlight { transition: all 0.2s; position: absolute; }`



---



## Day 21:JavaScript Geolocation based Speedometer and Compass  (2026/03/21)
## 学んだこと

### **「デバイス（スマホやPC）のGPS機能を使って、現在の移動速度と進んでいる方角をリアルタイムで監視し、画面上のスピードメーターと方位磁針（アロー）を動かす」**

1. `navigator.geolocation.watchPosition(...)`
これは「一度きりの取得」ではなく、**「位置が変わるたびにずっと見守る（監視する）」**という命令です。

  - getCurrentPosition: 今の場所を一回だけ取る（地図アプリで現在地ボタンを押した時のような動き）。

  - watchPosition: 移動に合わせて何度もデータを取る（カーナビのように、自分が動くと画面のアイコンも動く状態）。

2. `(data) => { ... } (成功時の処理)`
  - 位置情報が新しく更新されるたびに、この中のコードが実行されます。引数の data（先ほどコンソールで見た GeolocationPosition）には、移動に関する詳細な数値が入っています。

`data.coords.speed:`
  - 現在の移動速度です。単位は「メートル毎秒（m/s）」です。これを speed.textContent に入れることで、画面上の数字を書き換えています。

`data.coords.heading:`
  - 北を0度とした、**進んでいる方角（方位）**です。
    0 = 北, 90 = 東, 180 = 南, 270 = 西

`arrow.style.transform = ...:`
  - CSSの rotate（回転）を使って、画面にある矢印のアイコンを、進んでいる方角に合わせてクルリと回転させています。

3. `(err) => { ... } (失敗時の処理)`
  - もし位置情報が取れなかった場合（ユーザーが許可しなかった、GPSが届かない場所にいるなど）に実行されます。
  - `console.error(err)` と書くことで、なぜ失敗したのかをコンソールに赤文字で表示してくれます。



---



## Day 20:JavaScript Speech Recognition (2026/03/21)
## 学んだこと

### 連続して記録できないため、待ち時間を作って修正
  `recognition.addEventListener('end',() => {setTimeout(() => recognition.start(), 100);});`

**なぜ contenteditable がついているのか？**
ここが JavaScript30 の面白い工夫です。contenteditable 属性がついていると、**「ブラウザ上でユーザーが直接文字を打ち替えられる」**ようになります。

 - 音声認識：自動でどんどん <p> タグと文字を書き込む。

 - 手動修正：もし音声認識が間違っていたら、キーボードでその場をカチカチっとクリックして直せる。

**まるで「AIが下書きをして、人間が仕上げをする」エディタのような仕組みになっているんですね**

### `isFinal`は、デフォルト　＝　false

### ５秒喋るのをやめたら、表示を出して、録音を停止するこーどを追加！！

`window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;`
  - まず window.SpeechRecognition（標準の名前）があるか確認します。

  - もし存在しなければ（||）、window.webkitSpeechRecognition（Chrome/Safari用の名前）を代わりに入れます。

  - その結果を、左側の window.SpeechRecognition に代入して統一します。

  - これによって、その後のコードではブラウザの種類を気にせず、常に new SpeechRecognition() と書くだけで音声認識を開始できるようになります。

1. `const recognition = new SpeechRecognition();`
【マシンの起動】
「音声認識という機能を使って、実際に動くマシンを1つ作って recognition という名前を付けます」という宣言です。
これにより、recognition に対して「録音を開始して」「停止して」といった命令ができるようになります。

2. `recognition.interimResults = true;`
【リアルタイム表示の設定】
「話している途中の、まだ確定していない結果も報告してね」という設定です。

`true（有効）の場合`： しゃべっているそばから、コンピュータが「今はこう聞こえる…あ、やっぱり
                     こうかな？」と推測している途中の文字が次々と送られてきます。

`false（無効・デフォルト）の場合`： しゃべり終わって一息つくまで、何も表示されません。

3. `recognition.lang = 'en-US';`
【言語の設定】
「今から話すのは アメリカ英語（en-US） だよ」と、聞き取る言語を指定しています。

日本語を聞き取りたい場合は 'ja-JP' と書きます。

#### マイクが拾った音をリアルタイムで文字に変換し、特定の言葉を絵文字に変え、喋り終わったら新しい段落（改行）を作る」

##### 1. 音声をテキストデータに変換する

`const transcript = Array.from(e.results)`
  `.map(result => result[0])`
  `.map(result => result.transcript)`
  `.join('');`

音声認識の結果（e.results）は、そのままでは少し扱いづらい「データの塊」です。それを以下の手順で「普通の文字列」に整えています。

 - Array.from(e.results): 特殊なデータ形式を、加工しやすい「配列」に変換します。

 - map(result => result[0]): 1つの音声に対してブラウザは複数の候補を出しますが、その中から「一番もっともらしい（自信がある）結果」だけを選びます。

 - map(result => result.transcript): 信頼度などの余計なデータを除き、「喋った内容（テキスト）」だけを抜き出します。

- join(''): バラバラな言葉の断片を、一つの文章につなげます。

##### 2. 特定の単語を「検閲」して絵文字に変える

`const poopScript = transcript.replace(/poop|poo|shit|dump/gi, '💩');`
`p.textContent = poopScript;`
ここでは、ちょっとした遊び心を加えています。

 - replace: 指定した単語を見つけて、別のものに置き換えます。

`/poop|poo|.../gi`: 「poop または poo または...」という意味の正規表現です。
                     gi は「大文字小文字を無視して、見つかったもの全部」という意味です。

`p.textContent`: 変換後のきれいな（？）文字列を、画面上の <p> タグの中に書き込みます。
                 これでリアルタイムに文字が表示されます。

##### 3. 喋り終わったら「次の行」を準備する

`if (e.results[0].isFinal) {`
  `p = document.createElement('p');`
  `words.appendChild(p);`
`}`
ここが一番賢いポイントです。

`isFinal`: ブラウザが「あ、この人は今喋り終えたな（文章が確定したな）」と判断した時に
           true になります。

`中身`: 確定したら、新しい真っ白な <p> タグを作って、掲示板（.words）に貼り付けます。

`p = ...:` 変数 p をこの新しいタグで上書きすることで、
           **「次に喋る言葉は、この新しい行に書き込んでね」**と予約しているのです。

##### `recognition.addEventListener('end',() => {setTimeout(() => recognition.start(), 100);});`

この一行は、**「音声認識が一度終わっても、0.1秒後に自動で再起動して、ずっと聞き取りを続けさせる」**というループ（繰り返し）の仕組みを作っています。

##### 1. addEventListener('end', ...) の役割
【お仕事が終わった合図を待つ】
音声認識（recognition）は、しゃべり終わって沈黙が続くと、自動的に「お仕事終了（end）」という状態になります。
このコードは、「終了（end）した瞬間に、次の命令を出してね」という待ち伏せをしています。

##### 2. () => { ... }（アロー関数）の役割
【次にやることを包む】
ここでは「終了したら、また開始（start）してね」という指示を関数の中に包んでいます。
前にお話しした通り、直接 recognition.start と書かずにこの形で包むことで、「誰が開始するのか（recognitionオブジェクト）」という情報を正しく保持したまま命令を実行できます。

##### 3. setTimeout(() => ..., 100) の役割
【0.1秒の「ひと休み」を入れる】
これが一番のポイントです。

`なぜ待つのか`： ブラウザが「前の録音を完全に片付けて、次の録音の準備を整える」ための**わずかな時間（バッファ）**を作っています。

`100ミリ秒（0.1秒）`： 人間には一瞬ですが、コンピュータにとっては十分な準備時間です。

**5秒喋るのをやめたときのコードの注釈**

1. 変数の定義 (let silenceTimer)
JavaScriptでは、値を入れる前に「この名前の変数を使いますよ」と宣言する必要があります。宣言せずに clearTimeout(silenceTimer) を実行すると、「そんな名前のデータは知りません！」というエラー（ReferenceError）になります。

2. 自動再開フラグ (isAutoRestartEnabled)
ここが重要です！
元のコードでは、end イベントが起きると常に recognition.start() を呼ぶ設定になっていました。
5秒経って recognition.stop() が動いても、ブラウザは「あ、終わった（end）んだな」と判断して、また start() を呼んでしまいます。

そこで、「5秒経ったらフラグ（旗）を下ろす」という処理を加え、**「旗が立っている時だけ再開する」**という条件分岐（if）を追加しました。



---



## Day 19: Unreal Webcam Fun with getUserMedia() and HTML5 Canvas (2026/03/20)
## 学んだこと

## 「まず package.json が手元にあり、その中身（レシピ）を見て npm install が材料（ツール）を揃えてくれる」**という流れ

### 速度を早くするために、変数の定義を変更
`const ctx = canvas.getContext('2d', { willReadFrequently: true });`

###  <button onClick="takePhoto()">Take Photo</button>
  **「HTMLの属性を使って、特定のJavaScript関数を呼び出す設定」**をしています。
  専門用語では、これを 「インラインイベントハンドラー」 と呼びます。

`canvas`
  - HTMLに記述した <canvas id="myCanvas"></canvas> 要素をJavaScript側で取得した変数です。これは単なる「枠」や「画用紙」に相当します。

`.getContext('2d')`
  - 画用紙（canvas）に対して、**「どんな道具を使って描きたいか」**を指定するメソッドです。

`'2d'`: 2次元（平面）の図形やテキスト、画像を描画するためのメソッド群を呼び出します。

`const ctx`
  - 取り出した「筆セット（描画機能）」に ctx という名前をつけて保存しています（contextの略としてよく使われます）。

`navigator.mediaDevices`
  - ブラウザが持っている「カメラやマイクなどのデバイスを管理する機能」にアクセスします。

`.getUserMedia(...)`
  - 「ユーザーのメディア（映像・音声）を使わせて！」と要求するメソッドです。これを実行すると、ブラウザの画面上部に**「カメラの使用を許可しますか？」というポップアップ**が表示されます。

`{ video: true, audio: false }`
  - 「何を使いたいか」の条件設定（制約）です。

`video: true`: カメラ映像を取得する。
`audio: false`: マイク音声は取得しない。

`video.srcObject = localMediaStream;`
  - **「カメラの映像（ストリーム）を、ビデオプレーヤーに繋ぐ」**という意味です。

`video`: HTMLにある <video> タグをJavaScriptで取得した変数です。
`srcObject`: ビデオタグが持っている、メディアオブジェクト（映像データそのもの）を受け取る
             ための専用の「窓口」です。
`localMediaStream`: 前のステップで getUserMedia から受け取った、カメラからの生の映像
                    データです。
#### ポイント：
普通の動画ファイル（mp4など）なら video.src = "movie.mp4" と書きますが、カメラのような「常に流れ続けている生データ」の場合は、この `srcObject` を使って接続します。

`video.play();`
  - **「ビデオプレーヤーの再生を開始する」**という意味です。

実は、データを繋いだ（srcObject に代入した）だけでは、ビデオプレーヤーは一時停止状態のままです。このメソッドを呼ぶことで、初めてカメラの映像がパラパラ漫画のように動き出し、リアルタイムの映像が画面に表示されます。

`console.error(`OH NO!!!`, err);`
  - 第1引数: エラー発生場所を特定するための「叫び声（目印）」。
  - 第2引数: ブラウザが教えてくれる「エラーの詳細データ（証拠）」。

### 「ビデオの映像をリアルタイムでキャプチャし、1コマずつ画像加工（フィルター処理）を施してキャンバスに描き直す」**という、非常にエキサイティングな処理を行っています。

1. 準備：キャンバスのサイズをビデオに合わせる
`JavaScript`
`const width = video.videoWidth;`
`const height = video.videoHeight;`
`canvas.width = width;`
`canvas.height = height;`
  まず、カメラ映像（video）の実際の幅と高さを取得し、それをキャンバスのサイズに設定します。これで、画用紙と映像のサイズがピッタリ重なります。

2. メイン処理：16ミリ秒ごとのループ
`return setInterval(() => { ... }, 16);`
  setInterval を使って、**約16ミリ秒（1秒間に60回）**のペースで中の処理を繰り返します。パラパラ漫画のように高速で描画を更新することで、滑らかな動画に見えるようになります。

3. 画像加工の3ステップ
  - 映像をコピーする (`drawImage`)
    ビデオの現在の1フレームを、キャンバスにそのまま描き込みます。

  - ピクセルデータを「取り出す」 (`getImageData`)
    キャンバスに描いた画像を、膨大な数の「点（ピクセル）の色の数値データ」として変数 pixels に格納します。

  - データを「いじる」 (rgbSplit など)
    ここが一番面白いところです！

`rgbSplit(pixels)`：赤・緑・青の色情報を少しずつズラして、サイケデリックな残像エフェクトを
                   作っています。

（コメントアウトされていますが）redEffect なら画面を赤くしたり、greenScreen なら特定の背景を透明にしたりできます。

  - 加工したデータを「戻す」 (putImageData)
    色を塗り替えたピクセルデータをキャンバスに上書きします。
    これで加工後の映像が画面に表示されます。

`ctx.putImageData(pixels, 0, 0);}, 16);`
### パラパラ漫画の「新しいページをめくって、みんなに見せる」という、描画プロセスの**フィナーレ（書き出し）と、そのスピード（更新頻度）**を決めています。

ここを動かさない限り、どれだけ裏で画像を加工しても画面には反映されません。

1. `ctx.putImageData(pixels, 0, 0);`
**「加工し終わったピクセルデータを、キャンバスにバシッと貼り付ける」**という命令です。

`ctx`: 最初に取り出した「筆セット（2Dコンテキスト）」ですね。
putImageData: メモリ上でいじり倒した「色の数字の塊（pixels）」を、実際のキャンバスの絵として描き戻す専用のメソッドです。

`pixels`: 直前の工程で、赤色を強めたり、RGBをズラしたりして改造したデータ本体です。

`0, 0`: 貼り付ける位置（座標）です。キャンバスの左上端（x=0,y=0）からピッタリ貼り合わせます。

例えるなら：
getImageData でパズルをバラバラにして色を塗り替え、この putImageData で完成したパズルを額縁（キャンバス）に戻すイメージです。

2. `}, 16);`
これは setInterval（一定時間おきに繰り返す関数）の締めくくりで、**「次のコマを描くまでの待ち時間」**を指定しています。

16: 単位はミリ秒（ms）です。16ミリ秒（0.016秒）ごとに、この関数全体を最初から実行しなさい、という意味です。

**なぜ「16」なのか？**
数学的にとても理にかなった数字です。
私たちが普段見ているテレビやYouTubeの多くは、1秒間に60回画面が切り替わります（60fps）。

1000ms÷60回≈16.66ms
つまり、16ミリ秒ごとに描き直せば、人間の目には「滑らかな動画」に見えるというわけです。

### **「デジカメのシャッターを切って、現像した写真をアルバムに並べる」**という一連の動作をプログラムで再現
1. シャッター音を鳴らす
`snap.currentTime = 0;`
`snap.play();`
`snap`: 事前に読み込んでおいた「カシャッ」という音声ファイル（Audio要素）です。

`currentTime = 0`: 再生位置を強制的に「最初」に戻します。これにより、連写しても音が重ならず、毎回最初から「カシャッ」と鳴ります。

2. キャンバスから「写真データ」を書き出す
`const data = canvas.toDataURL('image/jpeg');`
`toDataURL`: これが魔法のメソッドです！

#### 現在 canvas（作業台）に映っているその瞬間の絵を、**「長い文字列のデータ（Data URL）」**に変換します。これが写真の「中身」になります。

3. ダウンロード用の「リンク」を作る
`const link = document.createElement('a');`
`link.href = data;`
`link.setAttribute('download', 'handsome');`
<a> タグ（リンク）をメモリ上に作成し、そのリンク先に先ほどの写真データを指定します。

`setAttribute('download', 'handsome')` とすることで、このリンクをクリックしたときに **「handsome.jpg」という名前で保存** されるようになります。

4. アルバム（strip）に写真を追加する
`link.innerHTML = `<img src="${data}" alt="Handsome Man" />`;`
`strip.insertBefore(link, strip.firstChild);`
作成したリンクの中に、写真のミニチュア（<img>）を入れます。

`insertBefore`: これにより、新しい写真が**常に一番左（または一番上）**に来るように追加されます。

### 映像の色（赤・緑・青）をわざとバラバラにずらして、サイケデリックな残像エフェクトを作る」**という、デジタルアートのような処理

1. `pixels.data` の中身はどうなっている？
まず、getImageData で取得した pixels.data は、巨大な数字の列（配列）です。
1つのドット（ピクセル）の色は、[赤, 緑, 青, 透明度] の4つの数字でセットになっています。

i + 0: 赤 (Red)
i + 1: 緑 (Green)
i + 2: 青 (Blue)
i + 3: 透明度 (Alpha)
i += 4 でループ回しているのは、この4つセット（1ピクセル分）を飛ばしながら処理するためです。

2. 何をして「ずらして」いるのか？
`pixels.data[i - 150] = pixels.data[i + 0];`  RED を「かなり前」のピクセルに放り込む
`pixels.data[i + 500] = pixels.data[i + 1];`  GREEN を「かなり後ろ」のピクセルに放り込む
`pixels.data[i - 550] = pixels.data[i + 2];`  BLUE を「さらに前」のピクセルに放り込む
 本来: i 番目のピクセルの「赤」は、i 番目の場所にいるはずです。
`このコード`: i 番目の「赤」の値を、**150個手前の場所（i - 150）**に無理やり書き込む。

**これを全ピクセルで実行すると、画面全体で**「赤い影が左に、緑の影が右に……」**といった具合に、それぞれの色が本来の位置からバラバラに飛び出したような見た目になります。**

3. このコードの「ワイルド」な点
このコードには少し強引な部分があります。

インデックスがマイナスになる可能性: 最初のほうのピクセルで i - 150 をすると、配列の範囲外（マイナス）を指定してしまいます。JavaScriptはこれでもエラーで止まらずに「無視」してくれますが、少し特殊な書き方です。

重なり: 色をずらして上書きしていくので、元の映像がぐちゃぐちゃに混ざり合い、独特の「色ズレ（クロマティック・アベレーション）」が発生します。



---



## Day 18: How JavaScript's Array Reduce Works  (2026/03/19)
## 学んだこと

### 全体的には、全部をまず秒にしていまい、それを全部足し、次にまた時間、分、秒という表示に治すという工程

### parseFloat は、**「文字列を浮動小数点数（小数を含む数値）に変換する」**ためのJavaScriptの組み込み関数です。

 - プログラミングでは、見た目が数字の "10" や "5.5" でも、引用符で囲まれていると
   コンピューターはそれを「文字（テキスト）」として扱う。
   これを「計算ができる数値」に作り替えるのが `parseFloat`の役割。

### dataset.time が返すのは必ず「文字列」
 - HTMLの属性（id, class, data-* など）に書き込まれた値は、プログラムの世界ではすべて 「文字列（String）」 として扱われます。

 - たとえHTMLに data-time="20" と数字っぽく書いてあっても、JavaScriptが dataset.time で読み取った瞬間、それは数値の 20 ではなく、引用符がついた "20" というテキストになります。

### .map は「同じ数の新しい配列」を作る
 - .map というメソッドの仕事は、**「元の配列の全アイテムに対して、指示通りの加工をして、新しい配列を作る」**ことです。

### 今回の流れをスローモーションで見るとこうなります：

 - 元の材料 (timeNodes): [<li>要素A</li>, <li>要素B</li>]

 - 加工指示: node => node.dataset.time （要素から data-time の中身だけ取ってこい！）

 - 1つ目の要素を加工: <li>要素A</li> の data-time は "5:42" だな。

 - 2つ目の要素を加工: <li>要素B</li> の data-time は "12:10" だな。

  **新しい配列が完成**:
   結果、加工された中身が並んだ ["5:42", "12:10"] という「文字列の配列」ができあがります。

### .reduce() の仕組み（そろばんイメージ）
 - このメソッドは、配列の中身を一つずつ取り出して、用意した「集計用の変数」に次々と足し込んでいく。

 - total (累計値):
 「これまでの合計」を覚えておく変数です。最初は配列の1番目の値が入ります。

 - vidSeconds (現在の値):
 「今、配列から取り出したばかり」の数値です。

 - total + vidSeconds (計算内容):
 「これまでの合計」に「新しい数値」をプラスして、次のステップへ渡します。


`const timeNodes = Array.from(document.querySelectorAll('[data-time]'));`
  - `data-time`属性を持つすべての要素を取得し、配列に変換して`timeNodes`に格納。

`const seconds = timeNodes`
  - 最終的な合計秒数を格納するための変数`seconds`を定義し、以下の処理（メソッドチェーン）を
   開始。
   **今ある材料で配列の中身を作ったのでsecondsに入れるという感覚**

`.map(node => node.dataset.time)`
  - 各要素から`data-time`属性の値（例: "5:42"）だけを取り出した文字列の配列を作る。

`.map(timeCode => {`
  - 取り出した時間文字列`（timeCode）`を秒数に変換する処理を開始。

`const [mins, secs] = timeCode.split(':').map(parseFloat);`
  - 文字列を:で分割し、それぞれを数値（分と秒）に変換して変数に代入。
  **timeCodeは使い捨ての名前**

`return (mins * 60) + secs;`
  - 「分」を60倍して「秒」を足し、その動画の総秒数を返す。

`.reduce((total, vidSeconds) => total + vidSeconds);`
  - 配列内のすべての秒数を足し合わせ、1つの大きな合計秒数（単一の値）にします。
  **前に.reduceをつけると、第１引数は合計（ここではtotalと名付ける）第２引数は次から出てくる数字（ここではvidSecondと名付ける）その計算式としては、total + vidSecondsとするという認識**

`let secondsLeft = seconds;`
  - 計算用に、合計秒数を`secondsLeft`という変数にコピーします。

`const hours = Math.floor(secondsLeft / 3600);`
  - 合計秒数を3600で割り、小数点以下を切り捨てて「時間」を算出します。
  **Math.floor() の役割**
  Math.floor() は、カッコ内の数値の小数点以下を切り捨てて、整数にする関数です。
  普通に割り算をすると、多くの場合で小数が出てしまいます。
  例：5400秒（1.5時間）の場合
  5400 / 3600 = 1.5
  しかし、時計の表示で「1.5時間」とは言わず、「1時間と◯分」と言いますよね。
  まずは「1時間」というキリの良い数字だけを取り出したいので、.5 を切り捨てて 1 にするために Math.floor を使います。

`secondsLeft = secondsLeft % 3600;`
  - 「時間」を引いた残りの秒数を、剰余演算（%）で算出して更新します。
  **%は余りを求めるということ**

`const mins = Math.floor(secondsLeft / 60);`
  - 残りの秒数を60で割り、小数点以下を切り捨てて「分」を算出します。

`secondsLeft = secondsLeft % 60;`
  - 「分」を引いた残りの秒数を算出し、最終的な「秒」を確定させます。

`console.log(hours, mins, secondsLeft);`
  - 算出された「時間」「分」「秒」をコンソールに出力します。

**補足**：
  計算の仕組みこのコードの後半は、大きな秒数を読みやすい単位に分解する「繰り下がり」の計算をしています。
  - 1時間 = 3600秒 ($60 \text{分} \times 60 \text{秒}$)
  - 1分 = 60秒
  例えば、合計が 3725秒 だった場合：
    1. 3725 / 3600 = 1時間（余り 125秒）
    2. 125 / 60 = 2分（余り 5秒）
    3. 結果：1 2 5 と出力される。




---




## Day 17: JavaScript Practice: Sorting Band Names without articles (2026/03/18)
## 学んだこと

### 🔍 1. /^(a |the |an )/i の正体（正規表現）
  - このスラッシュ / で囲まれた部分は**「正規表現（Regular Expression）」**といい、特定の文字パターンを探し出すためのルール。

  - ^: 「行の先頭」という意味です。名前の途中に the があっても無視し、**「一番最初にある時だけ」**反応します。

  - (a |the |an ): 「a か the か an（後ろに半角スペースあり）」のどれか、という意味です。

  - /i: 「大文字・小文字を区別しない」という命令です。The も the も同じように見つけます。

### 🛠️ 2. .replace(..., "") と .trim() の役割
  `.replace(..., ""):`
  - 見つかった「A 」や「The 」を、空っぽの文字（""）に置き換えて消去します。
  - 例: "The Plot in You" → "Plot in You"

  `.trim():`
  - 念のため、前後に残ってしまった余計な空白（スペース）を綺麗に掃除します。

### 💡 3. なぜ「元のバンド名」は消えないのか？
  - この return は、「並び替えの計算機（sort関数）」に渡すための一時的な値を作っている。
  - 計算機の中: 「よし、The Plot in You は Plot（P）として扱うぞ」と判断。
  - 画面の表示: 並び替えが終わった後、画面には元の The Plot in You がそのまま表示。


### ⚙️ 1. sort 関数の仕組み：2人ずつの勝ち抜き戦
  - `sort((a, b) => ...)` という書き方は、配列の中の要素を 「2つずつ（aとb）取り出して、どっちを前にするか決める」 というトーナメント戦のような動き。
  - a と b: 配列 bands の中から選ばれた2つのバンド名。

  `strip(a) > strip(b)`:
  - ここで先ほどの strip 関数が登場します！
  - 「The Plot in You」と「An Ocean Between Us」を比べる時、Plot と Ocean のどっちがアルファベット順で後ろか？ を判定しています。

### ⚖️ 2. ? 1 : -1（三項演算子）のルール
  - JavaScriptの sort には、返り値によって並び順を決めるルールがあります。
    1 を返す: 「a を b より 後ろ にしてね」という意味。
    -1 を返す: 「a を b より 前 にしてね」という意味。

  - つまり、strip(a)（Aの加工後）が strip(b)（Bの加工後）より大きければ（アルファベット順で後ろなら）、1を返して順番を入れ替える、という処理を一瞬で全件分繰り返しているのです。

### 💡 3. なぜ「元の名前」で並んでいるように見えるのか？
  - 判定（裏側）: strip 関数を使って「冠詞なし」で大きさを比べる。
  - 結果（表側）: 並び替えが終わった sortedBands に入っているのは、strip する前の 「元のバンド名（Theなどが付いたまま）」 です。

### 🛠️ 3つのステップで分解解説
このコードは、左から右へ「3段階の変身」を遂げています。

1. `.map((band) => ...)`（HTMLのタグで包む）
  - 並び替えが終わった sortedBands という配列（データの塊）を、一つずつ取り出して <li> タグという「封筒」 に入れ直しています。

    変換前: ['Plot', 'Ocean']
    変換後: ['<li>Plot</li>', '<li>Ocean</li>']（HTML文字列の配列になります）

2. `.join("")`（一つの長い文字列に合体させる）
  - 配列のままだと、ブラウザは画面に表示できません。.join("") を使うことで、バラバラだった <li> の封筒を**アロンアルファでくっつけて、一つの長い「HTMLの文章」**に変えます。

    合体後: "<li>Plot</li><li>Ocean</li>"

3. `.innerHTML = ...`（画面の「枠」に流し込む）
  - 最後に、HTML側にある <ul id="bands"></ul> という空っぽの枠の中に、完成した長い文章を ドサッと流し込みます。

### 💡 なぜ join("") が必要なのか？
  - もし .join("") を忘れると、JavaScriptは配列を無理やり文字列にしようとして、要素の間に「,（カンマ）」勝手に入れてしまいます。
    失敗例: <li>Plot</li> , <li>Ocean</li>（画面にカンマが出てしまう！）
    成功例: join("") で「何も挟まずにくっつけろ」と命令することで、綺麗なリストになります。




---




## Day 16: CSS Text Shadow on Mouse Move Effect (2026/03/18)
## 学んだこと

### **Math.round()** は、JavaScriptで**「数値を四捨五入して、一番近い整数にする」**ための魔法の命令。
  - Math.round(4.4); // 結果は 4 （切り捨て）
  - Math.round(4.5); // 結果は 5 （切り上げ）
  - Math.round(4.6); // 結果は 5 （切り上げ）

### ${xWalk * -1}px の中心にある * -1 は、**「数値の符号をひっくり返す」**という役割
  - xWalk が 10（右に10px移動）なら → -10（左に10px移動）
  - xWalk が -20（左に20px移動）なら → 20（右に20px移動）

### 🖱️ マウスの動きをキャッチする設定
`hero.addEventListener("mousemove", shadow);`
### heroという範囲で、アウスが動いたときはshadowという私が示した関数を実行しなさいということ
 - 場所 **hero**: マウスを見張る範囲。
 - きっかけ **mousemove**: マウスが1ピクセルでも動いたら反応する。
 - やること **shadow**: あらかじめ作っておいた「影を動かす魔法（関数）」を呼び出す。

`const { offsetWidth: width, offsetHeight: height } = hero;`
**「今、画面に表示されている hero というエリア（文字が入っている大きな枠）の実際の幅と高さ」**を測って、それを width と height という名前の変数（箱）に入れて保存しておく、という意味

### 🛠️ コードの1行解説

1. 要素の取得と定数の設定
`const hero = document.querySelector('.hero');`
  - 目的: エフェクトを監視する一番外側の大きな枠（舞台）を取得。

`const text = hero.querySelector('h1');`
  - 目的: 実際に影を動かす対象となる「文字」を取得。

`const walk = 500;`
  - 目的: 影が動く最大の「歩幅（500px）」を定義します。この数字を変えると影の勢いが変わる。

2. 関数 shadow(e) の開始
`function shadow(e)`
  - 目的: マウスが動いた時に実行する命令書を作る。(e) でマウスの最新レポートを受け取る。

`const { offsetWidth: width, offsetHeight: height } = hero;`
  - 目的: hero エリアの今の「横幅」と「縦幅」を測って、計算用の変数に入る。

`let { offsetX: x, offsetY: y } = e;`
  - 目的: レポート e から、マウスの今の「横の位置」と「縦の位置」を取り出す。

3. 座標の補正（重要なロジック）
`if (this !== e.target)`
  - 目的: カーソルの位置とheroの位置が完全一致しているかチェック。
  - これをしていないと文字が飛んでってしまう

`x = x + e.target.offsetLeft;`
  - 目的: 文字の上に乗ると座標がリセットされるため、文字が左端からどれだけ離れているかを
         足して修正。

`y = y + e.target.offsetTop;`
  - 目的: 同様に、文字が上端からどれだけ離れているかを足して、常に全体の左上からの距離に統一

4. 影の移動距離の計算
`const xWalk = Math.round((x / width * walk) - (walk / 2));`
  - 目的: マウスの位置を「中心からどれくらい離れているか」の数字に変換し、四捨五入して整数の
    ピクセル値にする。
  ### 左端からとなっているものを２で割ることによって中心から−５０から５０というふうに変換しているという考え方

`const yWalk = Math.round((y / height * walk) - (walk / 2));`
  - 目的: 縦方向についても同様に、中心からのズレを計算します。

5. CSSへの反映（仕上げ）
`text.style.textShadow = \ ... `;``
  - 目的: 計算した数字を、CSSの text-shadow プロパティに流し込みます。

`${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),`
  - 目的: 1つ目の影（ピンク）を配置します。

`${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),`
  - 目的: 2つ目の影（水色）を横方向に反転させて配置します。

`${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),`
  - 目的: 3つ目の影（緑）を縦横入れ替えて配置します。

`${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)`
  - 目的: 4つ目の影（青）をさらに別の角度に配置します。




---




## Day 15: How LocalStorage and Event Delegation work. (2026/03/17)
## 学んだこと

### 💾 なぜ LocalStorage が必要なのか
- **データの永続化**：ブラウザを閉じたり更新したりしても、データを消さないため。
- **仕組み**：
  1. `JSON.stringify` で配列を「文字」にして保存する。
  2. `JSON.parse` で「文字」を「配列」に戻して読み込む。
- **重要**：JavaScriptの変数だけでは、リロードした瞬間にすべてリセットされてしまう。

### 💡 イベントリスナーのスマートな書き方
`addItems.addEventListener('submit', addItem);`

- **意味**: 送信イベントが発生した時に、別途定義した `addItem` 関数を実行する。
- **メリット**:
  - コードが読みやすくなる（可読性の向上）。
  - 関数を他の場所でも再利用できる。
- **注意点**: 
  - 関数名の後ろに `()` はつけない！
  - `()` をつけると、イベントを待たずに「その瞬間」に動いてしまう。

## 📝 populateList 関数の中身
データ（JavaScript）から見た目（HTML）を作る「レンダリング」の仕組み。

- **`map()`**: 配列をループして、各要素を HTML 文字列に変換する。
- **`${plate.done ? "checked" : ""}`**: 「三項演算子」を使い、データの状態（true/false）
  に合わせてチェックの有無を切り替える。
- **`.join("")`**: `map` が作った配列の破片を、一つの文字列に連結する。
- **`data-index`**: 各要素に番号を振ることで、後から「どの項目がクリックされたか」を特定できる
  ようにしている。

### 💡 なぜ .join("") が必要なのか？
- **配列から文字列への変換**: `map` の結果は「配列」なので、そのままHTMLに入れると要素の間の 
  ` , `（カンマ）まで表示されてしまう。
- **解決策**: `.join("")` を使うことで、カンマを消して一つの連続したHTML文字列に合体させる
  ことができる。
- **おまじない**: JavaScriptでリスト（liタグなど）を生成するときは、`map` と `join("")` は
  必ずセットで使う！

## 🔄 toggleDone 関数：状態の切り替え
クリック一つで「データ」と「画面」の両方を更新する仕組み。

1. **イベント委譲**: `matches("input")` を使い、子要素（input）のクリックを検知する。
2. **データの反転**: `!item.done` で、true/false を交互に入れ替える（トグル処理）。
3. **同期（シンクロ）**:
   - **LocalStorage**: 変更をブラウザに保存。
   - **DOM（画面）**: `populateList` を再実行して見た目を最新にする。
- **ポイント**: 画面を直接いじるのではなく、「データを変えてから画面を再描画する」という流れが
              重要。

### 🎯 target プロパティ（決まりごと）
- **意味**: イベント（クリックや入力など）が「実際に発生した場所」を指す、ブラウザ規定のプロパティ。
- **イメージ**: 「事件の現場」や「クリックの的（まと）」。
- **役割**:
  - 親要素にセンサーを付けていても、実際に触られた子要素をピンポイントで特定できる。
  - `e.target` と書くことで、その要素の色を変えたり、番号（index）を読み取ったりできる。
- **注意**: これはJavaScriptの仕様で決まっている名前なので、`e.mato` や `e.genba` と書く
           ことはできない。

### 💾 データの永続化（データの復元）
`const items = JSON.parse(localStorage.getItem("items")) || [];`

- **目的**: ページを更新したりブラウザを閉じたりしても、以前のリストが消えないようにするため。
- **JSON.parse**: 文字列として保存されていたデータを、再びJavaScriptの配列オブジェクトとして
                  扱えるように復元する。
- **|| [] (初期値)**: データが存在しない初回起動時でもエラーにならないよう、空の配列をセット
                     する「ガード」の役割。

- e.preventDefault();
  画面の再読み込みをさせない

- const text = ...value;
  お客さんが入力した文字（例えば「エビのアヒージョ」など）を、変数 text という紙にメモする。

- const item = { text, done: false };
  ただの名前だけじゃなくて、「名前は『エビのアヒージョ』、まだ食べてないよ（done: false）」という情報がセットになった、専用の注文カードを1枚作る。
  （後でチェックを入れるために、done: falseが必要）

- items.push(item);
  作ったカードを、お店の注文ボックス（items という配列）の中にポイっと入れる。

- populateList(items, itemsList);
  ボックスの中にあるカードを全部見て、お客さんに見えるようにホワイトボード（画面）にきれいに書き直す作業。
  itemsは全体、itemListは各注文した料理というふうに定義しているということ.

- localStorage.setItem('items', JSON.stringify(items));
  #### ★ここが一番大事！
  「魔法の金庫に保存する」
  これが今回の「データベース」の代わり。

  JSON.stringify
  注文ボックスを「魔法のシュレッダー」にかけて、一本の長い糸（テキスト）にする。

  localStorage.setItem
  その糸をブラウザの「秘密の金庫」にしまいます。
  なぜ？：これをしておけば、パソコンの電源を切っても、次に開いたときに注文が残っているから。

- this.reset();
  次の注文を書きやすいように、入力した文字をきれいに消して空っぽにする。

1. plates.map((plate, i) => { ... })
 「カードを一枚ずつ取り出して、作り替える」
  map は、配列（注文カードの束）の中身を一つずつ取り出して、新しい形に加工する。

  plate: いま手に取っている1枚のカード（「エビ」とか「生ハム」）のこと。

  i: そのカードが何枚目かという「番号（0, 1, 2...）」のこと。

2. return `<li> ... </li>`
  「HTMLの着せ替えをする」
  取り出したデータ（文字）を、HTMLのタグにする。

  ${plate.text}: カードに書いてある料理名を表示。

  ${plate.done ? "checked" : ""}: もし done が true なら、チェックボックスに最初から
  チェックを入れる。

3. .join("")
  「バラバラの文字をつなげて一本にする」
  map で加工した直後は、まだ「バラバラのHTMLの破片」の状態。
  これを .join("") という糊（のり）を使って、一つの長い長い「HTMLの文章」につなぎ合わせる。

4. platesList.innerHTML = ...
  「ホワイトボードに一気に貼り付ける」
  最後に、完成した「長い文章」をホワイトボード（platesList）にペタッと貼り付ける。
  これで画面にメニューが表示される。

💡 なぜ data-index=${i} があるのか？
  これ、実はとっても大事な「名札」。
  あとでチェックボックスをクリックしたとき、「それは何番目のメニューなの？」とコンピュータが迷わないように、「これは ${i} 番目の料理だよ！」という番号札をつけている。

- if (!e.target.matches("input")) return;
  **「関係ない場所（余白など）をクリックしても無視してね」**という門番。チェックボックス（input）をクリックしたときだけ先に進む。

- const index = el.dataset.index;
  ここで「名札」が役に立つ！クリックされたのが**「何番目の料理か」**を番号札（index）から読み
  取る。

- items[index].done = !items[index].done;
  ここが魔法の瞬間 : ! は「反対」という意味なので、true なら false に、false なら true に
  ひっくり返す。

- localStorage.setItem(...)
  書き換わった最新のリストを、すぐに**「金庫（LocalStorage）」**に保存し直す。

- populateList(items, itemsList);
  最後に、新しいデータを使って**「画面を書き直しなさい」**と命令。これでチェックマークが付いたり消えたりする。

- なぜ「!（エクスクラメーション）」を使うの？
  !items[index].done という書き方は、プログラミングでよく使われる「スイッチ切り替え」のテクニック。
  もし今が false（未完了）なら → !false は true になる。
  もし今が true（完了）なら → !true は false になる。
  これ一行で、**「今とは逆の状態にする」**という便利な動きが作れる。

- なぜ最後にまた populateList を呼ぶの？
  これを忘れると、**「データ（金庫の中身）は変わっているのに、画面の見た目が変わらない」**というちぐはぐな状態になってしまう。
「データを変えたら、必ず画面も更新する」というのが、このアプリの鉄則。




---




## Day 14: JavaScript Fundamentals: Reference VS Copy (2026/03/17)
## 学んだこと

- let age = 100;
  age という箱に 100 が入る。

  let age2 = age;
  age の中身（100）をコピーして、新しい age2 という箱に入る。

  重要： ここで二つの箱のつながりは切れる。

  age = 200;
  age の箱の中身を 200 に書き換える。

  age2 は別の箱なので、100のまま影響を受けない。

- 配列の要素の変更は、矢印の先に表示されるようになっているので動画と挙動が違う

## JavaScriptの「参照」と「コピー」のまとめ
### このドキュメントでは、JavaScriptを扱う上で最も重要な「値の渡し方」と「安全なデータのコピー方法」についてまとめています。

 1. 基本的な考え方JavaScriptには、データの種類によって**「コピーのされ方」が2種類**あります。データの種類性質内容プリミティブ型値のコピー数値、文字列、真偽値など。代入すると完全に別のものになる。オブジェクト型参照のコピー配列、オブジェクトなど。代入すると「同じ場所」を指し示す。
 
 2. 配列のコピー（Array Copying）const team = players; とすると、同じ配列を共有してしまい、片方を変えるともう片方も壊れます（副作用）。これを防ぐための4つのコピー方法です。

 JavaScriptconst players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];

// --- 安全なコピー（新しい配列を作る） ---

// 1. slice() を使う
const team2 = players.slice();

// 2. [].concat() を使う
const team3 = [].concat(players);

## ✅ 推奨：一番おすすめ
// 3. スプレッド構文 (...) を使う
const team4 = [...players];

// 4. Array.from() を使う
const team5 = Array.from(players);

3. オブジェクトのコピー（Object Copying）オブジェクトも同様に、直接の代入は「参照」になります。

JavaScriptconst person = {
  name: 'Wes Bos',
  age: 80
};

  1. Object.assign を使う
  {}（空の箱）に、personの中身を流し込み、さらに追加・上書きをする
  const cap2 = Object.assign({}, person, { number: 99, age: 12 });

  2. オブジェクトのスプレッド構文を使う
  const cap3 = { ...person };

4. 注意点：浅いコピー（Shallow Copy）上記の方法（スプレッド構文やObject.assign）は、すべて**「1階層目だけ」**をコピーします。問題点： 配列やオブジェクトの中に、さらに配列やオブジェクトが入っている場合、その「中身の中身」は参照のままつながっています。解決策（Deep Copy）： 完全に別物にするには、一度文字列にしてから戻す手法が使われます。JavaScript// 完全に独立したコピー（ディープコピー）を作る
const dev2 = JSON.parse(JSON.stringify(wes));

- 現場での教訓「元のデータは変えない」（イミュータブル）が鉄則。データを加工する際は、まずコピー
  を作ってから作業する癖をつけること。意図しないデータの書き換えは、発見しづらいバグの最大の原因
  になる。




---




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




---




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




---




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




---




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




---




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




---




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




---




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




---




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




---




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




---




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




---




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




---




## Day 2: JS + CSS Clock (2026/03/06)
##　学んだこと
- 線を中心に回転させる変更点
- `setInterval` の使い方（引数は、2個になる、3個にもできる）
- 角度の計算式 `(s / 60) * 360`
- 時計の針は、角度で表して適用する
- 角度の計算式 に +90をして調整する方法
- 現在の時間を呼び出す方法
- CSSで実装したtransformの変更方法




---




## Day 1: JavaScript Drum Kit (2026/03/05)
##　学んだこと
- キーボードのイベント取得。
- 音を鳴らすコード
- 実装による期待されたものが終了したらもとに戻す方法
- キーボードをターゲットにする方法
