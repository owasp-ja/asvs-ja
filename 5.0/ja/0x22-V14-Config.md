# V14 構成

## 管理目標

箱から出してすぐのアプリケーションの構成はインターネット上で安全であるべきです。つまり、そのままで安全な構成であるということです。

この章ではアプリケーションの開発時に適用する構成とビルドおよびデプロイ時に適用される構成の両方を含む、これを実現するために必要なさまざまな構成に関するガイダンスを提供します。

これには、データ漏洩の防止、さまざまなコンポーネント間の通信の安全な管理、シークレットの保護方法などのトピックを含みます。

## V1.14 構成ドキュメント

このセクションでは、アプリケーションが外部サービスと通信する方法についてと、これらのサービスにアクセスできないことによる可用性の損失を防ぐために採用する必要がある技法についてのドキュメント要件を提供します。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **1.14.1** | [削除, スコープ外] | | v5.0.be-1.14.1 |
| **1.14.2** | [削除, スコープ外] | | v5.0.be-1.14.2 |
| **1.14.3** | [削除, 1.10.5, 10.6.1 でカバー] | | v5.0.be-1.14.3 |
| **1.14.4** | [削除, スコープ外] | | v5.0.be-1.14.4 |
| **1.14.5** | [1.10.4, 10.6.3 へ分割] | | v5.0.be-1.14.5 |
| **1.14.6** | [50.8.2 へ移動] | | v5.0.be-1.14.6 |
| **1.14.7** | [修正, 1.1.5 から移動] アプリケーションのすべての通信が文書化されている。これには、アプリケーションが依存する外部サービスや、アプリケーションが接続する外部ロケーションをエンドユーザが提供できる可能性がある場合が含まれる。 | 2 | v5.0.be-1.14.7 |
| **1.14.8** | [追加] アプリケーションが個別のサービスに接続する場合、アプリケーションのドキュメントには、各サービスへの最大可能並列接続数 (接続プール) と、この最大数に達した際のアプリケーションの応答方法を示し、サービス拒否状態を回避している。 | 3 | v5.0.be-1.14.8 |
| **1.14.9** | [追加] アプリケーションが個別のシステムリソースやサービス (データベース接続、ファイルやスレッドのオープンなど) に接続する場合、それぞれには、接続がタイムアウトする速度や再試行戦略など、文書化されたリソース解放および接続失敗戦略がある。これによりリソース枯渇を防いでいる。同期 HTTP リクエストレスポンスシナリオでの外部接続は、最大可能接続数に達することを避けるために、再試行せずにすぐに失敗する必要がある。 | 3 | v5.0.be-1.14.9 |

## V14.1 ビルドとデプロイ

ビルドプロセスのセキュリティと、関連する DevSecOps の側面は一般的に ASVS のスコープ外ですが、このセクションでは、アプリケーションのコンパイル方法やアプリケーションがデプロイされる際に不要なコンテンツを避けることなど、ビルドおよびデプロイプロセスで適用されるアプリケーション自体のセキュリティコントロールを取り上げます。

このセクションに準拠するには、自動ビルドシステムと、ビルドおよびデプロイスクリプトへのアクセスが必要です。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.1.1** | [削除, スコープ外] | | v5.0.be-14.1.1 |
| **14.1.2** | [レベル L2 > L3] コンパイラフラグが、スタックのランダム化、データ実行防止などの利用可能なすべてのバッファオーバーフローの保護と警告を有効にし、安全でないポインタ、メモリ、フォーマット文字列、整数、または文字列操作が見つかった場合にビルドを中断するように構成されている。 | 3 | v5.0.be-14.1.2 |
| **14.1.3** | [修正] すべてのサードパーティ製品、ライブラリ、フレームワーク、サービスにおいて、それぞれの推奨事項に従って、設定の堅牢化が実行されている。 | 3 | v5.0.be-14.1.3 |
| **14.1.4** | [削除, スコープ外] | | v5.0.be-14.1.4 |
| **14.1.5** | [修正] デプロイされた環境の有効期間が短く、頻繁に再デプロイされて「既知の良好な」更新された状態である。あるいは、有効期間が長い環境ではデプロイされた構成が安全でない状態に変更されないように、何らかの形の「ドリフト防止」を使用する必要がある。 | 3 | v5.0.be-14.1.5 |
| **14.1.6** | [修正, 14.2.2 から移動, 4.3.2 から分割] すべての不要な機能、ドキュメント、サンプルアプリケーション、構成、ファイルやディレクトリのメタデータ (Thumbs.db、.DS_Store など) が削除されている。 | 3 | v5.0.be-14.1.6 |
| **14.1.7** | [追加] 本番環境にテストコードが含まれていない。 | 3 | v5.0.be-14.1.7 |
| **14.1.8** | [追加] ビルドとデプロイメントプロセスに関連するデータ、状態情報、およびサーバーインスタンスはそのプロセスが終了した後には保持しない。 (一過性) | 3 | v5.0.be-14.1.8 |
| **14.1.9** | [追加] アプリケーションコードや機能は標準的な更新プロセスまたはビルドプロセスを通じてのみ変更可能であり、アプリケーション機能やその他の直接変更メカニズムを通じて本番環境で直接変更することはできない。 | 2 | v5.0.be-14.1.9 |
| **14.1.10** | [修正, 2.5.4 から移動] デフォルトユーザーアカウント ("root", "admin", "sa" など) がアプリケーションに存在しないか、無効になっている。 | 1 | v5.0.be-14.1.10 |
| **14.1.11** | [追加, 4.3.2 から分割] アプリケーションは .git や .svn フォルダなどのソース管理メタデータなしでデプロイされるか、またはこれらのフォルダが外部からもアプリケーション自体からもアクセスできない方法でデプロイされる。 | 1 | v5.0.be-14.1.11 |

## V14.2 依存関係

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.2.1** | [1.10.5, 10.6.1 へ分割] | | v5.0.be-14.2.1 |
| **14.2.2** | [14.1.6 へ移動] | | v5.0.be-14.2.2 |
| **14.2.3** | [50.7.1 へ移動] | | v5.0.be-14.2.3 |
| **14.2.4** | [削除, 1.10.2 へ移動] | | v5.0.be-14.2.4 |
| **14.2.5** | [1.10.2 へ移動] | | v5.0.be-14.2.5 |
| **14.2.6** | [1.10.3, 10.6.3 へ分割] | | v5.0.be-14.2.6 |

## V14.3 意図しない情報漏洩

不要なデータの開示を避けるために、本番用の構成を堅牢化する必要があります。これらの問題の多くは重大なリスクとして評価されることはほとんどありませんが、他の脆弱性と連鎖します。これらの問題がデフォルトで存在しない場合、アプリケーションを攻撃するハードルが上がり、攻撃者がより容易なターゲットを探すようになるかもしれません。

たとえば、サーバサイドコンポーネントのバージョンを非表示にしても、すべてのコンポーネントにパッチを適用する必要性がなくなるわけではありませんし、フォルダの一覧表示を無効にしても、認可コントロールを使用したり、パブリックフォルダからファイルを遠ざける必要性がなくなるわけではありませんが、ハードルを引き上げます。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.3.1** | [削除] | | v5.0.be-14.3.1 |
| **14.3.2** | [修正] デバッグ機能の開示や意図しない情報漏洩を防ぐために、運用環境ではすべてのコンポーネントのデバッグモードが無効になっている。 | 2 | v5.0.be-14.3.2 |
| **14.3.3** | [修正] アプリケーションはサーバサイドコンポーネントの詳細なバージョン情報を公開していない。 | 3 | v5.0.be-14.3.3 |
| **14.3.4** | [追加, 4.3.2 から分割] ディレクトリリスティグは、意図して許可されない限り、無効となっている。 | 2 | v5.0.be-14.3.4 |
| **14.3.5** | [文法, 12.5.1 から移動] 意図しない情報やソースコードの漏えいを防ぐために、特定のファイル拡張子を持つファイルのみを処理するように Web 層が構成されている。例えば、バックアップファイル（.bak）、一時作業ファイル（.swp）、圧縮ファイル（.zip、.tar.gz）およびエディタで一般的に使用されるその他の拡張子は、必要ない限り処理しない。 | 3 | v5.0.be-14.3.5 |
| **14.3.6** | [追加, 14.5.1 から分割] HTTP TRACE は潜在的な情報漏洩を避けるために無効化されている。 | 2 | v5.0.be-14.3.6 |

## V14.4 HTTP セキュリティヘッダ

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.4.1** | [13.1.7 へ移動] | | v5.0.be-14.4.1 |
| **14.4.2** | [削除, 50.6.1 へマージ] | | v5.0.be-14.4.2 |
| **14.4.3** | [50.3.1 へ移動] | | v5.0.be-14.4.3 |
| **14.4.4** | [50.3.2 へ移動] | | v5.0.be-14.4.4 |
| **14.4.5** | [50.3.3 へ移動] | | v5.0.be-14.4.5 |
| **14.4.6** | [50.3.4 へ移動] | | v5.0.be-14.4.6 |
| **14.4.7** | [50.3.5 へ移動] | | v5.0.be-14.4.7 |

## V14.5 HTTP リクエストヘッダのバリデーション

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.5.1** | [13.6.1, 14.3.6 へ分割] | | v5.0.be-14.5.1 |
| **14.5.2** | [削除, 4.2.3 でカバー] | | v5.0.be-14.5.2 |
| **14.5.3** | [50.3.6, 50.4.3 へ分割] | | v5.0.be-14.5.3 |
| **14.5.4** | [削除, 不正確] | | v5.0.be-14.5.4 |

## V14.6 Web サーバまたはアプリケーションサーバの構成

## V14.7 バックエンド通信構成

アプリケーションは、API、データベース、その他のコンポーネントを含む複数のサービスとやり取りする必要があります。これらはアプリケーションの内部とみなされるかもしれませんが、アプリケーションの標準アクセス制御メカニズムに含まれていないか、完全に外部かもしれません。いずれの場合も、これらのコンポーネントと安全にやり取りするようにアプリケーションを構成し、必要に応じてその構成を保護する必要があります。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.7.1** | [修正, 2.10.1 から移動, 1.2.2 からマージ] API、ミドルウェア、データレイヤなど、アプリケーションの標準ユーザセッションメカニズムをサポートしていないバックエンドアプリケーションコンポーネント間の通信は認証されている。認証は、パスワード、API キー、特権アクセスを備えた共有アカウントなどの不変のクレデンシャルではなく、個別のサービスアカウント、短期トークン、証明書ベースの認証を使用する必要がある。 | 2 | v5.0.be-14.7.1 |
| **14.7.2** | [文法, 2.10.2 から移動] サービス認証にクレデンシャルを使用する必要がある場合、コンシューマが使用するデフォルトクレデンシャルではない (たとえば、一部のサービスではインストール時に root/root や admin/admin がデフォルトになる)。 | 2 | v5.0.be-14.7.2 |
| **14.7.3** | [修正, 4.3.3 から移動] アプリケーションが外部のデータベースやサービスとの統合のためにパスワードや接続パラメータに関する構成の変更を許可する場合、それらは再認証や複数ユーザ承認などの特別なコントロールによって保護されている。 | 2 | v5.0.be-14.7.3 |
| **14.7.4** | [文法, 12.6.1 から移動] Web サーバまたはアプリケーションサーバはサーバがリクエストを送信したり、データやファイルをロードできるリソースやシステムの許可リストで構成されている。 | 2 | v5.0.be-14.7.4 |
| **14.7.5** | [修正, 1.2.1 から移動] ローカルまたはオペレーティングシステムのサービス、API、ミドルウェア、データレイヤなどのバックエンドアプリケーションコンポーネント間の通信は最小限の権限が割り当てられたアカウントで実行されている。 | 2 | v5.0.be-14.7.5 |
| **14.7.6** | [追加] アプリケーションが個別のサービスに接続する場合、最大並列接続数、最大許容接続数に達した際の動作、接続タイムアウト、再試行戦略など、各接続について文書化された構成に従っている。 | 3 | v5.0.be-14.7.6 |

## V14.8 シークレット管理

シークレット管理は、アプリケーションで使用されるデータの保護を確保するために不可欠な構成タスクです。暗号に関する具体的な要件は V6 章にありますが、このセクションではシークレットの管理と取り扱いの側面に焦点を当てています。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **14.8.1** | [修正, 6.4.1 から移動, 1.6.2, 2.10.4 からマージ, 2.9.1 から分割, 2.10.3, 2.8.2 をカバー] key vault などのシークレット管理ソリューションは、バックエンドシークレットを安全に作成、保管、アクセス制御、破棄するために使用している。これには、パスワード、鍵マテリアル、データベースやサードパーティシステムとの統合、時間ベースのトークンのための鍵やシード、その他の内部セキュリティ、API キーなどを含む。シークレットはアプリケーションのソースコードやビルド成果物に含めてはいけない。L3 アプリケーションでは、これには HSM などのハードウェア支援のソリューションを含む必要がある。 | 2 | v5.0.be-14.8.1 |
| **14.8.2** | [修正, 6.4.2 から移動] 鍵マテリアルはアプリケーション (フロントエンドでもバックエンドでも) には公開されないが、代わりに暗号操作のために vault などの分離されたセキュリティモジュールを使用している。 | 3 | v5.0.be-14.8.2 |
| **14.8.3** | [追加] 重要なシークレットには有効期限が定義されており、組織の脅威モデルとビジネス要件に基づいたスケジュールでローテーションしている。 | 3 | v5.0.be-14.8.3 |
| **14.8.4** | [追加] シークレット資産へのアクセスは最小権限の原則に従っている。 | 2 | v5.0.be-14.8.4 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Web Security Testing Guide 4.1: Testing for HTTP Verb Tampering](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering.html)
* [OWASP Web Security Testing Guide 4.1: Configuration and Deployment Management Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/README.html)
* [Tips to Reduce the Attack Surface When Using Third-Party Libraries](https://www.slideshare.net/KatyAnton1/tips-to-reduce-the-attack-surface-when-using-thirdparty-libraries)
