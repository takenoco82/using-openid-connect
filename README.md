# OpenID Connectの使い方
## OpenID Connect とは？
- 一般人向け → 認証の仕様
- エンジニア向け → 「ID Token」を発行するための仕様

## 用語解説
- OpenID Provider (OP)
  - ユーザ認証を行うサーバー
  - 認可サーバーを兼ねることが多い
- Relying Party (RP)
  - OpenID Provider のクライアント。Webアプリケーションなど
  - OP に以下を要求する
    - ID Token
    - ユーザ情報 (アイデンティティ情報)
- ID Token
  - 認証と認可の情報を含むJWT (JSON Web Token)
- Access Token
  - ユーザ情報にアクセスするためのトークン

## ID Token
ID Tokenは以下のような仕様で定義された認証情報。
JWTの一種。

### ID Tokenの外観
``` sh
# ID Token にはピリオド (.) が2つ含まれている
# ピリオドで3つの部分に分割できる
#    → ヘッダ.ペイロード.署名
eyJraWQiOiIxZTlnZGs3IiwiYWxnIjoiUlMyNTYifQ.ewogImlz
cyI6ICJodHRwOi8vc2VydmVyLmV4YW1wbGUuY29tIiwKICJzdWIiOiAiMjQ4
Mjg5NzYxMDAxIiwKICJhdWQiOiAiczZCaGRSa3F0MyIsCiAibm9uY2UiOiAi
bi0wUzZfV3pBMk1qIiwKICJleHAiOiAxMzExMjgxOTcwLAogImlhdCI6IDEz
MTEyODA5NzAsCiAibmFtZSI6ICJKYW5lIERvZSIsCiAiZ2l2ZW5fbmFtZSI6
ICJKYW5lIiwKICJmYW1pbHlfbmFtZSI6ICJEb2UiLAogImdlbmRlciI6ICJm
ZW1hbGUiLAogImJpcnRoZGF0ZSI6ICIwMDAwLTEwLTMxIiwKICJlbWFpbCI6
ICJqYW5lZG9lQGV4YW1wbGUuY29tIiwKICJwaWN0dXJlIjogImh0dHA6Ly9l
eGFtcGxlLmNvbS9qYW5lZG9lL21lLmpwZyIKfQ.rHQjEmBqn9Jre0OLykYNn
spA10Qql2rvx4FsD00jwlB0Sym4NzpgvPKsDjn_wMkHxcp6CilPcoKrWHcip
R2iAjzLvDNAReF97zoJqq880ZD1bwY82JDauCXELVR9O6_B0w3K-E7yM2mac
AAgNCUwtik6SjoSUZRcf-O5lygIyLENx882p6MtmwaL1hd6qn5RZOQ0TLrOY
u0532g9Exxcm-ChymrB4xLykpDj3lUivJt63eEGGN6DH5K6o33TcxkIjNrCD
4XB1CKKumZvCedgHHF3IAK4dVEDSUoGlH9z4pP_eWYNXvqQOjGs-rDaQzUHl
6cQQWNiDpWOl_lxXjQEvQ
```

### ID Tokenの項目
ヘッダーとペイロードはデコードするとJSONになっている。主要な項目だけ説明する。
1. ヘッダー
@import "docs/desc_id_token_header.csv"
2. ペイロード
@import "docs/desc_id_token_payload.csv"
3. 署名
   「ヘッダー.ペイロード」に対する署名

## フロー
OpenID Connectのフローは複数定義されている。どのフローを使うかRPが指定できる。(「認可リクエスト」に含まれるresponse_typeで指定する)
「ID Token と Access Token がどのように RP に返却されるか」がフローによって異なる。

### フローの種類
- Authorization Code Flow ← 基本
- Implicit Flow

``` sh
# 認可リクエストのイメージ
HTTP/1.1 302 Found
Location: https://op.example.domain/authorize?
response_type=code              # フローを指定
&scope=openid%20profile%20email # 属性の要求を指定
&client_id=XXXXXX
&state=abcdefg123
&redirect_uri=https%3A%2F%2Frp.example.domain%2Fcallback
```

### エンドポイントの種類
- 認可エンドポイント
  - 認可リクエストをするエンドポイント
  - まず最初にここにリクエストする
- トークンエンドポイント
  - Access Tokenを取得するエンドポイント
    - 認可リクエスト時の指定によっては Access Token だけでなく ID Token やユーザ情報も取得できる
  - 認可リクエストの次にリクエストする

## 参考
- [OpenID Connectの始め方 - Qiita](https://qiita.com/sawadashota/items/d7b794c155d01c319b04)
- [OpenID Connectユースケース、OAuth 2.0の違い・共通点まとめ - Build Insider](https://www.buildinsider.net/enterprise/openid/connect)
- [一番分かりやすい OpenID Connect の説明 - Qiita](https://qiita.com/TakahikoKawasaki/items/498ca08bbfcc341691fe)
- [[前編] IDトークンが分かれば OpenID Connect が分かる - Qiita](https://qiita.com/TakahikoKawasaki/items/8f0e422c7edd2d220e06)
- [OpenID Connect 全フロー解説 - Qiita](https://qiita.com/TakahikoKawasaki/items/4ee9b55db9f7ef352b47)
- [第一回 認証基盤のこれからを支えるOpenID Connect | オブジェクトの広場](https://www.ogis-ri.co.jp/otc/hiroba/technical/openid-connect/chap1.html)
- [GoogleのOpenID Connectorを使ってみた - Qiita](https://qiita.com/toshihirock/items/6679c59ac050f1740db2)
- [OpenID Connect 入門 〜コンシューマーにおけるID連携のトレンド〜](https://www.slideshare.net/kura_lab/openid-connect-id?ref=http://mnakajima18.hatenablog.com/entry/2016/05/04/205713)
