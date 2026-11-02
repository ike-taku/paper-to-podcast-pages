# 公開文言と実装の照合メモ

確認日：2026-11-02。ローカルの `paperpod.py` を読み取り、以下を照合しました。アプリの実データ、OAuth設定、秘密情報はこのリポジトリへコピーしていません。

確認対象コードのSHA256： `7b39ed38de52db07c86a1b43e50d1f7aa09d7e0f798a906eeac2d81089d01e52`

| 実装箇所 | 確認できた動作 | 公開文言 |
| --- | --- | --- |
| Config / LLM.ask | loopback HTTPのみ許可。本文・根拠・台本をローカルLLMへ送信。外部クラウドAPIへのフォールバックなし | ローカル推論、クラウドLLM/TTSへ送信しない |
| Worker.ingest / rc | rclone lsjsonでInbox直下を一覧、サイズ・更新日時・IDを使い安定確認、copytoでPDFを取得 | PDFとファイルメタデータ取得 |
| Worker.enqueue | PDFコピーをローカルinputsへ保存、SHA256・SQLiteで重複と状態管理 | ローカル保存・処理履歴 |
| extract / evidence / script | ページ本文、根拠、検証済み分割、章台本、失敗時の応答・診断情報を保存 | 本文・派生データ・診断データの保存 |
| KokoroTTS / audio | CPU KPipelineで発話生成、クリップを保存。FFmpegでMP3。結合WAVを変換後に削除 | CPU音声化、クリップは残る。一時削除と全データ削除を区別 |
| process | pages.json、evidence.json、台本JSON/Markdown、要約、指標、COMPLETEを保存 | ページ付き要約、音声・生成物の保存 |
| Worker.tick / publish | ProcessingにPDFコピー。OutputへMP3・要約・台本・指標・元ファイル情報・COMPLETE。Failedへ最終エラー送信を試行 | 原本保持、処理用コピー、生成物、失敗情報 |
| Worker sources / seen | 名前・サイズ・更新日時・IDを含むfingerprintをSQLiteへ保存。source.jsonにDriveファイル名 | 元ファイル名・メタデータの保存 |
| retry --reset-script | 対象ハッシュのローカル生成物を消して再生成。入力・Driveの全コピー削除機能ではない | 一括削除画面なし。手動で両側とログ等を削除 |
| 全体 | 保存期間の自動制御・独自保存時暗号化・モデル学習ループなし | 自動削除や暗号化を保証しない。学習には使用しない |
| .env.example / READMEのDrive手順 | 通常のdriveスコープを想定。フォルダ指定と権限制限は異なる | 広いOAuth権限と通常の処理対象を区別 |

## 誤認を避けるための注記

- 認証情報はrcloneが管理し、実際の保存場所・権限・暗号化の有無はrclone/OSの設定に依存します。このサイトは認証情報を扱いません。
- Google Driveの既存の共有設定をアプリが変更する処理はありません。完全非公開や完全オフラインとは記載していません。
- 運営者の中央収集サーバー、広告、アクセス解析、外部フォント、ブラウザのアップロード・ログイン機能は追加していません。
- Limited Useの文言は、ユーザー向け機能に利用を限定し、販売・広告利用・汎用モデル学習に使用しない運用方針を明示するものです。Googleによる承認や安全性の監査を取得したとの主張ではありません。
- 現在の個人利用構成と照合した文面です。コードの改変や複数ユーザー向け提供の際には再確認が必要です。

## 公開前後の確認

- [ ] ホームとprivacy.htmlが未ログインで閲覧可能
- [ ] GitHub Pagesの公開URLで相互リンク・CSS・SVGが取得可能
- [ ] 連絡先Issuesが利用可能（リポジトリの公開APIでIssues有効を確認）
- [ ] Google Auth Platformのアプリ名・URL・スコープ・連絡先と一致
- [ ] Googleから求められた所有権確認・審査の状態を確認

ローカルでの構造・リンク・機密情報検査と、実公開URLでの検証は別に扱います。最新の実施結果は親プロジェクトのPROGRESS.mdを確認してください。
