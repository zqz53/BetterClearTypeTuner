# Better ClearType Tuner 日本語 UI 版

[bp2008/BetterClearTypeTuner](https://github.com/bp2008/BetterClearTypeTuner) の非公式日本語版です。
上流の `master`、コミット `7482ead2f5a438272b8370741b43269f1b4110ee`（1.7）を基にしています。
日本語版のアプリバージョンは `1.7.0.1` です。

## 日本語化の範囲

- メイン画面、状態表示、ツールチップ、6 種類の設定ヘルプ。
- 管理者権限・レジストリ操作の警告、DirectWrite のエラー表示。
- メッセージ画面のボタン、コピー・すべて選択メニュー。
- 漢字・ひらがな・カタカナ・英数字を含む共通サンプルを、GDI / DirectWrite ClearType / DirectWrite グレースケールで表示。
- 初期フォントを `Meiryo UI` に変更。「表示フォントを変更」はプレビューのみに作用します。

日本語表示は固定です。OS 標準のフォント選択ダイアログ、UAC、.NET / Windows が生成する例外の詳細は OS の表示言語に従います。
開発用の `BCT_Tests` と上流の調査結果は翻訳していません。

## 起動方法

ビルドした ZIP を展開し、`.exe` と対応する `.exe.config` を同じフォルダーに置いて起動します。

| 実行ファイル | 必要なランタイム | 対象 |
| --- | --- | --- |
| `BetterClearTypeTuner.exe` | .NET Framework 4.7.2 以降 | Windows 7 SP1 以降、x86 / x64 |
| `BetterClearTypeTuner-ARM64.exe` | .NET Framework 4.8.1 | 対応する Windows 10 / 11。Windows on ARM 24H2 以降では ARM64 ネイティブ実行。x86 / x64 でも実行可能 |

日本語を表示できる Windows フォントが必要です。通常は標準の `Meiryo UI` を使用できます。
DirectWrite のサンプル描画は選択したフォントを直接使うため、フォント変更時も日本語の字形を収録したフォントを選んでください。

## 使い方

1. 「文字のアンチエイリアスを有効にする」を選び、グレースケール・RGB・BGR から方式を選びます。
2. GDI のコントラストを調整します。DirectWrite を調整する場合は「DirectWrite の既定値を上書きする」を有効にします。
3. 数値入力欄からフォーカスを移すか「適用」を押します。チェックボックスや選択項目の変更はその場で適用されます。
4. 3 種類の通常表示と 400% 拡大表示を比較します。`[?]` を押すと、その項目の説明を開けます。
5. 他のアプリにも反映するには、そのアプリを再起動します。

「既定値に戻す」は起動前の設定への取り消しではありません。Avalon.Graphics キーを削除し、RGB と GDI コントラスト 1200 に戻します。DirectWrite は独自の既定値を使います。
通常の設定変更に管理者権限は不要ですが、旧版などが残した全ユーザー共通のキーを削除する場合は UAC が表示されることがあります。
設定は接続中のすべてのディスプレイに適用されます。

## ビルド

Windows の Visual Studio 2022 以降（.NET デスクトップ開発）、または対応する .NET SDK と .NET Framework 4.7.2 / 4.8.1 のターゲットパックを使用します。

```powershell
dotnet build BetterClearTypeTuner/BetterClearTypeTuner.csproj -c Release -m:1
```

出力は `BetterClearTypeTuner/bin/Release/` です。
配布用 ZIP は `Releases/BetterClearTypeTuner-ja STAGING.zip` に生成され、両方の実行ファイル・設定ファイル・ライセンス・この説明書を含みます。
上流の `README.md` にある Releases は日本語版ではありません。

`.github/workflows/build.yml` は Windows 上で同じビルドを実行し、ZIP を Actions の成果物として保存します。
フォークで Actions が無効な場合は、Actions タブで有効化してから手動実行できます。

## 検証状況（2026-10-17）

.NET SDK 10.0.402 による Linux からのクロスビルドで、`net472` / `net481` の両方が警告・エラー 0 件で成功しました。使用コマンド:

```sh
dotnet build BetterClearTypeTuner/BetterClearTypeTuner.csproj -c Release -m:1 -p:EnableWindowsTargeting=true -p:BuildInParallel=false -p:UseSharedCompilation=false
```

XML の構文とコード差分も確認済みです。Windows での起動・描画・実設定変更・ARM64 実機動作は未確認です。

## Windows での表示確認

ビルド確認と Windows 上の表示・設定変更の実機確認は別です。初回実行時は次を確認してください。

- 通常表示・125% / 150% の拡大表示で文字が切れないこと。
- 明るい表示・ダークモード、画面の縮小時に各操作ができること。
- 日本語サンプルが 3 方式すべてで表示されること。古い Windows の DirectWrite 制限時は日本語エラーが出ること。
- 6 種類のヘルプ、コピー操作、フォント変更後も操作画面が読めること。

## ライセンスと変更日

上流と同じ GNU GPL v3 です。[LICENSE](LICENSE) と上流の著作権表示を維持しています。
日本語 UI の変更日: 2026-10-17。
設定値の範囲、Windows API / レジストリの操作、権限昇格処理は上流の実装を維持しています。
