# V12 安全な通信

## 管理目標

この章には、エンドユーザクライアントとバックエンドサービス間、および内部サービスとバックエンドサービス間の両方で、転送時のデータを保護するために導入すべき具体的なメカニズムに関する要件を含みます。

この章で推進される一般的な概念は以下のとおりです。

* 通信が外部で暗号化され、理想的には内部でも暗号化されていることを確保すること。
* 優先アルゴリズムや暗号方式など、最新のガイダンスを使用して暗号メカニズムを構成すること。
* 署名された証明書を使用することで、通信が不正な第三者に傍受されていないことを確保すること。

ASVS では、一般的な原則とベストプラクティスを概説するだけでなく、付録 C - 暗号化標準で暗号強度に関するより詳細な技術情報も提供しています。

## V12.1 一般的な TLS セキュリティガイダンス

このセクションは TLS 通信を保護する方法についての最初のガイダンスを提供します。最新のツールを使用して TLS 構成を継続的にレビューする必要があります。

ワイルドカード TLS 証明書の使用は本質的に安全でないわけではありませんが、所有するすべての環境 (実稼働、ステージング、開発、テストなど) にわたって展開される証明書の侵害は、それを使用するアプリケーションのセキュリティ態勢の侵害につながる可能性があります。適切な保護、管理を実施し、可能であれば、環境ごとに個別の TLS 証明書の使用を採用する必要があります。

| # | 説明 | レベル |
| :---: | :--- | :---: |
| **12.1.1** | TLS 1.2 や TLS 1.3 など、最新の推奨バージョンの TLS プロトコルのみが有効である。最新バージョンの TLS プロトコルを優先オプションにする必要がある。 | 1 |
| **12.1.2** | 推奨暗号スイートのみが有効であり、最も強力な暗号スイートが優先的に設定されている。L3 アプリケーションは前方秘匿性を提供する暗号スイートのみをサポートする必要がある。 | 2 |
| **12.1.3** | アプリケーションは、認証または認可に証明書 ID を使用する前に、mTLS クライアント証明書が信頼できることを検証している。 | 2 |
| **12.1.4** | Online Certificate Status Protocol (OCSP) Stapling などの証明書の失効を適切にチェックできる機能を設定し、有効にしている。 | 3 |
| **12.1.5** | アプリケーションの TLS 設定で Encrypted Client Hello (ECH) を有効にしており、TLS ハンドシェイクプロセス時に Server Name Indication (SNI) などの機密性の高いメタデータの開示を防いでいる。 | 3 |

## V12.2 外部向けサービスとの HTTPS 通信

アプリケーションが公開する外部向けサービスに対するすべての HTTP トラフィックが、公的に信頼できる証明書により暗号化されて送信されることを確認します。

| # | 説明 | レベル |
| :---: | :--- | :---: |
| **12.2.1** | TLS がクライアントと外部向けの HTTP ベースのサービス間のすべての接続に使用されており、安全でない通信や非暗号化通信にフォールバックしない。 | 1 |
| **12.2.2** | 外部向けサービスが公的に信頼できる TLS 証明書を使用している。 | 1 |

## V12.3 一般的なサービス間通信セキュリティ

サーバ通信 (内部および外部の両方) は HTTP だけではありません。他のシステムとの接続も安全である必要があり、理想的には TLS を使用します。

| # | 説明 | レベル |
| :---: | :--- | :---: |
| **12.3.1** | 監視システム、管理ツール、リモートアクセス、SSH、ミドルウェア、データベース、メインフレーム、パートナーシステム、外部 API など、アプリケーションとのすべてのインバウンドおよびアウトバウンドの接続に、TLS などの暗号化プロトコルが使用されている。サーバは安全でないプロトコルや暗号化されていないプロトコルにフォールバックしてはいけない。 | 2 |
| **12.3.2** | TLS クライアントは TLS サーバと通信する前に受信した証明書を検証している。 | 2 |
| **12.3.3** | アプリケーション内の HTTP ベースの内部サービス間のすべての接続に TLS または別の適切なトランスポート暗号化メカニズムが使用されており、安全でない通信や暗号化されていない通信にフォールバックしていない。 | 2 |
| **12.3.4** | 内部サービス間の TLS 接続には信頼できる証明書が使用されている。内部で生成された証明書または自己署名証明書を使用する場合、受け側のサービスは特定の内部認証局や特定の自己署名証明書のみを信頼するように構成する必要がある。 | 2 |
| **12.3.5** | システム内で通信するサービス (サービス内通信) は強力な認証を使用して、各エンドポイントが検証されていることを確保している。TLS クライアント認証などの強力な認証方式を採用し、公開鍵インフラストラクチャとリプレイ攻撃に耐性のあるメカニズムを使用して、アイデンティティを確認する必要がある。マイクロサービスアーキテクチャの場合は、サービスメッシュを使用して証明書管理を簡素化し、セキュリティを強化することを検討する。 | 3 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP - Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
* [Mozilla's Server Side TLS configuration guide](https://wiki.mozilla.org/Security/Server_Side_TLS)
* [Mozilla's tool to generate known good TLS configurations](https://ssl-config.mozilla.org/)
* [O-Saft - OWASP Project to validate TLS configuration](https://owasp.org/www-project-o-saft/)
