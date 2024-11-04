## 【coachtechフリマ（メルカリ風フリマアプリ）】  
　～トップ画面イメージ～  
 <img width="700" alt="トップページ" src="https://github.com/user-attachments/assets/abc1df29-e3a2-43db-8d8d-e7dfa1f41212">

## 【制作した目的】  
    『coachtechブランドのアイテムを出品したい』というクライアントの要望に応えるため。  
  
## 【機能一覧】  
#### 　・会員登録機能  
#### 　・ログイン機能  
#### 　・ログアウト機能  
#### 　・商品一覧・詳細表示  
#### 　・商品検索機能  
#### 　・商品お気に入り登録機能  
#### 　・商品コメント機能  
#### 　・商品購入機能  
#### 　・支払方法・配送先住所変更機能  
       支払い方法を「クレジットカード」「コンビニ」「銀行振込」から選択・変更が可能。  
       また、商品の配送先変更も可能。
#### 　・商品出品機能  
       出品時に複数枚の画像をアプロードできる。また、サムネイル画像も選択可能。  
       ※出品時のカテゴリー項目・ブランド項目  
         それぞれテーブルで管理しているが、現時点では最低限のものにしている。  
#### 　・マイページ機能（出品した商品・購入した商品がわかる）  
#### 　・プロフィール変更機能（マイページからユーザーのプロフィールを編集することができる）
       ユーザーアイコン用の画像を複数枚アプロード可能。アプロードした中から１枚選択してアイコン画像として表示。
#### 　・権限設定  
       管理者・一般ユーザーに権限を分けている。  
       またゲストユーザー（権限なし）には商品一覧・詳細表示・検索機能を許可している。  
#### 　・コメント削除機能（管理者）  
       全コメントの一覧表示ができ、条件で抽出が可能。一括削除にも対応。   
#### 　・ユーザー削除機能・メール送信機能（管理者）  
       一般ユーザーを削除する機能を管理者へ付与。  
       またユーザーに対して、個別・一括でのメール送信機能を実装。  
#### 　・決済機能（Stripeを利用）  
       アプリ内で決済機能を設けている。  
       ※Stripeのテスト環境で構築しているため、テスト時のクレジットカードは下記の公式ページのテスト用カードを使用すること。  
         テストカード：https://docs.stripe.com/testing?locale=ja-JP  
  
## 【使用技術（実行環境）】  
#### ・開発環境（Docker環境）  
    PHP		8.1.30  
    Laravel		8.83.27  
    MySQL		8.0.35  
    nginx		1.22.1  
    MailHog（メールサーバー）　  
  
#### ・本番環境（AWS環境）  
    EC2（バックエンド）  
    PHP		8.1.29  
    Laravel		8.83.27  
    nginx		1.22.1  
    RDS（データベース）  
    S3（ストレージ）  
    Gmail（メールサーバーとして使用）   
  
## 【テーブル設計図】  
 ![テーブル設計図完成](https://github.com/user-attachments/assets/65621758-91f1-4e5a-ac36-3731b5372e42)  
  
## 【ER図】  
 ![flea_ERまとめ](https://github.com/user-attachments/assets/812624a0-d45c-4cfc-9d91-adf661b8f7e0)  
  
## 【環境構築】  
### ※開発環境のみ記載※  
### Dockerビルド  
    １　git clone git@github.com:kazuhitotakao/flea-market.git  
    ２　DockerDesktopアプリを立ち上げる  
    ３　docker compose up -d --build  
   
### Laravel環境構築   
    １　docker compose exec php bash  
    ２　composer install  
    ３　「.env.example」ファイルを 「.env」ファイルに命名を変更  
    または、新しく.envファイルを作成  
  
    ４　.envファイルに以下の環境変数を修正・追加  
       APP_NAME=coachtech-flea  
       DB_HOST=mysql  
       DB_DATABASE=laravel_db  
       DB_USERNAME=laravel_user  
       DB_PASSWORD=laravel_pass  
       MAIL_USERNAME=user  
       MAIL_PASSWORD=password  
       MAIL_FROM_ADDRESS=info@example.com  

     ※Stripe決済機能のために追加（ = 以下を各個人で設定）  
      STRIPE_PUBLIC_KEY=  
      STRIPE_SECRET_KEY=  
     （このサイトに登録し、入手する）公式サイト：https://dashboard.stripe.com/  
   
    ５　アプリケーションキーの作成  
      php artisan key:generate  
  
    ６　マイグレーションの実行  
      php artisan migrate:fresh  
  
    ７　シーディングの実行  
      php artisan db:seed  
      ※テストデータが作成される 

    ８　パーミッション設定  
      chmod -R 777 /var/www/storage  

    ９　シンボリックリンク設定  
      php artisan storage:link  
   
## 【URL】  
   　【test用ユーザー】  
　　　`管理者　　　　→　Email：admin@sample.com　　 Password：password`  
　　　`一般ユーザー　→　Email：user1@sample.com　　 Password：password`  
　　　`一般ユーザー　→　Email：user2@sample.com　　 Password：password`  
　　　`一般ユーザー　→　Email：user3@sample.com　　 Password：password`  
   
#### ・開発環境  
    開発環境：http://localhost/  
    phpMyAdmin：http://localhost:8080/	  
    MailHog：http://localhost:8025/  
    
#### ・本番環境
    デプロイ済のURL：http://35.79.89.150/  
    
## 【その他】  
#### ・開発環境と本番環境の切り分けについて  
    .env.development（開発環境） .env.production（本番環境）で切り分けを実施  
    ※個人情報が記載されているため、git.ignoreに記載し、リモートリポジトリへはpushしていない  
    
#### ・開発環境と本番環境での主な相違点  
    開発環境ではDockerで環境構築を行い、メールサーバーをMailHogとしている。  
    本番環境では、AWSで環境構築を行い、メールサーバーをGmailとしている。  
