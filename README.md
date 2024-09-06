## 【Atte（勤怠管理アプリ）】  
　～トップ画面イメージ～  
 <img width="938" alt="トップ画面" src="https://github.com/kazuhitotakao/attendance/assets/158255815/b40bc9e6-f981-4a56-b015-43732b7f3783">  

## 【制作した目的】  
　社員の勤怠管理をシステムで行うことで、人事評価を適切に実施するため。  
  
## 【機能一覧】  
#### 　・ログイン機能  
#### 　・メールアドレス認証機能（メールによる本人確認）  
#### 　・ログアウト機能  
#### 　・勤務開始および勤務終了機能  
　　　ユーザーにつき1日１度可能。  
　　　日を跨いだ時点で翌日の出勤操作に切り替える。  
　　　※退勤を押さずに日を跨いだ場合、夜勤勤務として考え、23:59:59秒で退勤したことにし、  
　　　　00:00:00出勤のデータを作成する仕様にした。（クライアントであるコーチと相談済）  
#### 　・休憩開始および休憩終了機能  
　　　１日に何度でも休憩可能。  
#### 　・日付別勤怠情報取得  
　　　日付別に出勤操作などを行ったユーザーを確認可能。  
#### 　・ユーザー一覧機能  
　　　名前やメールアドレスでユーザーの抽出が可能。  
#### 　・ユーザー別の勤怠一覧表示機能  
　　　月別で表示可能。  
#### 　・ページネーション（５件ずつ取得）  
  
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
