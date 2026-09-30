Task Management App



Django REST

Framework、Vue.js、PostgreSQLを使用したシンプルなタスク管理アプリです。



概要



タスクの登録・一覧表示・編集・削除、および完了状態の管理ができます。



フロントエンドとバックエンドを分離し、Vue.jsからDjango REST

FrameworkのREST APIを呼び出す構成にしています。



主な機能



\-   タスク一覧表示

\-   タスク新規登録

\-   タスク編集

\-   タスク削除

\-   完了／未完了の切り替え

\-   編集キャンセル

\-   タイトル必須チェック



使用技術



Backend



\-   Python 3.14

\-   Django 6.1

\-   Django REST Framework 3.18

\-   PostgreSQL 18



Frontend



\-   Vue.js

\-   Vite

\-   JavaScript

\-   HTML / CSS



システム構成



&#x20;   Vue.js

&#x20;     ↓ REST API

&#x20;   Django REST Framework

&#x20;     ↓

&#x20;   Django

&#x20;     ↓

&#x20;   PostgreSQL



API



タスク管理API：



&#x20;   /api/tasks/



主に以下のHTTPメソッドを使用しています。



\-   GET：タスク一覧取得

\-   POST：タスク登録

\-   PATCH：タスク編集・完了状態変更

\-   DELETE：タスク削除



データ項目



Taskモデルでは以下の項目を管理しています。



\-   id

\-   title

\-   description

\-   is\_completed

\-   created\_at

\-   updated\_at



