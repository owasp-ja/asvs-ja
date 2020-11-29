# V11: ビジネスロジックの検証要件

## 管理目標

検証対象のアプリケーションが次の高次の要件を満たすことを確認します。

* ビジネスロジックが正しい順序で処理
* ビジネスロジックに自動攻撃を検知し防止する制限が実装されている。自動攻撃の例としては，連続的な少額の送金や 1 度に 100 万人の友人を追加する，などがある
* 高い価値を持つビジネスロジックにおいて悪用ケースや悪用する人を想定している。また，なりしまし (spoofing)，改ざん (tampering)，否認 (repudiation)，情報の漏えい (information disclosure)，権限昇格 (elevation of privilege) 攻撃の対策を行っている

## V11.1 ビジネスロジックのセキュリティ要件

ビジネスロジックセキュリティは、すべてのアプリケーションごとに固有なため、どのチェックリストも適用はできません。そして外部からの脅威から保護するように設計されている必要がありますが、それは Web アプリケーションファイアウォール (firewalls) や安全な通信を使用しても保護することはできません。OWASP Cornucopia などのツールを使用して、デザインスプリント中に脅威モデリングを使用することを推奨します。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---:| :---: | :---: |
| **11.1.1** | アプリケーションが同じユーザのビジネスロジックフローを手順通り、省略せずに処理する。 | ✓ | ✓ | ✓ | 841 |
| **11.1.2** | アプリケーションは、すべてのステップが人間の実際に処理する時間で処理されたビジネスロジックフローのみ処理する。（送信されるのが早すぎるトランザクションは処理されない。） | ✓ | ✓ | ✓ | 779 |
| **11.1.3** | アプリケーションに適切な制限が実装されており、特定のビジネス活動やトランザクションに対し、ユーザごとに適用されている。 | ✓ | ✓ | ✓ | 770 |
| **11.1.4** | データの流出、ビジネスロジック要求、過剰なファイルのアップロード、DoS 攻撃 (denial of service attacks) を検出および保護するために、アプリケーションに充分な自動攻撃を検知し防止する制限がある。 | ✓ | ✓ | ✓ | 770 |
| **11.1.5** | アプリケーションは脅威モデリング (threat modelling) または類似の方法を使用して特定されたビジネスリスクから保護するために、ビジネスロジックの制限またはバリデーションがある。 | ✓ | ✓ | ✓ | 841 |
| **11.1.6** | アプリケーションの機密性の高い処理が「Time of Check to Time of Use」TOCTOU 問題や、その他の競合状態の影響を受けない。 | | ✓ | ✓ | 367 |
| **11.1.7** | アプリケーションはビジネスロジックの観点から異常なイベントまたはアクティビティを監視している。例えば、順序がおかしい行為や通常のユーザが決して試みない行為の試行が該当する。 ([C9](https://www.owasp.org/index.php/OWASP_Proactive_Controls#tab=Formal_Numbering)) | | ✓ | ✓ | 754 |
| **11.1.8** | アプリケーションは、自動攻撃または異常なアクティビティが検出されたときにアラートする設定機能がある。 | | ✓ | ✓ | 390 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Testing Guide 4.0: Business Logic Testing](https://www.owasp.org/index.php/Testing_for_business_logic)
* [OWASP Cheat Sheet](https://www.owasp.org/index.php/Business_Logic_Security_Cheat_Sheet)
* 対自動処理はこれらを含む、多くの方法で対応できる。 [OWASP AppSensor](https://www.owasp.org/index.php/OWASP_AppSensor_Project) 、 [OWASP Automated Threats to Web Applications](https://www.owasp.org/index.php/OWASP_Automated_Threats_to_Web_Applications)
* [OWASP AppSensor](https://www.owasp.org/index.php/OWASP_AppSensor_Project) は、攻撃検知と対応に役立つ。
* [OWASP Cornucopia](https://www.owasp.org/index.php/OWASP_Cornucopia)