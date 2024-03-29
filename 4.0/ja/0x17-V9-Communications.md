# V9 通信

## 管理目標

検証対象のアプリケーションが以下の上位要件を満たすことを確認します。

* コンテンツの機密性に関わらず、TLS または強力な暗号を必要としている。
* 以下のような最新のガイダンスに従っている。
  * 構成に関する勧告
  * 優先アルゴリズムおよび暗号
* 最後の手段を除いて、脆弱または近い将来廃止予定のアルゴリズムや暗号を避けている。
* 非推奨または既知のセキュアでないアルゴリズムや暗号を無効にしている。

これらの要件を満たすために:

* セキュアな TLS 構成に関する業界の推奨事項は、(多くの場合、既存のアルゴリズムや暗号の壊滅的な破綻が原因で) 頻繁に変更されるため、最新の状態に保ちます。
* 最新バージョンの TLS 設定レビューツールを使用して、優先順位とアルゴリズム選択を設定します。
* 設定を定期的にチェックして、セキュアな通信が常に存在し有効であることを確認します。

## V9.1 クライアント通信のセキュリティ

すべてのクライアントメッセージは TLS 1.2 以降を使用して、暗号化されたネットワーク上で送信されるようにします。
最新のツールを使用して、クライアント構成を定期的に見直します。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **9.1.1** | TLS がすべてのクライアント接続に使用されており、安全でない通信や非暗号化通信にフォールバックしない。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 319 |
| **9.1.2** | 最新の TLS テストツールを使用して、強力な暗号スイートのみが有効であり、最も強力な暗号スイートが優先的に設定されている。 | ✓ | ✓ | ✓ | 326 |
| **9.1.3** | TLS 1.2 や TLS 1.3 など、最新の推奨バージョンの TLS プロトコルのみが有効である。最新バージョンの TLS プロトコルを優先オプションにする。 | ✓ | ✓ | ✓ | 326 |

## V9.2 サーバ通信のセキュリティ

サーバ通信は HTTP だけではありません。監視システム、管理ツール、リモートアクセスや SSH、ミドルウェア、データベース、メインフレーム、パートナーまたは外部ソースシステムなど、他のシステムとの安全な接続が必要です。「外部は堅牢だが、内部が傍受しやすい」ことを防ぐために、これらの通信はすべて暗号化する必要があります。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **9.2.1** | サーバ接続（外向き、内向きの両方）において、信頼できる TLS 証明書が使用されている。内部で生成された証明書または自己署名証明書を使用する場合、サーバは特定の内部認証局や特定の自己署名証明書のみを信頼するように構成し、それ以外の証明書はすべて拒否する。 | | ✓ | ✓ | 295 |
| **9.2.2** | 管理ポート、監視、認証、API、または Web サービスの呼び出し、データベース、クラウド、サーバレス、メインフレーム、外部やパートナー間の接続など、すべての外向き／内向きの接続に TLS などの暗号化通信が使用されている。サーバは、安全でないプロトコルや暗号化されていないプロトコルにフォールバックしてはいけない。 | | ✓ | ✓ | 319 |
| **9.2.3** | センシティブな情報や機能を持つ外部システムとの暗号化接続がすべて認証されている。 | | ✓ | ✓ | 287 |
| **9.2.4** | Online Certificate Status Protocol (OCSP) Stapling など証明書の失効を適切にチェックできる機能を設定し、有効にしている。 | | ✓ | ✓ | 299 |
| **9.2.5** | バックエンドの TLS 通信に関するエラーがログに記録されている。 | | | ✓ | 544 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP – TLS Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html)
* [OWASP - Pinning Guide](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning)
* 「TLS の承認されたモード」に関する注記:
    * これまで ASVS は米国標準 FIPS 140-2 を参照してきましたが、この米国標準をグローバル標準として適用することは困難であったり、矛盾が生じたり、あるいは混乱を招く可能性があります。
    * セクション 9.1 に準拠するためのより良い方法は [Mozilla's Server Side TLS](https://wiki.mozilla.org/Security/Server_Side_TLS) などのガイドを参照したり、 [既知の適切な構成をつくり](https://mozilla.github.io/server-side-tls/ssl-config-generator/) 、既存で最新の TLS 評価ツールを使用して望ましいレベルのセキュリティを確保することです。
