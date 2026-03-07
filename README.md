# JavaScript30 学習記録

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
