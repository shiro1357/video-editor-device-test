# video_probe_b.html（Step B / v0.1.0）
Step A とは別ページ。GitHub Pages では media_format_probe.html と同じ階層に置き、`vendor/mediabunny.min.mjs` を共用する（無ければ jsDelivr 固定版へフォールバック）。
公開URL: `https://<ユーザー名>.github.io/<リポジトリ名>/video_probe_b.html`
手順: 動画を選択 → 検査開始 → JSON保存。選択した素材は送信しない。
- 測定: シーク16回（正確なPTS時刻とフレーム中間時刻を交互）／連続シーク40回（20ms間隔）／ミュート再生（既定5秒）／負荷シーク／video要素の作成・破棄8回／WebCodecs限定デコード（同じ目標時刻）。
- フレーム判定: 元動画の全PTS表から「要求時刻に表示されるべきフレーム」を求め、rVFCのmediaTimeとのフレーム数差で exact / prev-frame / next-frame / keyframe-snap / wrong-frame / off-grid に分類する。
- 再現性: 目標時刻は固定シードで生成（同じ素材なら同じ時刻列）。
## 検証範囲（v0.1.1）
動画素材のみ。画像（JPEG/PNG/HEIC）と独立音声（MP3/WAV/M4A/FLAC/Opus）は対象外で、選ぶと自動でスキップする（Step E / Step D）。動画内蔵音声は存在とメタデータ（codec・ch・Hz）の記録のみで、再生はしない。
