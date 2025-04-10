# V3 Web フロントエンドセキュリティ

## 管理目標

このカテゴリはアプリケーションの Web フロントエンドを介して実行される攻撃から保護する要件に焦点を当てています。これらの要件はマシン間ソリューションには関係ありません。

## V3.1 サイト分離アーキテクチャ

このセクションではブラウザが提供する同一オリジン分離の利点を活用する方法についてのガイダンスを提供します。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.1.1** | アプリケーションドキュメントには、アプリケーションを使用するブラウザがサポートする必要がある想定されるセキュリティ機能 (HTTPS、HTTP Strict Transport Security (HSTS)、コンテンツセキュリティポリシー (CSP)、その他の関連する HTTP セキュリティメカニズムなど) を記載している。これらの機能の一部が利用できない場合にアプリケーションがどのように動作しなければならないか (ユーザへの警告やアクセスのブロックなど) も定義する必要がある。| 3 | v5.0.be-1.50.1 |

## V3.2 意図しないコンテンツ解釈

コンテンツや機能を不適切なコンテンツでレンダリングすると、さまざまなセキュリティ問題につながる可能性があります。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.2.1** | ブラウザが不正なコンテキスト (API、ユーザがアップロードしたファイル、または他のリソースが直接リクエストされる場合など) で HTTP レスポンスのコンテンツや機能をレンダリングすることを防ぐために、セキュリティ制御が行われている。可能な制御には、HTTP リクエストヘッダフィールド (Sec-Fetch-\* など) が正しいコンテキストであることを示さない限りコンテンツを提供しないこと、Content-Security-Policy ヘッダフィールドの sandbox ディレクティブを使用するか、Content-Disposition ヘッダフィールドの attachment ディポジションタイプを使用することなどがある。 | 1 | v5.0.be-50.6.1 |
| **3.2.2** | HTML としてレンダリングするのではなく、テキストとして表示することを意図したコンテンツは、HTML や JavaScript などのコンテンツの意図しない実行を防ぐために、安全なレンダリング関数 (createTextNode や textContent など) を使用して処理している。 | 1 | v5.0.be-50.6.2 |

## V3.3 クッキーセットアップ

このセクションでは、機密性の高いクッキーが盗まれたり不適切に使用されるリスクを軽減するために、機密性の高いクッキーを安全に構成する方法についての要件を提供します。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.3.1** | クッキーには 'Secure' 属性が設定されており、クッキー名に '\__Host-' プレフィックスが使用されていない場合は、クッキー名に '__Secure-' プレフィックスを使用する必要がある。 | 1 | v5.0.be-50.2.1 |
| **3.3.2** | 各クッキーの 'SameSite' 属性値はクッキーの目的に応じて設定され、一般にクロスサイトリクエストフォージェリ (CSRF) として知られる、ユーザーインタフェースのリドレス攻撃やブラウザベースのリクエストフォージェリ攻撃への露出を制限している。 | 2 | v5.0.be-50.2.3 |
| **3.3.3** | クッキーは、明示的に他のホストと共有するように設計されていない限り、クッキー名に '__Host-' プレフィックスを付けている。 | 2 | v5.0.be-50.2.4 |
| **3.3.4** | クッキーの値がクライアントサイドスクリプトからアクセスされることを意図していない場合 (セッショントークンなど)、クッキーは 'HttpOnly' 属性が設定される必要があり、同じ値 (セッショントークンなど) は 'Set-Cookie' ヘッダフィールドを介してのみクライアントに転送される必要がある。 | 2 | v5.0.be-50.2.2 |
| **3.3.5** | アプリケーションがクッキーを書き込む際、クッキー名と値を合わせた長さは 4096 バイトを超えない。大きすぎるクッキーはブラウザに保存されないため、リクエストとともに送信されず、ユーザはそのクッキーに依存するアプリケーション機能を使用できなくなる。 | 3 | v5.0.be-50.2.5 |

## V3.4 ブラウザのセキュリティメカニズムヘッダ

このセクションでは、機密データを開示する可能性のあるさまざまな種類の攻撃を防ぐために、HTTP レスポンスに設定する必要があるセキュリティヘッダを示します。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.4.1** | HTTP Strict Transport Security (HSTS) ポリシーを適用するために、Strict-Transport-Security ヘッダフィールドがすべてのレスポンスに含まれている。少なくとも 1 年間の最大有効期間を定義する必要があり、L2 以上では、ポリシーをすべてのサブドメインにも適用する必要がある。 | 1 | v5.0.be-50.3.3 |
| **3.4.2** | クロスオリジンリソース共有 (CORS) Access-Control-Allow-Origin ヘッダフィールドが信頼できるオリジンの厳密な許可リストに対して検証されている。 'Access-Control-Allow-Origin: *' を使用する必要がある場合、レスポンスに機密情報が含まれていない。 | 1 | v5.0.be-50.3.6 |
| **3.4.3** | すべての HTTP レスポンスには Content-Security-Policy ヘッダフィールドを含み、悪意のある JavaScript のリスクを軽減している。ディレクティブ object-src 'none' および base-uri 'none' を定義する必要がある。L3 アプリケーションでは、ナンスまたはハッシュを用いたレスポンスごとのポリシーを定義する必要がある。 | 2 | v5.0.be-50.3.1 |
| **3.4.4** | すべてのレスポンスに X-Content-Type-Options: nosniff ヘッダフィールドが含まれている。 | 2 | v5.0.be-50.3.2 |
| **3.4.5** | 適切な Referrer-Policy ヘッダフィールドが含まれており、URL 内の機密情報が Referer ヘッダを介して信頼できないパーティに公開されることを防いでいる。 | 2 | v5.0.be-50.3.4 |
| **3.4.6** | Web アプリケーションはすべての HTTP レスポンスに対して Content-Security-Policy ヘッダフィールドの frame-ancestors ディレクティブを使用して、デフォルトでは埋め込むことができないようにし、特定のリソースの埋め込みは必要な場合にのみ許可されるようにしている。X-Frame-Options ヘッダフィールドは、ブラウザによってサポートされているが、廃止されており、信頼できないことに注意する。 | 2 | v5.0.be-50.3.5 |
| **3.4.7** | Content-Security-Policy ヘッダフィールドが違反を報告する場所を指定している。 | 3 | v5.0.be-50.3.7 |

## V3.5 ブラウザのオリジン分離

サーバサイドで機密機能へのリクエストを受け入れる場合、アプリケーションはそのリクエストがアプリケーション自体あるいは信頼できるパーティによって開始され、攻撃者によって偽造されていないことを確認する必要があります。

このコンテキストにおける機密機能には、認証されたユーザと認証されていないユーザのフォーム投稿の受け入れ (認証リクエストなど)、状態変更操作、リソース要求機能 (データエクスポートなど) があります。

ここでの重要な保護は JavaScript の Same Origin Policy やクッキーの SameSite ロジックなどのブラウザセキュリティポリシーです。もう一つの一般的な保護は CORS プリフライトメカニズムです。このメカニズムは、異なるオリジンから呼び出されるように設計されたエンドポイントにとって重要ですが、異なるオリジンから呼び出されるように設計されていないエンドポイントにとっても、リクエストフォージェリ防止メカニズムとして役立ちます。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.5.1** | 機密機能への CORS セーフリストリクエストをチェックして、アプリケーション自体から発信したものであることを確認している。これはフォージェリ防止トークンを使用して検証するか、CORS セーフリストリクエストヘッダではない追加の HTTP ヘッダを要求することで行われる。これは、一般にクロスサイトリクエストフォージェリ (CSRF) と呼ばれる、ブラウザベースのリクエストフォージェリ攻撃を防御するためのものである。 | 1 | v5.0.be-50.4.1 |
| **3.5.2** | アプリケーションが CORS プリフライトメカニズムに依存して、機密機能の許可されていないクロスオリジン使用を防止している場合、CORS セーフリストリクエストで機能を呼び出すことはできない。これは 'Origin' および 'Content-Type' リクエストヘッダの値をチェックするか、CORS セーフリストではない追加のヘッダフィールドを使用する必要があるかもしれない。 | 1 | v5.0.be-50.4.3 |
| **3.5.3** | 機密機能の呼び出しは POST, PUT, PATCH, DELETE などの適切な HTTP メソッドを使用し、HEAD, OPTIONS, GET などの HTTP 仕様で「安全」と定義されているメソッドを使用しない。あるいは、Sec-Fetch-* リクエストヘッダフィールドの厳密なバリデーションを使用して、不適切なクロスオリジンコール、ナビゲーションリクエスト、期待されていないリソースロード (画像ソースなど) からリクエストが発生していないことを確保している。これはアプリケーションが URL パラメータとメッセージボディパラメータを区別しない場合に特に重要である。 | 1 | v5.0.be-50.4.4 |
| **3.5.4** | あるオリジンでロードされたドキュメントやスクリプトが別のオリジンのリソースとやり取りする方法や、クッキーのホスト名制限など、同一オリジンポリシーによって提供される制限を活用するために、別のアプリケーションは異なるホスト名でホストされている。 | 2 | v5.0.be-50.1.1 |
| **3.5.5** | メッセージのオリジンが信頼できない場合、またはメッセージの構文が無効な場合、postMessage インタフェースによって受信したメッセージを破棄している。 | 2 | v5.0.be-50.4.2 |
| **3.5.6** | クロスサイトスクリプトインクルージョン (XSSI) 攻撃を避けるために、JSONP 機能はアプリケーションのどの部分でも有効になっていない。 | 3 | v5.0.be-50.5.1 |
| **3.5.7** | クロスサイトスクリプトインクルージョン (XSSI) 攻撃を防ぐために、認可が必要なデータは JavaScript ファイルなどのスクリプトリソースレスポンスには含まれていない。 | 3 | v5.0.be-50.5.2 |

## V3.6 外部リソース完全性

このセクションではサードパーティのサイトでコンテンツを安全にホストするためのガイダンスを提供します。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.6.1** | JavaScript ライブラリ、CSS、Web フォントなどのクライアントサイド資産は、リソースが静的かつバージョン管理されており、サブリソース完全性 (Subresource Integrity, SRI) を使用して資産の完全性を検証している場合にのみ、外部 (コンテンツ配信ネットワーク (Content Delivery Network) など) にホストされている。これが不可能な場合は、各リソースについて、この正当性を証明するセキュリティ上の決定事項を文書化する必要がある。 | 3 | v5.0.be-50.7.1 |

## V3.7 ブラウザのセキュリティに関するその他の考慮事項

このセクションではクライアントサイドのブラウザセキュリティに必要となるその他のさまざまなセキュリティコントロールや最新のブラウザセキュリティ機能を含みます。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **3.7.1** | アプリケーションは依然としてサポートされており安全であると考えられているクライアントサイドのテクノロジのみを使用している。この要件を満たさないテクノロジの例としては NSAPI プラグイン、Flash、Shockwave、ActiveX、Silverlight、NACL、クライアントサイド Java アプレットなどがある。 | 2 | v5.0.be-50.8.2 |
| **3.7.2** | アプリケーションは、宛先が許可リストに記載されているもののみ、アプリケーション URL から直接別の URL にユーザーを自動的にリダイレクトしている。 | 2 | v5.0.be-50.8.5 |
| **3.7.3** | アプリケーションのトップレベルドメイン (例: site.tld) が HTTP Strict Transport Security (HSTS) のパブリックプリロードリストに追加されている。これにより、アプリケーションの TLS の使用が、Strict-Transport-Security レスポンスヘッダフィールドのみに依存するのではなく、メインブラウザに直接組み込まれるようになる。 | 3 | v5.0.be-50.8.4 |
| **3.7.4** | ユーザがアプリケーションの制御外の URL にリダイレクトされる場合、アプリケーションはナビゲーションをキャンセルするオプションとともに通知を表示している。 | 3 | v5.0.be-50.8.1 |
| **3.7.5** | アプリケーションへのアクセスに使用されるブラウザが、想定されるセキュリティ機能をサポートしていない場合、アプリケーションはドキュメントに記載されているとおりに動作する (ユーザへの警告やアクセスのブロックなど)。 | 3 | v5.0.be-50.8.3 |

## 参考情報

詳しくは以下の情報を参照してください。

* [Set-Cookie __Host- prefix details](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#cookie_prefixes)
* [OWASP Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
* [Exploiting CORS misconfiguration for BitCoins and Bounties](https://portswigger.net/blog/exploiting-cors-misconfigurations-for-bitcoins-and-bounties)
* [Sandboxing third-party components](https://cheatsheetseries.owasp.org/cheatsheets/Third_Party_Javascript_Management_Cheat_Sheet.html#sandboxing-content)
* [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)
* [OWASP Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
* [HSTS Browser Preload List submission form](https://hstspreload.org/)
