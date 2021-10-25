# V5 バリデーション、サニタイゼーション、エンコーディング

## 管理目標

最も一般的な Web アプリケーションのセキュリティ上の弱点は、クライアントや環境からの入力を、出力エンコーディングなしで直接使用する前に適切に検証できないことです。この弱点は、クロスサイトスクリプティング (XSS)、SQL インジェクション、インタプリタインジェクション、ロケール/Unicode 攻撃、ファイルシステム攻撃、バッファオーバーフローなど、Web アプリケーションの重要な脆弱性のほとんどすべてにつながっています。

検証対象のアプリケーションが以下の上位要件を満たすことを確認します。

* 入力のバリデーションと出力エンコーディングのアーキテクチャは、インジェクション攻撃を防ぐために合意されたパイプラインを持っています
* 入力データは強力に型付けされ、バリデーションされ、範囲や長さがチェックされ、最悪の場合、サニタイズされ、フィルタリングされていること
* 出力データは、データのコンテキストに応じて、可能な限りインタプリタに近い形でエンコード、エスケープされること

Wモダンな Web アプリケーションのアーキテクチャにおいては、出力エンコーディングはこれまで以上に重要です。特定のシナリオでは、堅牢な入力バリデーションを行うことは難しく、パラメータ化されたクエリ、テンプレートフレームワークによる自動エスケープ、または慎重に選択された出力エンコーディングのようなより安全な API を使用することは、アプリケーションのセキュリティにとって非常に重要です。

## V5.1 入力バリデーション

ポジティブ許可リストと強いデータ型付けを使用して、入力バリデーション制御を適切に実装すると、インジェクション攻撃を排除できる場合があります。しかし、入力バリデーションがセキュリティに有効でない場合もあります。たとえば、攻撃を成功させるために有効な電子メールアドレスや URL を使用することがあります。

入力バリデーションは依然としいて重要なセキュリティ衛生であり、可能な限りすべての入力に適用すべきです。

入力バリデーションがセキュリティに役立たないこともあれば、中程度に役立つこともあり、セキュリティ防御の要となることもあります。入力バリデーションがどれほど有効であるかは、データの種類とそのデータの使用方法に依存します。入力バリデーションは完全なセキュリティ戦略ではないため、サンドボックス、サニタイゼーション、エンコーディング、パラメータ化なども活用する必要があります。



| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **5.1.1** | アプリケーションが HTTP 変数汚染攻撃に対する防御策を備えている。特にアプリケーションフレームワークが、リクエストパラメータのソース (GET、POST、Cookie、ヘッダ、環境など) を区別しない場合はこの防御が必要。 | ✓ | ✓ | ✓ | 235 |
| **5.1.2** | フレームワークが大量のパラメータ割り当て攻撃から保護している、またはフィールドを非公開にするなど安全でないパラメータの代入を防ぐための対策が行われている。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 915 |
| **5.1.3** | すべての入力データがバリデーションされている。入力データには、HTML のフォームのフィールド、REST 呼び出し、クエリパラメータ、HTTP ヘッダ、Cookie、バッチファイル、RSS フィードなども含む。バリデーションは許可リスト方式で行う。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 20 |
| **5.1.4** | [修正] 構造化データが強く型付けされており、使用可能な文字、長さ、パターン等の定義されたスキーマに基づいてバリデーションされる。（例：クレジットカード番号、電子メールアドレス、電話番号などのバリデーションや、地区名と郵便番号等 2 つの関連するフィールドのデータが妥当なことのバリデーション） ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 20 |
| **5.1.5** | URL のリダイレクト先と転送先がホワイトリストに登録された宛先のみ許可されている、または信頼できない可能性のあるコンテンツにリダイレクトするときに警告を表示する。 | ✓ | ✓ | ✓ | 601 |
| **5.1.6** | [1.5.3 から移動, レベル L2 > L1] 信頼できるサービスレイヤで入力の妥当性確認が実施されている。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 602 |


## V5.2 サニタイゼーションとサンドボックス化

入力バリデーションは複雑なトピックです。

入力バリデーションがセキュリティに役立たないこともあれば、中程度に役立つこともあり、セキュリティ防御の要となることもあります。入力バリデーションがどれほど有効であるかは、データの種類とそのデータの使用方法に依存します。

例:

  * サニタイゼーション: ユーザが HTML をオーサリングする場合、標準的な防御策は HTML を標準化して削除することです。 JSON パーサーを使用する前に JSON サニタイズを実行します。もちろん XSS 防御のために HTML サニタイゼーションも行います。
  * エスケープ化: ユーザが入力したとおりにコンテンツを表示したい場合に UI で行われます。また LDAP インジェクション保護などのインジェクション保護のためにも行われます。
  * パラメータ化: 主に SQL インジェクションに対して行われます。
  * サンドボックス化: 何らかの理由で HTML をサニタイズできず、潜在的にアクティブなコンテンツを Web ページにダンプする必要がある場合、 iFrame サンドボックス化は非常に重要です。 CSP にもサンドボックス化の機能があります。
  * Web UI の URL では JavaScript やデータ URL を止めることが非常に重要です (XSS 対策) 。しかし多くの場合には有効なデータは依然として危険です。


| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **5.2.1** | WYSIWYG エディタ等から取得した信頼できない HTML 入力が、HTML サニタイザライブラリもしくはフレームワークの機能によって適切にサニタイズされ、入力バリデーションやエンコードにより適切に処理されている。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 116 |
| **5.2.2** | 非構造化データがサニタイズされ、使用可能な文字や長さなど一般的な対策が適用されている。 | ✓ | ✓ | ✓ | 138 |
| **5.2.3** | SMTP または IMAP インジェクションから保護するため、アプリケーションがメールシステムに渡す前にユーザ入力をサニタイズする。 | ✓ | ✓ | ✓ | 147 |
| **5.2.4** | eval()もしくは動的コード実行機能を使用しない。代替方法がない場合は、実行前に含まれるユーザ入力のサニタイズもしくはサンドボックス化する。 | ✓ | ✓ | ✓ | 95 |
| **5.2.5** | テンプレートインジェクション攻撃に対して、ユーザ入力のサニタイズもしくはサンドボックス化によってアプリケーションが保護されている。 | ✓ | ✓ | ✓ | 94 |
| **5.2.6** | 信頼できないデータやファイル名や URL 入力フィールドなど HTTP ファイルメタデータのバリデーションまたはサニタイズや、プロトコル、ドメイン、パス、ポートに許可リストを使用することで、アプリケーションが SSRF 攻撃から保護されている。 | ✓ | ✓ | ✓ | 918 |
| **5.2.7** | ユーザの指定した Scalable Vector Graphics (SVG) スクリプト化可能コンテンツ（特に XSS に関連するインラインスクリプトや foreignObject）に対して、アプリケーションがサニタイズまたはサンドボックス化している。 | ✓ | ✓ | ✓ | 159 |
| **5.2.8** | ユーザ指定の Markdown、CSS、XSL スタイルシート、BBCode のようなスクリプト可能もしくは式のテンプレート言語コンテンツに対して、アプリケーションがサニタイズ、無効化またはサンドボックス化されている。 | ✓ | ✓ | ✓ | 94 |

## V5.3 出力エンコーディングとインジェクション防御

使用しているインタプリタの近くまたは隣接している出力エンコーディングは、アプリケーションのセキュリティにとって非常に重要です。通常、出力エンコーディングは永続化されず、適切な出力コンテキストで出力を安全にレンダリングして、すぐに使用されます。出力エンコードに失敗すると、安全ではなくインジェクション可能で危険なアプリケーションになります。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **5.3.1** | 出力エンコーディングがインタプリタとコンテキストが要求するものに関連している。例えば特に信頼できない入力 ("ねこ" や "O'Hara" など、Unicode 文字やアポストロフィを含む名前など) に対して、HTML 値、HTML 属性、JavaScript、URL パラメータ、HTML ヘッダ、SMTP、その他コンテキストが必要とするものに対して個別のエンコードを使用する。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 116 |
| **5.3.2** | [削除] | | | | |
| **5.3.3** | コンテキストに応じて (できれば自動化された、あるいは最悪でも手動による) 出力エスケープにより反射型、格納型、および DOM ベース XSS から保護されている。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 79 |
| **5.3.4** | データ選択またはデータベースクエリ（例、SQL、HQL、ORM、NoSQL）がクエリのパラメータ化、ORM、エンティティフレームワークもしくは他の方法により保護されており、データベースインジェクション攻撃の影響を受けない。 ([C3](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 89 |
| **5.3.5** | パラメータ化もしくはより安全な機構が存在しない場合、SQL インジェクションから保護するための SQL エスケープの使用など、コンテキスト固有の出力エンコーディングによりインジェクション攻撃から保護されている。 ([C3, C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 89 |
| **5.3.6** | [修正] アプリケーションが JSON インジェクション攻撃、JSON eval 攻撃、および JavaScript 式評価から保護されている。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 830 |
| **5.3.7** | アプリケーションが LDAP インジェクションの影響を受けない、またはセキュリティ管理策によって LDAP インジェクションが防止される。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 90 |
| **5.3.8** | OS コマンドインジェクションに対して保護していること、およびオペレーティングシステムコールがパラメータ化された OS クエリを使用、もしくはコンテキストコマンドライン出力エンコーディングを使用する。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 78 |
| **5.3.9** | アプリケーションが、リモートファイルインクルード (RFI) やローカルファイルインクルード (LFI) の影響を受けない。 | ✓ | ✓ | ✓ | 829 |
| **5.3.10** | アプリケーションが XPath インジェクション攻撃や XML インジェクション攻撃から保護されている。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 643 |

注:クエリのパラメータ化や SQL のエスケープだけでは必ずしも十分ではありません。テーブル名やカラム名、ORDER BY などはエスケープできません。これらのフィールドにエスケープされたユーザ作成データが含まれていると、クエリの失敗や SQL インジェクションが発生します。

注: SVG フォーマットは、ほとんどすべてのコンテキストで ECMA スクリプトを明示的に許可しているため、すべての SVG XSS ベクターを完全にブロックすることは不可能かもしれません。SVG のアップロードが必要な場合は、アップロードされたファイルを text/plane として提供するか、別のユーザ指定コンテンツドメインを使用して、アプリケーションから引き継がれた XSS を防止します。

## V5.4 メモリ、文字列、アンマネージドコード

以下の要件は、アプリケーションがシステム言語またはアンマネージドコードを使用している場合にのみ適用されます。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **5.4.1** | メモリセーフな文字列、安全なメモリコピー、ポインタ演算を使って、スタック、バッファ、ヒープのオーバーフローをアプリケーションが検出または防止する。 | | ✓ | ✓ | 120 |
| **5.4.2** | フォーマット文字列は悪意のある入力を受け取らず、定数となっている。 | | ✓ | ✓ | 134 |
| **5.4.3** | 整数オーバーフローを防ぐために符号、範囲および入力のバリデーションが使用されている。 | | ✓ | ✓ | 190 |

## V5.5 デシリアライゼーション防御

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **5.5.1** | [削除] | | | | |
| **5.5.2** | アプリケーションが XML パーサを可能な限り最も制限の厳しい構成のみを使用するように正しく制限し、外部エンティティの解決などの危険な機能を無効にして XML 外部エンティティ (XML eXternal Entity, XXE) 攻撃を防ぐようにしている。 | ✓ | ✓ | ✓ | 611 |
| **5.5.3** | [修正, 1.5.2 からマージ] 信頼できないウライアントとの通信ではデシリアライゼーションが使用されていない。それができない場合は、デシリアライゼーション攻撃を防ぐために、オブジェクトタイプの許可リストのみを許可するか、クライアントがデシリアライズ先のオブジェクトタイプを定義できないようにするなど、デシリアライゼーションが安全に実行されるようにしている。 | ✓ | ✓ | ✓ | 502 |
| **5.5.4** | ブラウザもしくは JavaScript ベースのバックエンドで JSON をパースするときは JSON.parse を使用する。eval() を使用しない。 | ✓ | ✓ | ✓ | 95 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Testing Guide 4.0: Input Validation Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/README.html)
* [OWASP Cheat Sheet: Input Validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Testing for HTTP Parameter Pollution](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution.html)
* [OWASP LDAP Injection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LDAP_Injection_Prevention_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Client Side Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client_Side_Testing/)
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

デシリアライゼーションの詳細情報はこちらをご覧ください。

* [OWASP Deserialization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)
* [OWASP Deserialization of Untrusted Data Guide](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
