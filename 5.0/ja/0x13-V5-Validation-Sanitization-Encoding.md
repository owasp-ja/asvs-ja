# V5 バリデーション、サニタイゼーション、エンコーディング

## 管理目標

最も一般的な Web アプリケーションのセキュリティ上の弱点は、出力エンコーディング、クエリのパラメータ化、あるいはその他の出力処理防御を行わずに、安全でないコンテキストで信頼できないコンテンツを使用することです。この弱点は、クロスサイトスクリプティング (XSS)、SQL インジェクション、OS コマンドインジェクション、テンプレートインジェクション、ログインジェクション、LDAP インジェクションなど、Web アプリケーションの重要な脆弱性のほとんどすべてにつながっています。

検証対象のアプリケーションが以下の上位要件を満たすことを確認します。

* 入力のバリデーションと出力エンコーディングのアーキテクチャは、インジェクション攻撃を防ぐために合意されたパイプラインを持っています
* 入力データは強力に型付けされ、バリデーションされ、範囲や長さがチェックされ、最悪の場合、サニタイズされ、フィルタリングされていること
* 出力データは、データのコンテキストに応じて、可能な限りインタプリタに近い形でエンコード、エスケープされること

Wモダンな Web アプリケーションのアーキテクチャにおいては、出力エンコーディングはこれまで以上に重要です。特定のシナリオでは、堅牢な入力バリデーションを行うことは難しく、パラメータ化されたクエリ、テンプレートフレームワークによる自動エスケープ、または慎重に選択された出力エンコーディングのようなより安全な API を使用することは、アプリケーションのセキュリティにとって非常に重要です。

## V5.1 入力バリデーション

入力は HTML フォームフィールド、REST リクエスト、URL パラメータ、HTTP ヘッダ、Cookie、ディスク上のファイル、データベース、外部 API など、さまざまなソースからもたらされます。

ポジティブ許可リストと強いデータ型付けを使用して、入力バリデーション制御を適切に実装すると、アプリが受け取ることを期待するデータの種類に関するビジネスロジック制御の重要な実施を提供します。しかし、特定の場合を除き、一般的に特定の攻撃を防ぐことを意図していません。

入力バリデーションは依然として価値のあるセキュリティ衛生を提供するため、可能な限りすべての入力に適用すべきです。しかし、入力バリデーションは完全なセキュリティ戦略でははないため、潜在的に危険なコンテキストで入力が使用される際には常に、サンドボックス化、サニタイゼーション、エンコーディング、パラメータ化も利用すべきです。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.1.1** | アプリケーションが HTTP 変数汚染攻撃に対する防御策を備えている。特にアプリケーションフレームワークが、リクエストパラメータのソース (クエリ文字列、ボディパラメータ、Cookie、ヘッダ) を区別しない場合はこの防御が必要。 | ✓ | ✓ | ✓ | 235 |
| **5.1.2** | フレームワークが大量のパラメータ割り当て攻撃から保護している、またはフィールドを非公開にするなど安全でないパラメータの代入を防ぐための対策が行われている。 | ✓ | ✓ | ✓ | 915 |
| **5.1.3** | [修正] すべての入力は、値やパターンの許可リストを使用した、ポジティブバリデーションを使用して検証されている。 | ✓ | ✓ | ✓ | 20 |
| **5.1.4** | [文法] 構造化データが強く型付けされており、使用可能な文字、長さ、パターン等の定義されたスキーマに基づいてバリデーションされる。（例：クレジットカード番号、電子メールアドレス、電話番号などのバリデーションや、地区名と郵便番号等 2 つの関連するフィールドのデータが妥当なことのバリデーション） | ✓ | ✓ | ✓ | 20 |
| **5.1.5** | [修正, 50.7.1 に分割] アプリケーションは、宛先が許可リストに記載されているもののみ、アプリケーション URL から直接別の URL にユーザーを自動的にリダイレクトしている。 | ✓ | ✓ | ✓ | 601 |
| **5.1.6** | [追加] 信頼できない入力はクッキー (JWT の一部として含める) に含める前に長さが検証され、クッキー名と値の長さを合わせて 4096 バイトを超えていない。 | | ✓ | ✓ | |

## V5.2 サニタイゼーションとサンドボックス化

入力バリデーションは複雑なトピックです。

入力バリデーションがセキュリティに役立たないこともあれば、中程度に役立つこともあり、セキュリティ防御の要となることもあります。入力バリデーションがどれほど有効であるかは、データの種類とそのデータの使用方法に依存します。

例:

* サニタイゼーション: ユーザが HTML をオーサリングする場合、標準的な防御策は HTML を標準化して削除することです。 JSON パーサーを使用する前に JSON サニタイズを実行します。もちろん XSS 防御のために HTML サニタイゼーションも行います。
* エスケープ化: ユーザが入力したとおりにコンテンツを表示したい場合に UI で行われます。また LDAP インジェクション保護などのインジェクション保護のためにも行われます。
* パラメータ化: 主に SQL インジェクションに対して行われます。
* サンドボックス化: 何らかの理由で HTML をサニタイズできず、潜在的にアクティブなコンテンツを Web ページにダンプする必要がある場合、 iFrame サンドボックス化は非常に重要です。 CSP にもサンドボックス化の機能があります。
* Web UI の URL は XSS 攻撃に対する防御として JavaScript やデータ URL をブロックする必要があります。しかし多くの場合には有効なデータであっても依然として脅威となる可能性があることに注意することが重要です。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.2.1** | [修正] WYSIWYG エディタ等から取得した信頼できない HTML 入力が、よく知られた安全な HTML サニタイゼーションライブラリもしくはフレームワークの機能を使用して適切にサニタイズされている。 | ✓ | ✓ | ✓ | 116 |
| **5.2.2** | 非構造化データがサニタイズされ、使用可能な文字や長さなど一般的な対策が適用されている。 | ✓ | ✓ | ✓ | 138 |
| **5.2.3** | SMTP または IMAP インジェクションから保護するため、アプリケーションがメールシステムに渡す前にユーザ入力をサニタイズする。 | ✓ | ✓ | ✓ | 147 |
| **5.2.4** | eval()もしくは動的コード実行機能を使用しない。代替方法がない場合は、実行前に含まれるユーザ入力のサニタイズもしくはサンドボックス化する。 | ✓ | ✓ | ✓ | 95 |
| **5.2.5** | [修正] アプリケーションは信頼できない入力に基づくテンプレートの作成を許可しないことで、テンプレートインジェクション攻撃から保護している。代替手段がない場合、テンプレート作成中に動的に含まれるすべての信頼できない入力はサニタイズまたは厳密にバリデーションする必要がある。 | ✓ | ✓ | ✓ | 94 |
| **5.2.6** | 信頼できないデータやファイル名や URL 入力フィールドなど HTTP ファイルメタデータのバリデーションまたはサニタイズや、プロトコル、ドメイン、パス、ポートに許可リストを使用することで、アプリケーションが SSRF 攻撃から保護されている。 | ✓ | ✓ | ✓ | 918 |
| **5.2.7** | ユーザの指定した Scalable Vector Graphics (SVG) スクリプト化可能コンテンツ（特に XSS に関連するインラインスクリプトや foreignObject）に対して、アプリケーションがサニタイズまたはサンドボックス化している。 | ✓ | ✓ | ✓ | 159 |
| **5.2.8** | ユーザ指定の Markdown、CSS、XSL スタイルシート、BBCode のようなスクリプト可能もしくは式のテンプレート言語コンテンツに対して、アプリケーションがサニタイズ、無効化またはサンドボックス化されている。 | ✓ | ✓ | ✓ | 94 |
| **5.2.9** | [追加] アプリケーションはスラッシュを使用して、正規表現で使用される特殊文字を正確にエスケープし、制御文字として誤って解釈されないようにしている。 | ✓ | ✓ | ✓ | 624 |
| **5.2.10** | [追加] 正規表現に指数関数的なバックトラッキングを引き起こす要素がないことを検証している。また、信頼できない入力をサニタイズし、ReDoS 攻撃や Runaway Regex 攻撃を軽減している。 | ✓ | ✓ | ✓ | 1333 |
| **5.2.11** | [追加] アプリケーションは Java Naming and Directory Interface (JNDI) クエリで使用する前に信頼できない入力を適切にサニタイズし、JNDI を可能な限り安全に構成して JNDI インジェクション攻撃を防いでいる。 | ✓ | ✓ | ✓ | 917 |
| **5.2.12** | [追加] アプリケーションは memcache に送信する前にコンテンツをサニタイズし、インジェクション攻撃を防いでいる。 | | ✓ | ✓ | |
| **5.2.13** | [修正, 5.4.2 から移動] 使用時に、予期しないあるいは悪意のある方法で解決される可能性のあるフォーマット文字列は、処理される前にサニタイズされている。 | | ✓ | ✓ | 134 |

## V5.3 出力エンコーディングとインジェクション防御

使用しているインタプリタの近くまたは隣接している出力エンコーディングは、アプリケーションのセキュリティにとって非常に重要です。通常、出力エンコーディングは永続化されず、適切な出力コンテキストで出力を安全にレンダリングして、すぐに使用されます。出力エンコードに失敗すると、安全ではなくインジェクション可能で危険なアプリケーションになります。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.3.1** | [修正] HTTP レスポンス/HTML ドキュメント/XML ドキュメントの出力エンコーディングは、メッセージやドキュメント構造の変更を避けるために、HTML 要素、HTML 属性、HTML コメント、JavaScript、CSS、HTTP ヘッダに関連する文字をエンコードするなど、要求されるコンテキストに関連している。 | ✓ | ✓ | ✓ | 116 |
| **5.3.2** | [削除, 14.4.1 と重複] | | | | |
| **5.3.3** | コンテキストに応じて (できれば自動化された、あるいは最悪でも手動による) 出力エスケープにより反射型、格納型、および DOM ベース XSS から保護されている。 | ✓ | ✓ | ✓ | 79 |
| **5.3.4** | [修正] データ選択またはデータベースクエリ（例、SQL、HQL、NoSQL、Cypher）がクエリのパラメータ化、ORM、エンティティフレームワークもしくは他の方法により保護されており、SQL インジェクションや他のデータベースインジェクション攻撃の影響を受けない。これはストアドプロシージャを記述する際にも考慮している。 | ✓ | ✓ | ✓ | 89 |
| **5.3.5** | [削除, 5.3.4 と重複] | | | | |
| **5.3.6** | [修正] アプリケーションが JSON インジェクション攻撃から保護されている。 | ✓ | ✓ | ✓ | 75 |
| **5.3.7** | アプリケーションが LDAP インジェクションの影響を受けない、またはセキュリティ管理策によって LDAP インジェクションが防止される。 | ✓ | ✓ | ✓ | 90 |
| **5.3.8** | OS コマンドインジェクションに対して保護していること、およびオペレーティングシステムコールがパラメータ化された OS クエリを使用、もしくはコンテキストコマンドライン出力エンコーディングを使用する。 | ✓ | ✓ | ✓ | 78 |
| **5.3.9** | [削除, 12.3.2, 12.3.3 と重複] | | | | |
| **5.3.10** | アプリケーションが XPath インジェクション攻撃や XML インジェクション攻撃から保護されている。 | ✓ | ✓ | ✓ | 643 |
| **5.3.11** | [追加] アプリケーションが CSV インジェクションや数式インジェクションから保護されている。アプリケーションは CSV ファイルをエクスポートする際に RFC4180 2.6 および 2.7 で定義されているエスケープ規則に従う必要がある。アプリケーションは CSV ファイルや xls, xlsx, odf などのその他のスプレッドシート形式をエクスポートする際に、フィールドの最初の文字が '=', '+', '-', '@', '\t' (タブ), '\00' (ヌル文字) などの特殊文字である場合、シングルクォートを使用してエスケープする必要がある。 | ✓ | ✓ | ✓ | 1236 |
| **5.3.12** | [追加] LaTeX プロセッサは ("--shell-escape" フラグを使用しないなど) 安全に構成されており、LaTeX インジェクション攻撃を防ぐためにコマンド許可リストを使用している。 | | ✓ | ✓ | |
| **5.3.13** | [追加, 5.3.1 から分割] URL を動的に構築する場合、信頼できないデータはそのコンテキストに応じてエンコードされている (例: クエリやパスパラメータの URL エンコーディングや base64url エンコーディング)。安全な URL プロトコルのみが許可されるようにしている (例: javascript: や data: を許可しない)。 | ✓ | ✓ | ✓ | 116 |
| **5.3.14** | [追加] 出力エンコーディングは、上記以外の潜在的に危険なインタプリタが使用されているコンテキストで必要なインタプリタとコンテキストに関連している。 | | ✓ | ✓ | |

注:クエリのパラメータ化や SQL のエスケープだけでは必ずしも十分ではありません。テーブル名やカラム名、ORDER BY などはエスケープできません。これらのフィールドにエスケープされたユーザ作成データが含まれていると、クエリの失敗や SQL インジェクションが発生します。

注: SVG フォーマットは、ほとんどすべてのコンテキストで ECMA スクリプトを明示的に許可しているため、すべての SVG XSS ベクターを完全にブロックすることは不可能かもしれません。SVG のアップロードが必要な場合は、アップロードされたファイルを text/plane として提供するか、別のユーザ指定コンテンツドメインを使用して、アプリケーションから引き継がれた XSS を防止します。

## V5.4 メモリ、文字列、アンマネージドコード

以下の要件は、アプリケーションがシステム言語またはアンマネージドコードを使用している場合にのみ適用されます。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.4.1** | メモリセーフな文字列、安全なメモリコピー、ポインタ演算を使って、スタック、バッファ、ヒープのオーバーフローをアプリケーションが検出または防止する。 | | ✓ | ✓ | 120 |
| **5.4.2** | [削除, 5.2.13 へ移動] | | | | |
| **5.4.3** | 整数オーバーフローを防ぐために符号、範囲および入力のバリデーションが使用されている。 | | ✓ | ✓ | 190 |

## V5.5 デシリアライゼーション防御

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.5.1** | [削除, 不正確] | | | | |
| **5.5.2** | アプリケーションが XML パーサを可能な限り最も制限の厳しい構成のみを使用するように正しく制限し、外部エンティティの解決などの危険な機能を無効にして XML 外部エンティティ (XML eXternal Entity, XXE) 攻撃を防ぐようにしている。 | ✓ | ✓ | ✓ | 611 |
| **5.5.3** | [修正, 1.5.2 からマージ] 信頼できないウライアントと通信するときにデシリアライゼーションが使用される場合、入力は安全に処理されている。たとえば、デシリアライゼーション攻撃を防ぐために、オブジェクトタイプの許可リストのみを許可するか、クライアントがデシリアライズ先のオブジェクトタイプを定義できないようにしている。 | ✓ | ✓ | ✓ | 502 |
| **5.5.4** | [削除, 5.2.4 と重複] | | | | |
| **5.5.5** | [修正, 13.1.1 から移動, レベル L1 > L2] JSON 相互運用性の脆弱性や、さまざまな URI やファイルの解析動作がリモートファイルインクルージョン (RFI) やサーバサイドリクエストフォージェリ (SSRF) 攻撃で悪用されるような問題を回避するために、同じデータ型に対してアプリケーションで使用されるさまざまなパーサー (JSON パーサー、XML パーサー、URL パーサーなど) は一貫した方法で解析を実行し、同じエンコードメカニズムを使用する。 | | ✓ | ✓ | 436 |

## V5.6 バリデーションおよびサニタイゼーションアーキテクチャ

構文固有の要件では「正しいことを行う」といいますが、ここでは「正しい順序で行う」および「正しい場所で行う」という要件があります。

また、この要件は、二重エンコーディング問題を防ぐために、データが保存されるときは常に、エンコードされた状態 (HTML エンコーディングなど) ではなく元の状態で保存されることを確保することを目的としています。

<!--
以下の場合、要件はここに属します。

  * 入力バリデーション、サニタイゼーション、エンコーディングアーキテクチャ
  * 入力バリデーション、サニタイゼーション、エンコーディング処理 (順序)

以下の場合、要件はここに属しません。

  * 構文固有または明確な入力バリデーション、サニタイゼーション、エンコーディング要件

再構成: 段落の最初の章に移動します
-->

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.6.1** | [追加] 入力は一度だけ標準形式にデコードまたはアンエスケープされ、これは入力をさらに処理する前に行われる。たとえば、入力バリデーションやサニタイゼーションの後には実行されない。 | ✓ | ✓ | ✓ | 174 |
| **5.6.2** | [修正, 1.5.3 から移動, レベル L2 > L1] アプリケーションは信頼できるサービスレイヤで入力バリデーションを実行するように設計されている。クライアントサイドのバリエーションはユーザービリティを向上しますが、セキュリティはそれに依存してはいけない。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 602 |
| **5.6.3** | [修正, 1.5.4 から移動] アプリケーションはそれが意図されているインタプリタまたはインタプリタ自体によって使用される前の最終ステップとして、出力エンコーディングおよびエスケープを実行する。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 116 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Testing Guide 4.0: Input Validation Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/README.html)
* [OWASP Cheat Sheet: Input Validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Testing for HTTP Parameter Pollution](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution.html)
* [OWASP LDAP Injection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LDAP_Injection_Prevention_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Client-Side Testing](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/11-Client-side_Testing/README)
* [OWASP Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
* [OWASP DOM Based Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)
* [OWASP Java Encoding Project](https://owasp.org/owasp-java-encoder/)
* [OWASP Mass Assignment Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mass_Assignment_Cheat_Sheet.html)
* [DOMPurify - Client-side HTML Sanitization Library](https://github.com/cure53/DOMPurify)
* [XML External Entity (XXE) Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)

自動エスケープの詳細な情報はこちらをご覧ください。

* [Reducing XSS by way of Automatic Context-Aware Escaping in Template Systems](https://googleonlinesecurity.blogspot.com/2009/03/reducing-xss-by-way-of-automatic.html)
* [AngularJS Strict Contextual Escaping](https://docs.angularjs.org/api/ng/service/$sce)
* [AngularJS ngBind](https://docs.angularjs.org/api/ng/directive/ngBind)
* [Angular Sanitization](https://angular.io/guide/security#sanitization-and-security-contexts)
* [Angular Security](https://angular.io/guide/security)
* [ReactJS Escaping](https://reactjs.org/docs/introducing-jsx.html#jsx-prevents-injection-attacks)
* [Improperly Controlled Modification of Dynamically-Determined Object Attributes](https://cwe.mitre.org/data/definitions/915.html)

デシリアライゼーションやパースの問題の詳細情報はこちらをご覧ください。

* [OWASP Deserialization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)
* [OWASP Deserialization of Untrusted Data Guide](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
* [An Exploration of JSON Interoperability Vulnerabilities](https://bishopfox.com/blog/json-interoperability-vulnerabilities)
* [Orange Tsai - A New Era of SSRF Exploiting URL Parser In Trending Programming Languages](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf)
