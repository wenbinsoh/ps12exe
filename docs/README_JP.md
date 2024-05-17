# ps12exe

## 導入

ps12exe は、.ps1 スクリプトから実行可能ファイルを作成できる PowerShell モジュールです。  

[![CI](https://github.com/steve02081504/ps12exe/actions/workflows/CI.yml/badge.svg)](https://github.com/steve02081504/ps12exe/actions/workflows/CI.yml)
[![PSGallery ダウンロード数](https://img.shields.io/powershellgallery/dt/ps12exe)](https://www.powershellgallery.com/packages/ps12exe)
[![GitHub issues by-label bug](https://img.shields.io/github/issues/steve02081504/ps12exe/bug?label=bugs)](https://github.com/steve02081504/ps12exe/issues?q=is%3Aissue+is%3Aopen+label%3Abug)
[![コーダシー・バッジ](https://app.codacy.com/project/badge/Grade/ecfd57f5f2eb4ac5bbcbcd525b454f99)](https://app.codacy.com/gh/steve02081504/ps12exe/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)
[![コードファクター](https://www.codefactor.io/repository/github/steve02081504/ps12exe/badge/master)](https://www.codefactor.io/repository/github/steve02081504/ps12exe/overview/master)
[![PR歓迎](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

![repo img](https://repository-images.githubusercontent.com/729678966/3ed3f02f-c7c9-4a18-b1f5-255e667643b6)

[![中文](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/48/China.png)](./docs/README_CN.md)
[![English](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/48/United-Kingdom.png)](./docs/README_EN.md)
[![Español](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/48/Spain.png)](./docs/README_ES.md)
[![हिन्दी](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/48/India.png)](./docs/README_HI.md)

## インストール

```powershell
Install-Module ps12exe ## ps12exeモジュールをインストールする
Set-ps12exeContextMenu ## コンテキストメニューを設定する
```

(リポジトリをクローンして `.\ps12exe.ps1`)

**『PS2EXE から ps12exe へのアップグレードは難しいじゃないか？』そんなことはありません！**  
PS2EXE2ps12exe は PS2EXE の呼び出しを ps12exe にフックできます。PS2EXE をアンインストールしてからこのをインストールし、その後は通常通り PS2EXE を使用すれば問題ありません。

```powershell
Uninstall-Module PS2EXE
Install-Module PS2EXE2ps12exe
```

## 使用方法

### 右クリックメニュー

一度 `Set-ps12exeContextMenu` を設定すれば、任意の ps1 ファイルを右クリックして exe にコンパイルしたり、ps12exeGUI を開いたりすることができます。  
![image](https://github.com/steve02081504/ps12exe/assets/31927825/24e7caf7-2bd8-46aa-8e1d-ee6da44c2dcc)

### GUI モード

```powershell
ps12exeGUI
```

### コンソールモード

```powershell
ps12exe .\ソース.ps1 .\ターゲット.exe
```

`ソース.ps1`を`ターゲット.exe`にコンパイルする（`.\ソース.ps1`を省略した場合は `.\ソース.exe`に書き出されます）。

```powershell
'"Hello World!"' | ps12exe
```

`"Hello World!"`を実行ファイルにコンパイルして `.\a.exe`に出力します。

```powershell
ps12exe https://raw.githubusercontent.com/steve02081504/ps12exe/master/src/GUI/Main.ps1
```

インターネットから `Main.ps1` を実行ファイルにコンパイルして `.\Main.exe`に出力する。

### セルフホスト型ウェブサービス

```powershell
Start-ps12exeWebServer
```

ユーザーがオンラインでpowershellコードをコンパイルできるようにするWebサービスを開始します。

## パラメーター

### GUI パラメータ

```powershell
ps12exeGUI [[-ConfigFile] '<設定ファイル>'] [-Localize '<言語コード>'] [-UIMode 'Dark'|'Light'|'Auto'] [-help]
```

```text
ConfigFile : 読み込む設定ファイル。
Localize   : 使用する言語コード。
UIMode     : 使用するユーザーインターフェースモード。
help       : このヘルプ情報を表示します。
```

### コンソールパラメータ

```powershell
[input |] ps12exe [[-inputFile] '<ファイル名|url>' | -Content '<スクリプト>'] [-outputFile '<ファイル名>']
        [-CompilerOptions '<オプション>'] [-TempDir '<ディレクトリ>'] [-minifyer '<scriptblock>'] [-noConsole]
        [-architecture 'x86'|'x64'] [-threadingModel 'STA'|'MTA'] [-prepareDebug] [-lcid <lcid>]
        [-resourceParams @{iconFile='<ファイル名|url>'; title='<タイトル>'; description='<説明>'; company='<会社>';
        product='<製品>'; copyright='<著作権>'; trademark='<商標>'; version='<バージョン>'}]
        [-UNICODEEncoding] [-credentialGUI] [-configFile] [-noOutput] [-noError] [-noVisualStyles] [-exitOnCancel]
        [-DPIAware] [-winFormsDPIAware] [-requireAdmin] [-supportOS] [-virtualize] [-longPaths]
        [-GuestMode] [-Localize '<言語コード>'] [-help]
```

```text
input            : PowerShellスクリプトファイルの内容の文字列で、-Contentと同じです
inputFile        : 実行可能ファイルに変換したいPowerShellスクリプトファイルのパスまたはURL（ファイルはUTF8またはUTF16でエンコードされている必要があります）
Content          : 実行可能ファイルに変換したいPowerShellスクリプトの内容
outputFile       : ターゲットの実行可能ファイル名またはディレクトリで、デフォルトは'.exe'拡張子を持つinputFileです
CompilerOptions  : 追加のコンパイラオプション（参照： https://msdn.microsoft.com/en-us/library/78f4aasd.aspx）
TempDir          : 一時ファイルを保存するディレクトリ（デフォルトは%temp%にランダムに生成される一時ディレクトリ）
minifyer         : コンパイル前にスクリプトを縮小するスクリプトブロック
lcid             : コンパイルされた実行可能ファイルのロケールID。指定されていない場合は、現在のユーザーのカルチャーです
prepareDebug     : デバッグに役立つ情報を作成します
architecture     : 特定のランタイムのみのコンパイル。可能な値は'x64'、'x86'、'anycpu'です
threadingModel   : 'STA'（シングルスレッドアパートメント）または'MTA'（マルチスレッドアパートメント）モード
noConsole        : 生成された実行可能ファイルは、コンソールウィンドウのないWindows Formsアプリケーションになります
UNICODEEncoding  : コンソールモードで出力をUNICODEでエンコードします
credentialGUI    : コンソールモードでGUIプロンプトを使用して資格情報を求めます
resourceParams   : コンパイルされた実行可能ファイルのリソースパラメータを含むハッシュテーブル
configFile       : 設定ファイル（<outputfile>.exe.config）を書きます
noOutput         : 生成された実行可能ファイルは、標準出力（詳細および情報チャネルを含む）を生成しません
noError          : 生成された実行可能ファイルは、エラー出力（警告およびデバッグチャネルを含む）を生成しません
noVisualStyles   : 生成されたWindows GUIアプリケーションのビジュアルスタイルを無効にします（-noConsoleと共に使用）
exitOnCancel     : Read-Host入力ボックスでCancelまたは\"X\"を選択したときにプログラムを終了します（-noConsoleと共に使用）
DPIAware         : 表示スケーリングが有効になっている場合、GUIコントロールは可能な限りスケーリングされます
winFormsDPIAware : 表示スケーリングが有効になっている場合、WinFormsはDPIスケーリングを使用します（Windows 10および.Net 4.7以上が必要）
requireAdmin     : UACが有効になっている場合、コンパイルされた実行可能ファイルは昇格されたコンテキストでのみ実行可能です（必要に応じてUACダイアログが表示されます）
supportOS        : 最新のWindowsバージョンの機能を使用します（[Environment]::OSVersionを実行して違いを確認）
virtualize       : アプリケーションの仮想化が有効になっています（x86ランタイムを強制）
longPaths        : OSで有効になっている場合、長いパス（260文字以上）を有効にします（Windows 10以上にのみ適用）
GuestMode        : ネイティブ ファイルへのアクセスを防ぐために、スクリプトをコンパイルする際に保護を追加します
Localize         : 使用する言語コード
Help             : このヘルプ情報を表示します
```

## 備考

### 前処理

ps12exe はコンパイル前にスクリプトを前処理します。  

```powershell
# ps12exe.csファイルからプログラムフレームを読み込む
#_if PSEXE # これは ps12exe によってスクリプトがコンパイルされるときに使用される前処理コードです。
	#_include_as_value programFrame "$PSScriptRoot/ps12exe.cs" #ps12exe.csの内容をこのスクリプトに挿入する。
#_else #そうでなければ、csファイルを普通に読み込む
	[string]$programFrame = Get-Content $PSScriptRoot/ps12exe.cs -Raw -Encoding UTF8
#_endif
```

#### `#_if <condition>`/`#_else`/`#_endif`

```powershell
$LocalizeData =
	#PSScript
		. $PSScriptRoot\src\LocaleLoader.ps1
	#_else
		#_include "$PSScriptRoot/src/locale/en-UK.psd1"
	#_endif
```

`PSEXE` と `PSScript` は以下の条件でのみサポートされるようになりました。  
`PSEXE` は真、`PSScript` は偽。  

#### `#_include <ファイル名|url>`/`#_include_as_value <値> <ファイル名|url>`

```powershell
#_include <filename|url>
#_include_as_value <valuename> <file|url>
```

ファイル `<filename|url>` または `<file|url>` の内容をスクリプトに含めます。 ファイルの内容は `#_include`/`#_include_as_value` コマンドで指定した場所に挿入されます。  

前処理コマンドの `#_include` 系列は、`#_if` ステートメントとは異なり、ファイル名を引用符で囲まない場合、末尾のスペース `#` もファイル名の一部として扱います。  

```powershell
#_include $PSScriptRoot/super #変なファイル名.ps1
#_include "$PSScriptRoot/filename.ps1" #安全なコメント！
```

`#_include`を使うと、ファイルの内容が前処理されるので、複数のレベルのファイルをインクルードすることができます。

そのため、複数のレベルのファイルをインクルードすることができます。 `#_include_as_value` を使用すると、ファイルの内容が文字列の値としてスクリプトに挿入されます。 ファイルの内容は前処理されません。  

大体の場合、 `#_if` と `#_include` の前処理コマンドを使わなくても、exe に変換した後のスクリプトにサブスクリプトを正しくインクルードすることができます：

```powershell
. PSScriptRoot/another.ps1
& $PSScriptRoot/another.ps1
$result = & "$PSScriptRoot/another.ps1" -args
```

#### `#_!!`

```powershell
$Script:eshDir =
#_if PSScript #無効な$PSScriptRootで$EshellUIをPSEXEに入れることはできない
if (Test-Path "$($EshellUI.Sources.Path)/path/esh") { $EshellUI.Sources.Path }
elseif (Test-Path $PSScriptRoot/../path/esh) { "$PSScriptRoot/.." }
elseif
#_else
	#_!!if
#_endif
(Test-Path $env:LOCALAPPDATA/esh) { "$env:LOCALAPPDATA/esh" }
```

`#_!!`行の`#_!!`は削除されます。

#### `#_require <モジュールリスト>`

```powershell
#_require ps12exe
#_pragma Console 0
$Number = [bigint]::Parse('0')
$NextNumber = $Number+1
$NextScript = $PSEXEscript.Replace("Parse('$Number')", "Parse('$NextNumber')")
$NextScript | ps12exe -outputFile $PSScriptRoot/$NextNumber.exe *> $null
番号
```

`#_require` スクリプト全体で必要なモジュールを数え、最初の `#_require` の前に以下のコードと等価のスクリプトを追加します：

```powershell
$modules | ForEach-Object{
	if(!(Get-Module $_ -ListAvailable -ea SilentlyContinue)) {
		Install-Module $_ -Scope CurrentUser -Force -ea Stop
	}
}
```

このコードが生成するのはモジュールのインストールだけであり、インポートではないことにご注意ください。
適宜 `Import-Module` を使用してください。

複数のモジュールをrequireする必要がある場合は、複数行のrequire文を書く代わりに、区切り文字としてスペース、カンマ、セミコロンとダンスを使うことができます。

```powershell
#_require module1 module2;module3、module4,module5
```

#### `#_pragma`

pragma プリプロセッシングディレクティブはスクリプトの内容には影響しませんが、コンパイルに使用するパラメータを変更します。  
以下に例を示します：

```powershell
PS C:\Users\steve02081504> '12' | ps12exe
Compiled file written -> 1024 bytes
PS C:\Users\steve02081504> ./a.exe
12
PS C:\Users\steve02081504> '#_pragma Console no
>> 12' | ps12exe
Preprocessed script -> 23 bytes
Compiled file written -> 2560 bytes
```

ご覧のように、コンパイル時に`#_pragma Console no`を指定しなかったとしても、生成されたexeファイルはウィンドウモードで実行されます。
pragmaコマンドは任意のコンパイルパラメータを設定できる：

```powershell
#_pragma noConsole #ウィンドウモード
#_pragma Console #コンソールモード
#_pragma Console no #ウィンドウモード
#_pragma Console true #コンソールモード
#_pragma icon $PSScriptRoot/icon.ico #アイコンの設定
#_pragma title "title" #exeのタイトルを設定する
```

### ミニファイア

ps12exeの "コンパイル "はスクリプト内の全てをそのままリソースとして実行ファイルに埋め込むので、スクリプトに無駄な文字列が多いと、実行ファイルは非常に大きくなります。  
`-Minifyer` パラメータを使うと、コンパイルの前にスクリプトを前処理するスクリプトブロックを指定することができ、生成される実行ファイルを小さくすることができます。  

このようなスクリプトブロックの書き方がわからない場合は、[psminnifyer](https://github.com/steve02081504/psminnifyer) を使ってください。

```powershell
& ./ps12exe.ps1 ./main.ps1 -NoConsole -Minifyer { $_ | & ./psminnifyer.ps1 }.
```

### 未実装コマンドレット一覧

ps12exe の基本的な入出力コマンドは C# で書き換える必要がある。 未実装のものは、コンソールモードでの *`Write-Progress`* (作業が多すぎる) と *`Start-Transcript`*/*`Stop-Transcript`* (Microsoft には適切なリファレンス実装がない) です。

### GUIモードの出力形式

デフォルトでは、powershellの小さなコマンドの出力形式は1行1行です（文字列の配列として）。 コマンドが10行の出力を生成し、GUIを使用して出力される場合、10個のメッセージボックスが表示され、それぞれが決定されるのを待ちます。 これを避けるには、`Out-String`コマンドをコマンドラインにインポートする。 これにより、出力が10行の文字列配列に変換され、そのすべてがメッセー ジボックスに表示される（例：`dir C:\ | Out-String`）。

### 設定ファイル

ps12exe は `generated executable + ".config"` という名前の設定ファイルを作成できます。 ほとんどの場合、これらの設定ファイルは必須ではなく、どの.Net Frameworkのバージョンを使用するかのリストです。 通常は実際の.Net Frameworkを使用するので、設定ファイルなしで実行ファイルを実行してみてください。

### パラメーター処理

コンパイルされたスクリプトは、元のスクリプトと同様にパラメータを処理します。 すべての実行可能ファイルでは、すべてのパラメータはString型であり、パラメータ型が暗黙的に変換されない場合は、スクリプト内で明示的に変換する必要があります。 実行可能ファイルにコンテンツをパイプすることもできますが、同じ制限（パイプされた値はすべてString型）があります。

### パスワードのセキュリティ

コンパイル済みスクリプトには、絶対にパスワードを保存しないでください！  
スクリプト全体は、.net デコンパイラで簡単に見ることができます。  
![image](https://github.com/steve02081504/ps12exe/assets/31927825/92d96e53-ba52-406f-ae8b-538891f42779)

### スクリプトによって環境を区別する  

スクリプトがコンパイルされたexeで実行されているのか、スクリプトで実行されているのかは、`$Host.Name`で判別できます。

```powershell
if ($Host.Name -eq "PSEXE") { Write-Output "ps12exe" } else { Write-Output "他のホスト" }.
```

### スクリプト変数

ps12exe はスクリプトを実行ファイルに変換するので、変数 `$MyInvocation` の値はスクリプトの値とは異なる。

実行ファイルがあるディレクトリのパスを取得するには `$PSScriptRoot` を使用し、実行ファイル自体のパスを取得するには新しい `$PSEXEpath` を使用します。

### -noConsole モードでのバックグラウンドウィンドウ

`-noConsole` モードを使用するスクリプト（`Get-Credential` や `cmd.exe` を必要とするコマンドなど）で外部ウィンドウを開くと、ウィンドウがバックグラウンドで開きます。

これは外部ウィンドウを閉じるときに、ウィンドウズが親ウィンドウをアクティブにしようとするためです。 コンパイルされたスクリプトはウィンドウを持たないため、コンパイルされたスクリプトの親ウィンドウがアクティブになり、通常はエクスプローラやPowershellのウィンドウがアクティブになります。

これを回避するには、`$Host.UI.RawUI.FlushInputBuffer()`を使って、アクティブにできる不可視のウィンドウを開きます。 次に`$Host.UI.RawUI.FlushInputBuffer()`を呼び出すと、このウィンドウは閉じます（以下同様）。

次の例では、`ipconfig | Out-String`を一度だけ呼び出すのとは異なり、バックグラウンドでウィンドウを開かなくなります：

```powershell
$Host.UI.RawUI.FlushInputBuffer()
ipconfig | Out-String
$Host.UI.RawUI.FlushInputBuffer()
```

## 比較の優位性 🏆

### クイック比較 🏁

| 比較項目 | ps12exe | [`MScholtes/PS2EXE@678a892`](https://github.com/MScholtes/PS2EXE/tree/678a89270f4ef4b636b69db46b31e1b4e0a9e1c5) |
| --- | --- | --- |
| 純スクリプトリポジトリ 📦 | ✔️画像と依存関係以外はすべてテキストファイル | ❌オープンソースライセンスのexeファイルを含む |
| "Hello World!"を生成するためのコマンド 🌍 | 😎`'"Hello World!"' \| ps12exe` | 🤔`echo "Hello World!" *> a.ps1; PS2EXE a.ps1; rm a.ps1` |
| 生成された"Hello World"の実行可能ファイルのサイズ 💾 | 🥰1024バイト | 😨25088バイト |
| GUIの多言語サポート 🌐 | ✔️ | ❌ |
| コンパイル時の構文チェック ✔️ | ✔️ | ❌ |
| プリプロセッサ機能 🔄 | ✔️ | ❌ |
| `-extract`などの特殊パラメータ解析 🧹 | 🗑️削除済み | 🥲ソースコードの変更が必要 |
| PRの歓迎度 🤝 | 🥰歓迎！ | 🤷14件のPRのうち13件がクローズされました |

### 詳細な比較 🔍

[`MScholtes/PS2EXE@678a892`](https://github.com/MScholtes/PS2EXE/tree/678a89270f4ef4b636b69db46b31e1b4e0a9e1c5)に比べて、このプロジェクトは以下の改善をもたらしています：

| 改善内容 | 説明 |
| --- | --- |
| ✔️ コンパイル時の構文チェック | コード品質を向上させるためにコンパイル時に構文チェックを実行 |
| 🔄 強力なプリプロセッサ機能 | スクリプトをコンパイル前にプリプロセス処理し、スクリプト全体をコピー＆ペーストする必要なし |
| 🛠️ `-CompilerOptions`パラメータ | 生成された実行可能ファイルをさらにカスタマイズするためのパラメータを追加 |
| 📦️ `-Minifyer`パラメータ | コンパイル前にスクリプトをプリプロセス処理し、より小さな実行可能ファイルを生成 |
| 🌐 URLからスクリプトと含まれるファイルをコンパイルするサポート | アイコンのダウンロードにURLをサポート |
| 🖥️ `-noConsole`パラメータの最適化 | オプション処理とウィンドウタイトル表示を最適化。カスタムのポップアップウィンドウタイトルを設定できます |
| 🧹 exeファイルの削除 | コードリポジトリからexeファイルを削除 |
| 🌍 多言語サポート、純スクリプトGUI | より良い多言語サポート、純スクリプトGUI、ダークモード対応 |
| 📖 csファイルをps1ファイルから分離 | 読みやすく、保守しやすく |
| 🚀より多くの改善点 | and more... |
