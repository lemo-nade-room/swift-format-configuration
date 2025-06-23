# Swift Format 設定ファイル

このリポジトリにはカスタマイズされた`.swift-format`設定ファイルが含まれています。
Server-Side Swiftの開発を想定し、コードの厳格性と可読性を重視した設定になっています。

## 主な特徴

### 🌏 日本語対応
- **IdentifiersMustBeASCII: false** - 日本語を含む非ASCII文字の識別子（変数名、関数名、クラス名など）を使用可能

```swift
// 例：日本語の変数名や関数名が使用可能
let 税込価格 = 商品価格 * 1.1
func 消費税を計算する(価格: Double) -> Double { }
```

### 📐 改行とフォーマット

#### 引数での折り返し
- **lineBreakBeforeEachArgument: true** - 各引数の前で改行
- **lineBreakBeforeEachGenericRequirement: true** - ジェネリック要件ごとに改行

```swift
// 複数の引数は垂直に配置される
func データを取得する(
  from url: URL,
  timeout: TimeInterval,
  retryCount: Int = 3
) async throws -> Data {
  // 実装
}
```

#### 関数シグネチャの保持
- **prioritizeKeepingFunctionOutputTogether: true** - `async throws -> 戻り値` 部分を一緒に保持

```swift
// async throws -> は閉じ括弧と同じ行に保持される
func 非同期処理(
  パラメータ1: String,
  パラメータ2: Int
) async throws -> 結果型 {
  // 実装
}
```

#### ドットチェーンの改行
- **lineBreakAroundMultilineExpressionChainComponents: true** - ドットの前で改行

```swift
let 結果 = データ配列
  .map { 要素 in
    要素を処理(要素)
  }
  .filter { $0.有効性をチェック() }
  .sorted { $0.優先度 > $1.優先度 }
```

### ⚠️ エラーハンドリング
- **NeverForceUnwrap: false** - 強制アンラップ（!）を許可
- **NeverUseForceTry: true** - 強制的な`try!`は禁止
- **NeverUseImplicitlyUnwrappedOptionals: true** - 暗黙的アンラップ型は禁止

### 📚 ドキュメント化
- **AllPublicDeclarationsHaveDocumentation: true** - すべてのpublic宣言にドキュメントコメントを要求
- **BeginDocumentationCommentWithOneLineSummary: true** - ドキュメントは1行の要約から開始
- **UseTripleSlashForDocumentationComments: true** - ドキュメントコメントには`///`を使用
- **ValidateDocumentationComments: true** - ドキュメントコメントの検証を有効化

### 🔧 コード簡潔性
- **OmitExplicitReturns: true** - 単一式の関数・クロージャ・計算プロパティでreturnを省略

```swift
// returnが省略される例
var 税込価格: Double {
  商品価格 * 1.1  // returnキーワード不要
}

func 二倍にする(_ 数値: Int) -> Int {
  数値 * 2  // returnキーワード不要
}

// クロージャでも同様
let 偶数のみ = 数値配列.filter { $0 % 2 == 0 }
```

- **multiElementCollectionTrailingCommas: true** - 複数要素のコレクションで末尾カンマを強制

```swift
// 配列や辞書で末尾カンマが必須
let 設定 = [
  "ホスト": "localhost",
  "ポート": 8080,
  "デバッグ": true,  // ← 末尾カンマ必須
]

let 引数リスト = [
  第一引数,
  第二引数,
  第三引数,  // ← 末尾カンマ必須
]
```

### 🎨 コードスタイル

#### インデント
- **spaces: 2** - インデントは2スペース
- **indentSwitchCaseLabels: true** - switch文のcaseラベルをインデント
- **indentConditionalCompilationBlocks: true** - 条件付きコンパイルブロックをインデント

#### 行の長さ
- **lineLength: 120** - 最大行長は120文字

#### その他の厳格なルール
- **UseEarlyExits: true** - 早期リターンを推奨
- **UseWhereClausesInForLoops: true** - forループでwhere句の使用を推奨
- **OmitExplicitReturns: true** - 単一式の場合はreturnを省略
- **multiElementCollectionTrailingCommas: true** - 複数要素のコレクションでケツカンマを強制
- **NoEmptyLinesOpeningClosingBraces: true** - 開き/閉じブレースの前後の空行を禁止
- **NoLeadingUnderscores: true** - 先頭のアンダースコアを禁止
- **OrderedImports: true** - import文を自動的に整理

## 使用方法

### インストール
```bash
# swift-formatのインストール（Homebrewを使用）
brew install swift-format

# または、Swift Package Managerを使用
git clone https://github.com/apple/swift-format.git
cd swift-format
swift build -c release
```

### フォーマットの実行
```bash
# ファイルをフォーマット
swift-format format -i ファイル名.swift

# ディレクトリ全体をフォーマット
swift-format format -i -r ソースディレクトリ/

# フォーマットの確認（変更なし）
swift-format lint ファイル名.swift
```

### Xcodeとの統合
Build Phasesに以下のスクリプトを追加することで、ビルド時に自動フォーマットが可能です：

```bash
if which swift-format >/dev/null; then
  swift-format format -i -r "${SRCROOT}/Sources"
else
  echo "warning: swift-format not installed"
fi
```

## 設定の詳細

### フォーマット設定

| 設定項目 | 値 | 説明 |
|---------|-----|------|
| `fileScopedDeclarationPrivacy.accessLevel` | `fileprivate` | ファイルスコープ宣言のデフォルトアクセスレベル |
| `lineLength` | `120` | 1行の最大文字数 |
| `maximumBlankLines` | `1` | 連続する空行の最大数 |
| `multiElementCollectionTrailingCommas` | `true` | 複数要素のコレクションで末尾カンマを使用 |
| `spacesAroundRangeFormationOperators` | `true` | 範囲演算子の周りにスペースを配置 |
| `spacesBeforeEndOfLineComments` | `2` | 行末コメント前のスペース数 |

### 有効化されているルール（主要なもの）

| ルール | 説明 |
|--------|------|
| `AllPublicDeclarationsHaveDocumentation` | public宣言にはドキュメントが必須 |
| `AlwaysUseLiteralForEmptyCollectionInit` | 空のコレクションはリテラル（`[]`, `[:]`）で初期化 |
| `AvoidRetroactiveConformances` | 遡及的な適合を避ける |
| `GroupNumericLiterals` | 大きな数値リテラルをグループ化（例：`1_000_000`） |
| `OmitExplicitReturns` | 単一式のreturnを省略 |
| `UseEarlyExits` | 早期リターンパターンを使用 |
| `UseWhereClausesInForLoops` | forループでwhere句を活用 |

### 無効化されているルール

| ルール | 説明 |
|--------|------|
| `IdentifiersMustBeASCII` | 日本語識別子を許可するため無効化 |
| `NeverForceUnwrap` | 強制アンラップを許可するため無効化 |

## カスタマイズ

設定を変更する場合は、`.swift-format`ファイルを編集してください。
変更後は以下のコマンドで設定の妥当性を確認できます：

```bash
swift-format dump-configuration > 現在の設定.json
```

## 注意事項

- この設定は厳格なルールを適用するため、既存のコードベースに適用する際は大量の変更が発生する可能性があります
- チーム開発では、メンバー全員が同じ設定を使用することを確認してください
- CI/CDパイプラインでの自動チェックを推奨します