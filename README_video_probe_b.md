# Step B ブラウザ検証ページ（v0.3.3）

公開ページ：
https://shiro1357.github.io/video-editor-device-test/video_probe_b.html

このリポジトリの `video_probe_b.html` は、動画編集ソースリポジトリの
`maintenance/cleanup-2026-11-23` ブランチにある
`tools/video_probe_b.html`（Step B v0.3.3）と同じ内容です。

## ファイル構成

- `video_probe_b.html`：Step B v0.3.3 のブラウザ検証ページ
- `vendor/mediabunny.min.mjs`：Mediabunny 1.64.0（動画メタデータ解析用）
- `vendor/LICENSE-mediabunny.txt`：ライセンス

HTMLは `./vendor/mediabunny.min.mjs` を先に読み込みます。
このリポジトリのvendorファイルは、ソースリポジトリ側のvendorファイルと同一のGit blobです。
vendorを更新する必要はありません。

## 検証モード

ページを再読み込みすると、検証モードは必ず「1 情報確認」に戻ります。

| モード | 内容 |
|---|---|
| 1 情報確認 | ①-A メタデータ解析、①-B canPlayType、①-C MediaCapabilities、①-D isConfigSupported。各フェーズを個別に実行 |
| 2 基本動画検証 | 情報確認に加えて、②シーク（最大8回）と④再生（最大5秒）。HEVCは実行対象外 |
| 3 高度診断 | ③連続シーク、⑤負荷シーク、⑥video要素再生成、⑦WebCodecs。各フェーズで追加確認が必要 |

一度に実行するのは「1ファイル・1フェーズ」だけにしてください。
実行後はJSONを保存し、端末・OS・ブラウザ・素材形式と一緒に管理してください。

## 安全上の重要事項

この検証ページの改善は、WindowsのGPUクラッシュ原因が解決したことを意味しません。

Windows PCではLiveKernelEvent 141（AMD GPU関連）が複数回発生しているため、
**原因調査と安全確認が済むまで、再生・シーク・連続シーク・負荷試験・video要素再生成・WebCodecsを実行しないでください。**

情報確認モードも無条件に安全ではありません。①-AはMediabunnyが動画ファイルを読み取り、
最大40,000パケットのメタデータを解析します。情報確認モードであっても、
試験内容と端末の安全性を事前に確認し、許可された場合に限って1ファイル・1フェーズで実行してください。

画面暗転、フリーズ、ブラウザの応答停止、AMDのエラー表示、USB切断音、
通常と異なる長い応答遅延が発生した場合は直ちに中止し、同じ試験を再試行しないでください。

## 結果の読み方

- コンテナ識別、メタデータ解析、ブラウザAPIの対応表示、実デコード、実再生・シークは別の判定段階です。
- canPlayType、MediaCapabilities、VideoDecoder.isConfigSupportedの結果は、実際のデコード・再生成功やハードウェアデコード成功を保証しません。
- HTMLVideoElementのハードウェア／ソフトウェアデコードの区別は、このツールから確定できません。
- 診断結果はブラウザ内のlocalStorageに保存され、JSONとして書き出せます。選択した動画ファイルや映像データをサーバーへ送信する設計ではありません。
- GitHub ActionsやNode.jsの模擬テストの成功は、実機受け入れを意味しません。Step Bの受け入れとStep Cへの移行は、別途実機結果で判断します。

詳細な仕様は動画編集ソースリポジトリの `tools/README_video_probe_b.md` と
`docs/testing/step-b/runtime-acceptance.md` を参照してください。
