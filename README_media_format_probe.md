# media_format_probe.html（Step A / v1.1.0）

動画・画像・音声を選び、「解析できた」「ブラウザがデコードできた」「実際に表示・再生できた」を別々に検査する静的ページ。
サーバー側の処理は不要。選んだファイルはブラウザ内だけで処理し、送信しない。
`timeline/timeline_only.html` など既存ファイルには触れない。

## 必要なファイル（リポジトリに置くもの）
```
media_format_probe.html           ← 本体（必須）
vendor/mediabunny.min.mjs         ← 解析ライブラリ 1.64.0（推奨。無くてもCDNへ自動フォールバック）
vendor/LICENSE-mediabunny.txt     ← MPL-2.0ライセンス全文（vendorを置く場合は一緒に）
.nojekyll                         ← 空ファイル（推奨。Jekyll処理を避ける）
README_media_format_probe.md      ← 任意
```
**テスト用の動画・画像・音声はリポジトリに入れない。**

## GitHub Pagesへの配置
1. リポジトリを用意（Publicか、Pagesが使えるプラン）。上のファイルをブランチ（例: `main`）に置く。
   - ルートに置くか、`probe/` のようなサブフォルダでもよい（参照は全て相対パス）。
2. GitHub → Settings → Pages → Source を「Deploy from a branch」、Branch を `main` / `/ (root)` にして保存。
3. 数分後、次のURLで開く（HTTPS）:
   - ルート配置: `https://<ユーザー名>.github.io/<リポジトリ名>/media_format_probe.html`
   - サブフォルダ配置: `https://<ユーザー名>.github.io/<リポジトリ名>/probe/media_format_probe.html`
4. 更新後に古い版が出る場合は、ページを再読み込み（iPadはタブを閉じて開き直す）。

## 通信について
- ページ表示以外の通信は、解析ライブラリの読み込みのみ（まず `./vendor/…`、無ければ jsDelivr の固定バージョン 1.64.0）。
- 素材ファイル・サムネイル・メタデータタグの値は送信もJSON出力もしない。`localhost` や固定IPへの依存なし。

## 画面で確認できる環境情報
HTTPS / isSecureContext / VideoDecoder・VideoEncoder / AudioDecoder・AudioEncoder / MediaCapabilities /
requestVideoFrameCallback / ImageDecoder / ブラウザ名・版 / OS（推定）/ ライブラリ読込状態。同じ内容がJSONの `environment` に入る（ホスト名は含めない）。

## 動作確認項目（公開後）
1. HTTPSで開き、`HTTPS` と `isSecureContext` が✔になる。
2. ライブラリのチップが✔（環境情報の読込元が `./vendor/…` か CDN か確認）。
3. MP4(H.264)・MOV・JPEG・MP3・WAV・M4A を選び、結果カードが出る。
4. 音声は「音を確認」で再生が進み、停止できる。
5. 「JSON保存」で端末にJSONがダウンロードされる（不可なら「JSONコピー」）。
6. 検査後「使用中Object URL」が0に戻る（JSON保存直後の約10秒を除く）。
7. iPad Safari / iPad Chrome / Windows Chrome・Edge・Firefox でそれぞれ結果JSONを保存し、環境別に比較する。

## ステータス
native-ready / native-limited / conversion-required / unsupported / probe-failed（proxy-readyは予約・未使用）。
音声は試聴前のみ暫定値 `awaiting-user-playback`。MediaCapabilitiesは予測であり、実測と食い違う場合は警告 `[判定]` で明示する。

## ライセンス
Mediabunny 1.64.0（MPL-2.0）。`vendor/` に置く場合はLICENSEも同梱すること。
