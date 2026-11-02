# Paper-to-Podcast 公開説明ページ

Google Auth Platformのホームページ・プライバシーポリシー欄に登録する静的サイトです。アプリ本体とは独立したGitリポジトリです。HTML/CSS/SVGのみで、ビルド・JavaScript・Python依存の追加は不要です。

## ファイル

- `index.html`：アプリ概要、利用目的、処理の流れ、問い合わせ先
- `privacy.html`：データ取得、Drive権限、ローカル処理、保存・削除、第三者提供、Limited Use
- `styles.css` / `favicon.svg`：ローカルの表示用アセット
- `.nojekyll`：Jekyll処理を使わず静的配信
- `DATA_FLOW.md`：公開文言と実装の照合メモ

連絡先は https://github.com/ike-taku/paper-to-podcast-pages/issues です。IssuesにはPDF・個人情報・認証情報を投稿しないでください。

## GitHub Pagesで無料公開する

対象リポジトリ： https://github.com/ike-taku/paper-to-podcast-pages

GitHub Freeで使う場合はPublicリポジトリを使用します。このリポジトリは公開ページ専用です。開発リポジトリの内容を一括コピーしないでください。

1. 変更ファイルを確認し、次の公開用ファイルだけをコミット・pushします。

   ```bash
   cd /path/to/paper-to-podcast-pages
   git status --short
   git add index.html privacy.html styles.css favicon.svg .nojekyll .gitignore README.md DATA_FLOW.md
   git commit -m "Add Paper-to-Podcast app and privacy pages"
   git push -u origin main
   ```

2. GitHubでリポジトリの **Settings → Pages** を開きます。
3. **Build and deployment → Source → Deploy from a branch** を選びます。
4. **Branch: main / Folder: / (root)** を選んで **Save** します。
5. デプロイ完了後、Pages画面の **Visit site** を開きます。HTTPSを使用し、**Enforce HTTPS** が表示される場合は有効にします。
6. 未ログインのブラウザとスマートフォン幅で、ホーム・プライバシー・連絡先リンクを確認します。

公開後の予定URL（実際のデプロイ完了までは公開確認済みではありません）：

- ホーム： https://ike-taku.github.io/paper-to-podcast-pages/
- プライバシー： https://ike-taku.github.io/paper-to-podcast-pages/privacy.html

更新は同じmainブランチへpushします。公開HTMLの履歴も公開されるため、秘密情報を一度でもコミットしないでください。`.gitignore`だけでは誤公開の防止を保証できません。

## Google Auth Platformへの登録

1. アプリ名を `Paper-to-Podcast` とし、サポートメール・デベロッパー連絡先には利用者自身が管理する実在のメールアドレスを登録します。公開ページのIssues連絡先とは別の設定です。
2. **ブランディング → アプリのドメイン** のホームページとプライバシーポリシー欄に、上記の公開確認済みURLを登録します。
3. Googleから所有権確認や承認済みドメインの登録を求められた場合は、実際に管理するサイトの範囲で確認します。`github.io`全体の所有権を主張しないでください。GitHub PagesのURLで要件を満たせないとGoogleから指摘された場合は、所有権を確認できる独自ドメインが必要になる場合があります。この静的サイト自体に有料ホスティングは不要です。
4. 外部アプリの「テスト中」ではDrive等の認可が7日で失効するため、無人運転前に公開ステータスと再認証条件を確認します。
5. 本番環境への変更、ブランディングの確認、機密・制限付きスコープの審査は別の手続きです。ページの公開だけでGoogleの承認・審査完了を保証しません。個人利用には審査の例外が適用される場合があります。

OAuthクライアントの種類はデスクトップアプリです。GitHub PagesのURLは説明用であり、OAuthリダイレクト先へ設定するためのURLではありません。認証はrcloneのローカルコールバックを使います。このページへ認証情報を入力・保存する処理はありません。

## ローカル確認（既存uv環境を利用）

```bash
cd /path/to/paper-to-podcast
uv run --locked python -m http.server 8765 --bind 127.0.0.1 --directory paper-to-podcast-pages
```

ブラウザで http://127.0.0.1:8765/ を開きます。SSHの場合は手元から `ssh -L 8765:127.0.0.1:8765 takuya@接続先` で接続して開きます。終了はCtrl+C。サーバーを外部公開する必要はありません。

## 記載内容を更新する場合

現行アプリの挙動を確認してから更新します。クラウド推論、モデル学習、収集サーバー、共有機能、保存・削除方法などを変更した場合は、変更前に説明と必要な同意を更新します。未実装の暗号化、自動削除、完全匿名化、第三者認証・Google承認を記載しないでください。

## 参照した公式資料

- https://developers.google.com/terms/api-services-user-data-policy
- https://developers.google.com/identity/protocols/oauth2/production-readiness/brand-verification
- https://rclone.org/drive/#making-your-own-client-id
- https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
