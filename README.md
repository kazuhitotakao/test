
## 【Rese（飲食店予約アプリ）】  
　～トップ画面イメージ～  
 <img width="886" alt="トップページ" src="https://github.com/user-attachments/assets/83af9e52-9f89-491d-8548-5ff231bd4f5e">

## 【制作した目的】  
　『外部の飲食店予約サービスは手数料を取られるので自社で予約サービスを持ちたい』というクライアントの要望に応えるため。  
  
## 【機能一覧】  
#### 　・ログイン機能  
#### 　・メールアドレス認証機能（メールによる本人確認）  
#### 　・ログアウト機能  
#### 　・飲食店一覧表示　＆　検索機能  
#### 　・飲食店お気に入り登録機能  
#### 　・飲食店予約機能  
#### 　・マイページ機能（予約内容・お気に入り一覧）  
#### 　・権限設定  
　　　管理者・店舗代表者・一般ユーザーに権限を分けている。  
　　　【test用ユーザー】  
　　　管理者　→　Email：admin@sample.com　　Password:password  
　　　店舗代表者　→　Email：owner@sample.com　　Password:password  
　　　管理者　→　Email：user@sample.com　　Password:password  
#### 　・店舗情報作成　＆　予約確認画面（店舗代表者向け）    
　　　店舗画像のアプロード可能  
　　　時間帯ごとの予約枠の上限設定（店側でこのアプリから予約できる枠の指定ができます。）  
　　　予約時の時間間隔の設定（店側で予約の時間間隔を何分毎の間隔にするかの指定ができます。）　 
#### 　・店舗代表者設定管理画面（管理者向け）  
#### 　・メール送信機能（店舗代表者・管理者）  
#### 　・リマインダー機能  
　　　タスクスケジューラーを利用して、予約当日の朝に予約情報のリマインダーを送る。  
#### 　・評価機能  
#### 　・ＱＲコード  
#### 　・決済機能（Stripeを利用）  
　　　※Stripeのテスト環境で構築しています。テスト時のクレジットカードは下記の公式ページのテスト用カードを使用してください。  
　　　　テストカード：https://docs.stripe.com/testing?locale=ja-JP  
#### 　・

  
## 【使用技術（実行環境）】  
#### ・開発環境（Docker環境）  
　　PHP		7.4.9  
　　Laravel	8.83.27  
　　MySQL	8.0.26  
　　nginx	1.21.1  
　　MailHog（メールサーバー）　  
　　cron （自動実行ジョブサーバー）※深夜0時の処理用  
　　※開発環境においては、PCがスリープ中はcronの実行が行われないため、常にPCを起動しておくか、  
　　　タイムスケジューラ等で実行時刻の1分前ほどにスリープ解除するタスクの実装を推奨する。  
  
#### ・本番環境（AWS環境）  
　　EC2（バックエンド）  
　　　PHP		8.2.19  
　　　Laravel		8.83.27  
　　　nginx		1.22.1  
　　RDS（データベース）  
　　S3（ストレージ）  
　　Gmail（メールサーバーとして使用）  
　　cron（Amazon linux2に初期実装されている）  
  
## 【テーブル設計図】  
<img width="497" alt="テーブル設計図_user" src="https://github.com/kazuhitotakao/attendance/assets/158255815/21817345-1f30-4889-9cd2-4b4b67f33009">  
<img width="498" alt="テーブル設計図_attendances" src="https://github.com/kazuhitotakao/attendance/assets/158255815/617c30e3-17a0-4d02-bdf7-b168331bf500">  
  
## 【ER図】  
<img width="443" alt="Atte_ER" src="https://github.com/kazuhitotakao/attendance/assets/158255815/05fa8f48-238e-4650-a240-f4f9adbbe589">  
  
## 【環境構築】  
### ※開発環境のみ記載※  
### Dockerビルド  
　１　git clone git@github.com:kazuhitotakao/attendance.git  
　２　DockerDesktopアプリを立ち上げる  
　３　docker compose up -d --build  
   
### Laravel環境構築  
　１　docker compose exec php bash  
　２　composer install  
　３　「.env.example」ファイルを 「.env」ファイルに命名を変更  
 　　　または、新しく.envファイルを作成  
  
　４　.envファイルに以下の環境変数を修正・追加  
　　　APP_NAME=Atte  
　　　DB_HOST=mysql  
　　　DB_DATABASE=laravel_db  
　　　DB_USERNAME=laravel_user  
　　　DB_PASSWORD=laravel_pass  
　　　MAIL_USERNAME=user  
　　　MAIL_PASSWORD=password  
　　　MAIL_FROM_ADDRESS=info@example.com
  
　５　アプリケーションキーの作成  
　　　php artisan key:generate  
  
　６　マイグレーションの実行  
　　　php artisan migrate:fresh  
  
　７　シーディングの実行  
　　　php artisan db:seed  
　　　※テストデータが作成されます。  
  
## 【URL】  
#### ・開発環境  
　　開発環境：http://localhost/  
　　phpMyAdmin：http://localhost:8080/	  
　　MailHog：http://localhost:8025/  
#### ・本番環境
　　デプロイ済のURL：http://43.207.55.116/  
　　※開発環境・本番環境ともに、ログイン前に、メールアドレス登録を行い、登録したメールアドレスでの認証が必要。  

## 【その他】  
#### ・開発環境と本番環境の切り分けについて  
　　.env.development（開発環境） .env.production（本番環境）で切り分けを実施  
　　※個人情報が記載されているため、git.ignoreに記載し、リモートリポジトリへはpushしていない  
  
#### ・開発環境と本番環境での主な相違点  
　　開発環境ではDockerで環境構築を行っている。  
　　メールアドレスの認証にMailHogを使用しており、MailHog上でメール認証を行うことが必要。  
　　本番環境では、AWSで環境構築を行っている。  
　　メールアドレス認証にGmailサーバーを使用しており、実際に各人のメールアドレスでメール認証を行うこと。  
