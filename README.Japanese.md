[English Version is here.](README.English.md)

# C# コーディング ガイドライン (2025年度版)

## 概要
「C# コーディング ガイドライン」は、C#コードの読みやすさ、一貫性、チームでの共同作業を促進することを目的としています。このガイドラインは、最新のC#言語機能の活用を重視し、変数やメソッド名の名前付け規則、コードのレイアウトと書式設定、例外処理やコレクションの扱いといった推奨されるコードの書き方に関する具体的な指針を示しています。また、パフォーマンスに関する考慮事項や、.editorconfigファイルやコード分析といったツールの活用についても触れており、最終的にチームで合意した上で柔軟に適用することが重要であると結んでいます。

## 主なトピック

- コーディングガイドライン
- 命名規則
- レイアウト書式設定
- コードの書き方
- ツールの活用

## C# コーディング ガイドライン

### 1\. はじめに

コーディング規則は、開発チーム内でのコードの読みやすさ、一貫性、コラボレーションを維持するために不可欠です。確立されたガイドラインに従うことで、コードは理解しやすく、保守しやすく、拡張しやすくなります。自分で書いたコードでも、時間が経つと他人のコードと同じようになるため、一貫した規則に従うことが重要です。

このガイドラインは、Microsoft のドキュメントチームがサンプルおよびドキュメント開発のために使用している規則、Qiita で公開されている C# コーディングガイドライン 2024 、および Google の C# スタイルガイドを参考に作成されています。チームのニーズに合わせて規則を変更することも可能です。

### 2\. 全般的な方針

コードは実装を、コメントは意図を、コミットは変更理由を、チケットには現象や問題を記載します。意味のある、わかりやすい、正しい名前付けを心がけ、コードを洗練させます。正しい名前を付けるためには、正しいクラス構造が必要となります。

このガイドラインでは、以下の点を重視します。

- 可能な限り、最新の言語機能と C# バージョンを利用します。これにより、より簡潔で読みやすいコードを記述し、新しい機能の導入を促進します。
- 古い言語コンストラクトは避けます。
- コードの可読性と保守性を高めることを目指します。

### 3\. 命名規則

命名規則は、Microsoft の C# 命名ガイドラインに従います。

- Pascal Case を使用します。
    - クラス、メソッド、列挙体
    - パブリックなフィールドとプロパティ
    - 定数 (Qiita推奨、変更の可能性あり)。Google は const も camelCase に影響しないとしていますが、Qiita では private フィールド以外は Pascal Case を推奨しています。
    - 構造体
    - 名前空間とアセンブリ名
    - ファイル名とディレクトリ名
    - record 型のプライマリコンストラクタのパラメータ
    - コントロール名 (Qiita推奨、変更の可能性あり)
- camel Case を使用します。
    - パラメータ変数
    - ローカル変数
- プライベート フィールドにはアンダースコアを付けます。
    - private string _userId; (Qiita, Google推奨)
    - private static フィールドには s_ を付けます。
        - private static string s_userId; (Qiita, Google推奨)
    - Microsoft のドキュメントチームのスタイルでは private フィールドにアンダースコアを付けない場合があります。チームで一貫したスタイルを選択してください。.editorconfig で設定可能です。
- 接辞:
    - インターフェースは I- を付けます。
    - 機能を表すインターフェースは -able を付けます。
    - 抽象クラスは -Base を付けます。
    - 非同期メソッドは -Async を付けます。
    - 型パラメータは T または T- から始めます。
- メソッド名は動詞句とします。
- 論理値を表す名前には is-, has-, can- を付けます。変数名、プロパティ名、メソッド名に適用されます。
    - 変数名: bool isItemExist, isChanged, canSave
    - プロパティ名: bool IsRefreshing { get; }
    - メソッド名: bool IsNullOrEmpty(...)
- 二重否定は避けます。論理値を返す名前は肯定形を基本とします。
- コレクションやリストの変数名は複数形とします。
- 省略形は避けます。ただし、スコープの短い局所変数では検討できます。
- 静的メンバーは、クラス名 ClassName.StaticMember を使用して呼び出します。こうすることで、静的アクセスが明確になり、コードがよりわかりやすくなります。
- 定義済みの型 (int, string など) を使用します。ランタイム型 (System.Int32, System.String など) の代わりに、データ型の言語キーワードを使用することを推奨します。Qiita ではメンバーアクセス時にCLR型を推奨していますが、Microsoft の公式推奨に従います。
- 名前空間とクラス名の重複は避けます。
- 列挙体のビットフィールドは複数形とします。[Flags] 属性を付けます。

### 4\. レイアウトと書式設定

適切なレイアウトは、コードの構造を強調し、読みやすさを向上させます。

- インデントには 4 つのスペースを使用します。タブは使用しません。.editorconfig で設定できます。
- 1 つの行には 1 つのステートメントのみを記述します。
- 1 つの行には 1 つの宣言のみを記述します。
- 長い行は改行します(ドキュメントでの可読性向上のため、65文字を目安)。継続行にはインデント(4スペース)を設定します。
- 中かっこには "Allman" スタイルを使用します。開きかっこと右角かっこは、独自の新しい行です。中かっこは現在のインデント レベルに合わせて並びます。
- 必要に応じて、中かっこを常に使用します。if ステートメントなどで単一のステートメントの場合でも省略しません。
- メソッド定義とプロパティ定義の間に少なくとも 1 行の空白行を追加します。
- ファイルの末尾に空行を入れることを推奨します。
- 式に句を作成するときはかっこを使用します。
- 範囲を表す比較演算子の向きを揃えます。
- メソッドチェーンを記述する場合はピリオドを前置とします。
- 自動実装プロパティや空のコンストラクタは1行で記述します。
- 配列の列挙時などで最後の要素にもカンマを付けることを推奨します。
- 文字コードや改行コードを指定し、一貫性を保ちます。.editorconfig で設定できます。

### 5\. コードの書き方(プラクティス&イディオム)

#### 新しい言語機能の活用

最新の言語機能を積極的に使用します。

- var は、代入の右側から型が明らかな場合に積極的に使用します。閲覧者が式から型を推論できる場合に使用します。これにより記述が短くなり、可読性が向上する場合があります。メソッド名から型が明らかであると想定しないでください。.editorconfig で設定できます。
- コレクション式 ([]) を使用して、すべてのコレクション型を初期化します。
    - string[] vowels = [ "a", "e", "i", "o", "u" ];
    - 空のコレクションを作成することもできます。
- Func<> と Action<> を使用してデリゲート型を定義します。デリゲート型のインスタンスを作成する場合、簡潔な構文を使用します。
- using ステートメントを使用してコードを簡潔にします。try-finally ステートメントで finally ブロックのコードが Dispose メソッドの呼び出しだけである場合は、using ステートメントを代わりに使用します。中かっこを必要としない新しい using 構文を使用します。
    - using Font normalStyle = new Font("Arial", 10.0f);
- 生文字列リテラル (""" """) を推奨します。エスケープシーケンスや逐語的な文字列よりも推奨されます。複数行のテキストや、エスケープ文字を多く含む文字列(パスや正規表現など)に使用すると、可読性が大幅に向上します。ヒアドキュメントとしても使用できます。
    - var message = """This is a long message...""";
    - var sql = """SELECT u.user_name ...""";
- 式ベースの文字列補間 ($"{}") を使用します。短い文字列を連結するときに推奨されます。可読性が向上します。
- required プロパティを使用して、オブジェクト作成時にプロパティ値の初期化を強制します。コンストラクターの代わりに初期化の義務を表現できます。
- record 型を積極的に活用します。プライマリコンストラクタで宣言することで、読み取り専用プロパティ、Constructor, Deconstruct, Clone, ToString, Equals, GetHashCode などが自動的に実装され、ボイラープレートコードを削減できます。不変なデータコンテナに適しています。with 式による非破壊的な変更が可能です。
    - public record Person(string FirstName, string LastName);
- ファイルスコープ namespace を使用します。コード全体のインデントレベルを減らし、可読性が向上します。
    - namespace MySampleCode;
- Null 許容参照型を有効にすることを検討します。コンパイル時に null チェックが行われるため、実行時エラーの可能性を減らすことができます。Null チェックのガード節が不要となる場合があります。
    - #nullable enable

#### 例外処理

- 適切に処理できる例外のみをキャッチします。一般的な例外 (System.Exception など) を例外フィルターなしでキャッチしないようにします。
- 特定の例外の種類を使用して、意味のあるエラーメッセージを提供します。
- try-catch ステートメントをほとんどの例外処理に使用します。
- フロー制御に例外を使用しません。例外は予期しないエラー状態を示すために使用し、通常のプログラムの分岐には使用しません。ただし、非同期のキャンセル例外はこの限りではありません。
- catch ブロックで例外を再スローする場合、throw; を使用して元のスタックトレースを保持します。原因を呼び出し元にそのまま伝える場合に使用します。詳細が不要な場合や情報漏洩を防ぐ場合は throw ex; を使用することも検討しますが、スタックトレースが上書きされることに注意が必要です。
- NullReferenceException や IndexOutOfRangeException など、コーディングミスに起因する例外はキャッチせず、コードの修正を心がけます。
- すべての例外をキャッチする場合 (catch-all) は、慎重に使用し、可能な限りログに記録します。

#### コレクションの扱い

- コレクション式 ([]) またはコレクション初期化子を使用して、コレクションを初期化します。
- メソッドの引数でコレクションを受けるときは、IEnumerable。これにより、配列やリストなど、様々なコレクション型を受け入れられるようになります。
- ループ中にコンテナから項目を削除する場合は、RemoveAll() メソッドを使用するか、または削除しない項目で新しいコレクションを作成します。
- 公開する変数、プロパティ、戻り値の型には List<> を優先することを推奨します。入力には IReadOnlyCollection / IReadOnlyList / IEnumerable を使用すると、コレクションが不変であることを示せます。
- Dictionary<TKey, TValue> を宣言する際は、キーと値が何を表すかをコメントで残すことを推奨します。

#### 制御フロー

- 早期リターンを積極的に使用します。ガード節など、コードのネストを減らし、可読性を向上させることができます。
- 単純な条件は三項演算子を使うことで可読性が上がります。複雑な条件には使用しません。
- 複雑な分岐は switch 式を使うと可読性が上がります。より複雑な場合は if-else を使用するなど、読みやすさを重視して構文を決定します。パターンマッチングを活用できます。

#### その他

- マジックナンバーは使わず、説明変数や定数を用意します。意図をコメントに残しておくとコードの理解を助けます。
- インクリメント/デクリメント演算子 (++/--) を式の中で使用せず、別の行に記述することを推奨します。考慮する事項が減り、誤読を避けることができます。
- ローカル変数は不必要に使い回さず、正確な名前を付けます。また、スコープは小さく、使う直前で宣言します。ループ内での不必要なオブジェクト生成 (new) は避けます。
- 副作用のないメソッド(純粋関数)と、副作用のあるメソッドの役割を明確にします。Get(), Set() のような軽量で単純な動作が期待される名前に副作用を持たせてはいけません。
- プロパティは、軽量で副作用を持たない場合に使うのが理想です。処理に時間がかかる場合や、呼び出し毎に結果が変わる場合はメソッドにすることを検討します。
- 手続き的な記述よりも、宣言的な記述を優先します。LINQ、オブジェクト初期化子、switch 式など、宣言的な構文を利用すると理解しやすく、バグが減らせる場合があります。
- struct は、小さく、短命で、値型として扱える場合(Vector3, Quaternion など)に検討します。ほとんどの場合は class を使用します。パフォーマンスがシビアな場合は struct を検討できます。readonly struct や in 修飾子、record struct も活用できます。
- デリゲートを呼び出す際は、Delegate?.Invoke() の形式を推奨します。これは呼び出し箇所でデリゲート呼び出しであることが明確になり、スレッドセーフな null チェックが可能です。
- フィールド初期化子を推奨します。
- 拡張メソッドは、元クラスのソースがない場合や変更が困難な場合に限定して使用を検討します。追加する機能は、元クラスのコア機能として適切であるべきです。拡張メソッドはコードを不明瞭にする可能性があるため、慎重に使用します。
- 文字列の連結には、短い場合は文字列補間 ($"{}")、複数回連結する場合は System.Text.StringBuilder を使用します。パフォーマンスが問題にならない場合は、可読性が最も高いものを優先します。
- オブジェクト初期化子 (new SomeClass { ... }) は、'plain old data' 型に適しています。コンストラクタを持つクラスや構造体には避けることを推奨します。

### 6\. パフォーマンスに関する考慮事項

早すぎる最適化は避けるべきです。まずクリーンで読みやすいコードを記述し、計測によってボトルネックを特定してから最適化に取り組みます。

- 常に最新の .NET 実行ランタイムを使用します。ランタイムの進化により、コードを変更せずにパフォーマンスが向上します。
- ユーザーの操作をブロックしないように、async/await を使って UI スレッドをブロックしません。I/O バウンドな操作 (ネットワーク、データベース、ファイルアクセスなど) には非同期プログラミングを使用します。時間のかかる CPU バウンドな処理は Task.Run() を使用することを検討します。
- I/O バウンド操作には、async および await を使用した非同期プログラミングを使用します。
- デッドロックに注意し、必要に応じて Task.ConfigureAwait を使用してください。特に ASP.NET Core API など、コンテキスト切り替えのオーバーヘッドを避けたい場合は .ConfigureAwait(false) を設定することを検討します。
- パフォーマンスが非常にシビアな短い非同期メソッドでは、Task よりも ValueTask の使用を検討し、ヒープ割り当てを減らします。
- 不要なオブジェクト生成(特にループ内での new)や、不要なヒープ割り当て(ボクシング、多量の文字列連結など)を避けるようにコードを記述します。System.Text.StringBuilder や ArrayPool<T>, System.IO.Pipelines など、効率的なメモリ管理ができるクラスの活用を検討します。
- LINQ は可読性が高いですが、遅延評価の特性や、不適切な使用によるパフォーマンス低下に注意が必要です。ToList(), ToArray() による即時評価を適切に使用し、不要な再評価を防ぎます。
- 例外処理やスタックトレースの取得はコストがかかるため、可能な限り例外を回避できる TryParse() や is/as 演算子などを優先して使用します。

### 7\. ツールの活用

ツールは、チームが規則を適用するのに役立ちます。

- コード分析を有効にして、必要なルールを適用します。ルール違反を検出すると警告や診断が生成され、CI ビルドで開発者に通知できます。
- .editorconfig ファイルを作成し、スタイルガイドラインを自動的に適用します。Visual Studio でコードの書式設定や一部の規則の適用を自動化できます。開始点として、既存の .editorconfig をコピーして使用できます。
- Visual Studio のコードクリーンアップ機能などを活用し、コードスタイルを自動的に修正します。
- 必要に応じて、ReSharper や StyleCop.Analyzers などのアナライザーや拡張機能を導入します。

### 8\. コメント規則

コメントは、コードが何をしようとしているかではなく、なぜそれをしようとしているのか、どのようにそれが特定の設計や要件を満たしているのかといった意図を伝えるために使用します。

- 簡単な説明には、1 行のコメント (//) を使います。
- 複数行のコメント (/* */) は避け、行コメント (//) を使います。ブロックコメントはコミットログの Diff を見にくくする可能性があります。
- メソッド、クラス、フィールド、およびすべてのパブリックメンバーを記述するために、XML コメントを使用します。これにより、インテリセンスでドキュメントを表示できます。少なくとも public な部分にはコメントを付けることを推奨します。
- コメントは、コード行の末尾ではなく別の行に記述します。
- コメントのテキストは大文字で開始し、ピリオドで終了します。
- コメント デリミター (//) とコメント テキストの間に空白を 1 つ挿入します。
- コードの内容を繰り返すだけのコメントはノイズになるため書きません。
- 変更履歴のコメントはノイズになるため書きません。変更履歴は git blame などで確認します。
- TODO、HACK、NOTE などのタグ付きコメントを使用して、未完了の作業や一時的な解決策などを明記します。

### 9\. 参考資料

このガイドラインは以下の資料を参考にしています。

- [.NET コーディング規則 - C# | Microsoft Learn](https://learn.microsoft.com/ja-jp/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [C# CODING GUIDELINES 2024 #プログラミング - Qiita](https://qiita.com/Ted-HM/items/1d4ecdc2a252fe745871)
- [C# at Google Style Guide | styleguide](https://google.github.io/styleguide/csharp-style.html)

これらの資料は、C# のコーディングスタイルやプラクティスについて、さらに詳細な情報を提供しています。

このガイドラインは出発点として利用できます。チームの状況やプロジェクトの特性に合わせて、柔軟に調整し、全員が合意した上で適用することが最も重要です。

### 10\. 音声による解説

- [C# コーディング ガイドライン 2025 音声解説](Contents/CSharpCodingGuildline.AudioCommentary.Japanese.mp3)
- [C# コーディング ガイドライン 2025 音声解説 トランスクリプト](Contents/CSharpCodingGuildline.AudioCommentary.Japanese.txt)
