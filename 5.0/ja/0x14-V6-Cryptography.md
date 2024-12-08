# V6 保存時の暗号化

## 管理目標

検証対象のアプリケーションが以下の上位要件を満たすことを確認します。

* すべての暗号モジュールが安全に失敗し、エラーが正しく処理されること
* 適切な乱数発生器が使用されていること
* 鍵へのアクセスが安全に管理されていること

## V6.1 データ分類

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.1.1** | [削除, 1.8.1 へマージ] | | | | |
| **6.1.2** | [削除, 1.8.1 へマージ] | | | | |
| **6.1.3** | [削除, 1.8.1 と重複] | | | | |

## V6.2 アルゴリズム

最近の暗号技術の進歩は、以前は安全だったアルゴリズムや鍵の長さが、データを保護するためにはもはや安全でも十分でもないことを意味しています。したがって、アルゴリズムを変更することは必要です。

このセクションはペネトレーションテストが難しく、ほとんどの項目で L1 が含まれていませんが、開発者はこのセクション全体を必須項目と考えてください。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.2.1** | すべての暗号化モジュールにおいて、処理に失敗した場合の安全対策が施されており、オラクルパディング攻撃を許さない方法でエラーが処理される。 | ✓ | ✓ | ✓ | 310 |
| **6.2.2** | 独自実装された暗号化方式ではなく、業界で実績のある、または政府が承認した暗号化アルゴリズム、モード、およびライブラリが使用されている。 | | ✓ | ✓ | 327 |
| **6.2.3** | [削除, 6.2.5 と重複] | | | | |
| **6.2.4** | 暗号の破壊から保護するため、乱数、暗号化またはハッシュアルゴリズム、鍵の長さ、ラウンド、暗号方式またはモードが、いつでも再構成やアップグレードもしくは入れ替えられる。 | | ✓ | ✓ | 326 |
| **6.2.5** | 既知の安全でないブロックモード（ECB など）、パディングモード（PKCS#1 v1.5 など）、ブロックサイズが小さい暗号（Triple-DES、Blowfish など）、および弱いハッシュアルゴリズム（MD5、SHA1 など）が使用されていない。 | | ✓ | ✓ | 326 |
| **6.2.6** | [修正, レベル L2 > L3] ナンス、初期化ベクトル、およびその他の使い捨ての数字が、暗号化鍵やデータ要素ペアで複数回使いまわしされていない。生成方法は、使用しているアルゴリズムに適切となっている。 | | | ✓ | 326 |
| **6.2.7** | 暗号化されたデータが署名、認証された暗号モード、または HMAC によって認証され、暗号文が権限のない者によって変更されない。 | | | ✓ | 326 |
| **6.2.8** | 情報の漏洩を防ぐために、すべての暗号化操作が、比較や計算または返戻の際にショートカット操作がなく、一定時間となっている。 | | | ✓ | 385 |

## V6.3 乱数値

暗号論的に安全な疑似乱数生成 (Cryptographically secure Pseudo-random Number Generation, CSPRNG) を正しく行うことは非常に困難です。一般的にシステム内のエントロピーの良いソースは使い過ぎるとすぐに枯渇してしまいますが、ランダム性の少ないソースは予測可能な鍵や秘密情報につながる可能性があります。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.3.1** | [修正] すべての乱数、ランダムなファイル名、ランダムな文字列は、攻撃者が推測できないことを意図している場合、暗号的に安全な疑似乱数ジェネレータ（Cryptographically-secure Pseudo-random Number Generator, CSPRNG）を用いて生成する。 | | ✓ | ✓ | 338 |
| **6.3.2** | [修正] GUID が暗号的に安全な疑似乱数ジェネレータ（Cryptographically-secure Pseudo-random Number Generator, CSPRNG）を利用する GUID v4 アルゴリズムの実装で作成されている。他のアルゴリズムを使用したり十分に安全でない疑似乱数ジェネレータを使用して作成された GUID は予測可能な場合がある。 | | ✓ | ✓ | 338 |
| **6.3.3** | アプリケーションの負荷が高い状態であっても、乱数が適切なエントロピーを使って生成される、あるいはアプリケーションがそのような環境で適切にデグレードする。 | | | ✓ | 338 |

## V6.4 シークレット管理

このセクションはペネトレーションテストが難しく、ほとんどの項目で L1 が含まれていませんが、開発者はこのセクション全体を必須項目と考えてください。

| # | 説明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.4.1** | [修正, 2.10.4 からマージ] 鍵保管庫のような秘密情報管理ソリューションが、パスワード、データベースやサードパーティシステムとの統合、シードや内部秘密情報、API キーなどのバックエンド秘密情報を安全に作成、保存、アクセス制御、破棄するために使用される。秘密情報にはソースコードに含めたり、CI/CD 変数として受け取ってはならない。L3 アプリケーションの場合、これには HSM などのハードウェア支援ソリューションを含むべきである。 | | ✓ | ✓ | 798 |
| **6.4.2** | [修正] 鍵マテリアルが (フロントエンドかバックエンドかに関わらず) アプリケーションに公開されていない。代わりに暗号化操作用の金庫のように隔離されたセキュリティモジュールを使用している。 | | ✓ | ✓ | 320 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Testing Guide 4.0: Testing for Weak Cryptography](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/README.html)
* [OWASP Cheat Sheet: Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
* [FIPS 140-3](https://csrc.nist.gov/pubs/fips/140-3/final)
