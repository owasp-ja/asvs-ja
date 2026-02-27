# 付録 C: 暗号化標準

「暗号化」の章は単にベストプラクティスを定義するだけではありません。暗号の原則に対する理解を深め、より耐性のある最新のセキュリティ手法の採用を促進することを目的としています。この付録では、「暗号化」の章で概説されている包括的な標準を保管するために、各要件に関する詳細な技術情報を提供します。

この付録ではさまざまな暗号メカニズムの承認レベルを定義します。

* 承認済み (Approved) (A) メカニズムはアプリケーションで使用できます。
* レガシー (Legacy) メカニズム (L) はアプリケーションで使用すべきではありませんが、既存のレガシーアプリケーションやコードとの互換性のためだけに依然として使用されるかもしれません。このようなメカニズムの使用自体は現在のところ脆弱性とはみなされませんが、できるだけ早く、より安全で将来性のあるメカニズムに置き換えるべきです。
* 不許可 (Disallowed) メカニズム (D) は、現時点で破壊されているとみなされているか、十分なセキュリティを提供していないため、使用してはいけません。

このリストは、以下のようなさまざまな理由により、特定のアプリケーションのコンテキストにおいて上書きされる可能性があります。

* 暗号分野における新たな進化
* 規制への準拠

## 暗号インベントリとドキュメント

このセクションでは V11.1 暗号インベントリとドキュメントに対する追加情報を提供します。


アルゴリズム、鍵、証明書などの暗号資産を定期的に発見し、インベントリ化し、評価することが重要です。レベル 3 では、これにはアプリケーションでの暗号の使用を発見するための静的スキャンおよび動的スキャンの使用を含むべきです。SAST や DAST などのツールがこれに役立つかもしれませんが、より包括的なカバレッジを得るには専用のツールが必要になるかもしれません。フリーウェアのツールの例には以下のものがあります。

* [CryptoMon - Network Cryptography Monitor - using eBPF, written in python](https://github.com/Santandersecurityresearch/CryptoMon)
* [Cryptobom Forge Tool: Generating Comprehensive CBOMs from CodeQL Outputs](https://github.com/Santandersecurityresearch/cryptobom-forge)

## 暗号パラメータの等価な強度

さまざまな暗号システムの相対的なセキュリティ強度は以下の表のとおりです ([NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final), p.71 より):

| セキュリティ強度 | 対称鍵アルゴリズム | 有限体 | 整数因数分解 | 楕円曲線 |
|--|--|--|--|--|
| <= 80 | 2TDEA | L = 1024 <br> N = 160 | k = 1024 | f = 160-223 |
| 112 | 3TDEA   | L = 2048 <br> N = 224 | k = 2048 | f = 224-255 |
| 128 | AES-128 | L = 3072 <br> N = 256 | k = 3072 | f = 256-383 |
| 192 | AES-192 | L = 7680 <br> N = 384 | k = 7680 | f = 384-511 |
| 256 | AES-256 | L = 15360 <br> N = 512 | k = 15360 | f = 512+ |

適用例:

* 有限体暗号: DSA, FFDH, MQV
* 整数因数分解暗号: RSA
* 楕円曲線暗号: ECDSA, EdDSA, ECDH, MQV

注: このセクションでは量子コンピュータが存在しないことを前提としています。そのようなコンピュータが存在する場合、最後の 3 列の推定値はもはや有効ではなくなります。

## 乱数値

このセクションでは V11.5 乱数値に対する追加情報を提供します。


| 名称 | バージョン/リファレンス | 備考 | ステータス |
|:---|:----|:----|:-:|
| `/dev/random` | Linux 4.8 以降 [(2016 年 10 月)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=818e607b57c94ade9824dad63a96c2ea6b21baf3)、iOS、Android、その他の Linux ベースの POSIX オペレーティングシステムにもあります。[RFC7539](https://datatracker.ietf.org/doc/html/rfc7539) に基づいています。 | ChaCha20 ストリームを利用します。iOS [`SecRandomCopyBytes`](https://developer.apple.com/documentation/security/secrandomcopybytes(_:_:_:)?language=objc) および Android [`Secure Random`](https://developer.android.com/reference/java/security/SecureRandom) にあり、それぞれに正しい設定が提供されています。 | A |
| `/dev/urandom` | ランダムデータを提供するための Linux カーネルの特殊ファイルです。 | ハードウェアのランダム性から高性能のエントロピーソースを提供します。 | A |
| `AES-CTR-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | [`BCRYPT_RNG_ALGORITHM`](https://learn.microsoft.com/en-us/windows/win32/seccng/cng-algorithm-identifiers) で設定された [Windows CNG API `BCryptGenRandom`](https://learn.microsoft.com/en-us/windows/win32/api/bcrypt/nf-bcrypt-bcryptgenrandom) などの一般的な実装で使用されます。 | A |
| `HMAC-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | A |
| `Hash-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | A |
| `getentropy()` | [OpenBSD](https://man.openbsd.org/getentropy.2)、[Linux glibc 2.25 以降](https://man7.org/linux/man-pages/man3/getentropy.3.html)、[macOS 10.12 以降](https://support.apple.com/en-gb/guide/security/seca0c73a75b/web) で利用可能です。 | シンプルで最小限の API で、カーネルのエントロピーソースから安全なランダムバイトを直接提供します。より現代的であり、古い API に関連する落とし穴を回避します。 | A |

HMAC-DRBG または Hash-DRBG で使用される基礎となるハッシュ関数はこの用途に対して承認されていなければなりません。

## 暗号アルゴリズム

このセクションでは V11.3 暗号アルゴリズムに対する追加情報を提供します。


承認済み暗号アルゴリズムを優先順に並べています。

| 対称鍵アルゴリズム | リファレンス | ステータス |
| ------ | ------ |:-:|
| AES-256 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | A |
| Salsa20 | [Salsa 20 specification](https://cr.yp.to/snuffle/spec.pdf) | A |
| XChaCha20 | [XChaCha20 Draft](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-xchacha-03) | A |
| XSalsa20 | [Extending the Salsa20 nonce](https://cr.yp.to/snuffle/xsalsa-20110204.pdf) | A |
| ChaCha20 | [RFC 8439](https://www.rfc-editor.org/info/rfc8439) | A |
| AES-192 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | A |
| AES-128 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | L |
| 2TDEA | | D |
| TDEA (3DES/3DEA) | | D |
| IDEA | | D |
| RC4 | | D |
| Blowfish| | D |
| ARC4 | | D |
| DES | | D |

### AES 暗号モード

AES などのブロック暗号はさまざまな動作モードで使用できます。電子コードブック (ECB) などの多くの動作モードは安全ではないため、使用してはいけません。ガロア/カウンタモード (Galois/Counter Mode, GCM) とカウンタ暗号ブロック連鎖メッセージ認証コード (CCM) の動作モードは、認証済み暗号化を提供するため、現代のアプリケーションでは使用すべきです。

承認済みモードを優先順に並べています。

| モード | 認証済み | リファレンス | ステータス | 制限 |
|--|--|--|:-:|--|
| GCM | Yes | [NIST SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | A | |
| CCM | Yes | [NIST SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | A | |
| CBC | No | [NIST SP 800-38A](https://csrc.nist.gov/pubs/sp/800/38/a/final) | L | |
| CCM-8 | Yes | | D | |
| ECB | No | | D | |
| CFB | No | | D | |
| OFB | No | | D | |
| CTR | No | | D | |

注:

* すべての暗号化されたメッセージは認証されなければなりません。CBC モードを使用する場合は必ず、メッセージを検証するために、関連するハッシュ MAC アルゴリズムがなければなりません (MUST)。一般的に、これは Encrypt-Then-Hash 方式で適用されなければなりません (MUST) (ただし、TLS 1.2 では代わりに Hash-Then-Encrypt を使用します)。これが保証できない場合、CBC を使用してはいけません (MUST NOT)。MAC アルゴリズムなしでの暗号化が許される唯一のアプリケーションはディスク暗号化です。
* CBC を使用する場合、パディングの検証が一定時間で行われることを保証する必要があります。
* CCM-8 を使用する場合、MAC タグは 64 ビットのセキュリティしか持ちません。これは、少なくとも 128 ビットのセキュリティを必要とする要件 11.2.3 に準拠していません。
* ディスク暗号化は ASVS のスコープ外と考えられます。したがって、この付録ではディスク暗号化の承認済みの方式は記載しません。この用途では、通常、認証なしの暗号化が受け入れられ、XTS、XEX、LRW モードが一般的に使用されます。

### 鍵ラッピング

暗号鍵ラップ (および対応する鍵アンラップ) は、追加の暗号メカニズムを使用して既存の鍵をカプセル化 (つまりラップ) し、転送中などに元の鍵が明らかに露出しないようにすることで、既存の鍵を保護する方法です。元の鍵を保護するために使用されるこの追加の鍵はラップ鍵と呼ばれます。

この操作は、信頼できないとみなされる場所で鍵を保護したい場合、あるいは信頼できないネットワーク上やアプリケーション内で機密鍵を送信したい場合に実行できます。
ただし、ラップ/アンラップ手順を実行する前に、元の鍵の性質 (アイデンティティや目的など) を理解することを真剣に検討すべきです。これは、セキュリティと、特に鍵の機能 (署名など) の監査証跡や適切な鍵の保存を含むコンプライアンスの点で、ソースとターゲットの両方のシステム/アプリケーションに影響を及ぼす可能性があります。

特に、鍵ラッピングには、[NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) に従い、量子脅威に対する将来を見据えた対策を考慮して、AES-256 を使用しなければなりません (MUST)。AES を使用する暗号モードは優先順に以下のとおりです:

| 鍵ラッピング | リファレンス | ステータス |
|--|--|:-:|
| KW | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | A |
| KWP | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | A |

AES-192 と AES-128 はユースケースで必要な場合に使用できます (MAY) が、その理由はエンティティの暗号インベントリに文書化されなければなりません (MUST)。

### 認証された暗号

ディスク暗号化を除いて、暗号化されたデータは何かしらの形で認証された暗号 (AE) スキーム、通常は関連データ付き認証暗号 (AEAD) スキームを使用して、認可されていない変更から保護されなければなりません。

アプリケーションは承認済みの AEAD スキームを使用することをお勧めします。代わりに、承認済みの暗号スキームと承認済みの MAC アルゴリズムを Encrypt-then-MAC 構造で組み合わせることもできます。

MAC-then-encrypt はレガシーアプリケーションとの互換性のために依然として許可されています。これは古い暗号スイートを使用する TLS v1.2 で使用されます。

| AEAD メカニズム | リファレンス | ステータス |
|---|---------|:-:|
| AES-GCM | [SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | A |
| AES-CCM | [SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | A |
| ChaCha-Poly1305 | [RFC 7539](https://datatracker.ietf.org/doc/html/rfc7539) | A |
| AEGIS-256  | [AEGIS: A Fast Authenticated Encryption Algorithm (v1.1)](https://competitions.cr.yp.to/round3/aegisv11.pdf) | A |
| AEGIS-128  | [AEGIS: A Fast Authenticated Encryption Algorithm (v1.1)](https://competitions.cr.yp.to/round3/aegisv11.pdf) | A |
| AEGIS-128L | [AEGIS: A Fast Authenticated Encryption Algorithm (v1.1)](https://competitions.cr.yp.to/round3/aegisv11.pdf) | A |
| Encrypt-then-MAC | | A |
| MAC-then-encrypt | | L |

## ハッシュ関数

このセクションでは V11.4 ハッシュ化とハッシュベース関数に対する追加情報を提供します。


### 一般的なユースケースでのハッシュ関数

以下の表は、デジタル署名などの一般的な暗号ユースケースで承認されているハッシュ関数を示しています。

* 承認済みハッシュ関数は強力な衝突耐性を備えており、高セキュリティのアプリケーションに適しています。
* これらのアルゴリズムの中には、適切な暗号鍵管理と併用すると攻撃に対する強力な耐性を発揮するものがあるため、HMAC、KDF、RBG 機能についても追加的に承認されています。
* 出力が 254 ビット未満のハッシュ関数は衝突耐性が不十分であるため、デジタル署名や衝突耐性を必要とするその他の用途には使用してはいけません。その他の用途では、レガシーシステムとの互換性と検証にのみ使用できます (ONLY) が、新しい設計には使用してはいけません。

| ハッシュ関数 | リファレンス | ステータス | 制限 |
| ------ | ----------- |:-:| ---------- |
| SHA3-512 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-512 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA3-384 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-384 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA3-256 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-512/256 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA-256 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHAKE256 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| BLAKE2s | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | A | |
| BLAKE2b | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | A | |
| BLAKE3 | [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf) | A | |
| SHA-224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA-512/224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA3-224 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA-1 | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| CRC (any length) |  | D |  |
| MD4 | [RFC 1320](https://www.rfc-editor.org/info/rfc1320) | D | |
| MD5 | [RFC 1321](https://www.rfc-editor.org/info/rfc1321) | D | |

### パスワード保存のためのハッシュ関数

安全なパスワードハッシュには、専用のハッシュ関数を使用しなければなりません。これらの低速ハッシュアルゴリズムは、パスワードクラッキングの計算難易度を上げることで、ブルートフォース攻撃や辞書攻撃を軽減します。

| 鍵導出関数 | リファレンス | 必要なパラメータ | ステータス |
| ---------- | --------- | ------------ |:-:|
| argon2id | [RFC 9106](https://www.rfc-editor.org/info/rfc9106) | t = 1: m ≥ 47104 (46 MiB), p = 1 | A |
|          |                                                     | t = 2: m ≥ 19456 (19 MiB), p = 1 | A |
|          |                                                     | t ≥ 3: m ≥ 12288 (12 MiB), p = 1 | A |
| scrypt   | [RFC 7914](https://www.rfc-editor.org/info/rfc7914) | p = 1: N ≥ 2^17 (128 MiB), r = 8 | A |
|          |                                                     | p = 2: N ≥ 2^16 (64 MiB), r = 8  | A |
|          |                                                     | p ≥ 3: N ≥ 2^15 (32 MiB), r = 8  | A |
| bcrypt | [A Future-Adaptable Password Scheme](https://www.researchgate.net/publication/2519476_A_Future-Adaptable_Password_Scheme) | cost ≥ 10 | A |
| PBKDF2-HMAC-SHA-512 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 210,000 | A |
| PBKDF2-HMAC-SHA-256 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 600,000 | A |
| PBKDF2-HMAC-SHA-1 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 1,300,000 | L |

承認済みのパスワードベースの鍵導出関数はパスワードストレージとして使用できます。

## 鍵導出関数 (KDF)

### 汎用の鍵導出関数

| 鍵導出関数       | リファレンス                                                                                  | ステータス |
| ---------------- | -------- |:-:|
| HKDF             | [RFC 5869](https://www.rfc-editor.org/info/rfc5869)                                           | A          |
| TLS 1.2 PRF      | [RFC 5248](https://www.rfc-editor.org/info/rfc5248)                                           | L          |
| MD5-based KDFs   | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                           | D          |
| SHA-1-based KDFs | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) | D          |

### パスワードベースの鍵導出関数

| 鍵導出関数 | リファレンス | 必要なパラメータ | ステータス |
| ---------- | --------- | ------------ |:-:|
| argon2id   | [RFC 9106](https://www.rfc-editor.org/info/rfc9106) | t = 1: m ≥ 47104 (46 MiB), p = 1 | A |
|            |                                                     | t = 2: m ≥ 19456 (19 MiB), p = 1 | A |
| scrypt     | [RFC 7914](https://www.rfc-editor.org/info/rfc7914) | p = 1: N ≥ 2^17 (128 MiB), r = 8 | A |
|            |                                                     | p = 2: N ≥ 2^16 (64 MiB), r = 8  | A |
|            |                                                     | p ≥ 3: N ≥ 2^15 (32 MiB), r = 8  | A |
| PBKDF2-HMAC-SHA-512 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 210,000 | A |
| PBKDF2-HMAC-SHA-256 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 600,000 | A |
| PBKDF2-HMAC-SHA-1 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 1,300,000 | L |

## 鍵交換メカニズム

このセクションでは V11.6 公開鍵暗号に対する追加情報を提供します。


### KEX スキーム

すべての鍵交換スキームに対して 112 ビット以上のセキュリティ強度が確保されていなければならず (MUST)、その実装は以下の表のパラメータ選択に従わなければなりません (MUST)。

| スキーム | ドメインパラメータ | 前方秘匿性 | ステータス |
|--|--|--|:-:|
| Finite Field Diffie-Hellman (FFDH) | L >= 3072 & N >= 256 | Yes | A |
| Elliptic Curve Diffie-Hellman (ECDH) | f >= 256-383 | Yes | A |
| Encrypted key transport with RSA-PKCS#1 v1.5 | | No | D |

ここでのパラメータは以下のとおりです:

* k は RSA 鍵の鍵サイズです。
* L は有限体暗号の公開鍵 (public key) のサイズであり、N は秘密鍵 (private key) のサイズです。
* f は ECC の鍵サイズの範囲です。

新しい実装では [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final) & [B](https://csrc.nist.gov/pubs/sp/800/56/b/r2/final) および [NIST SP 800-77](https://csrc.nist.gov/pubs/sp/800/77/r1/final) に準拠していないスキームを使用してはいけません (MUST NOT)。特に IKEv1 は本番環境で使用してはいけません (MUST NOT)。

### Diffie-Hellman グループ

Diffie-Hellman 鍵交換の実装には以下のグループが承認されています。セキュリティ強度は [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final), Appendix D および [NIST SP 800-57 Part 1 Rev.5](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に記載されています。

| グループ         | ステータス |
|------------------|:------:|
| P-224, secp224r1 | A      |
| P-256, secp256r1 | A      |
| P-384, secp384r1 | A      |
| P-521, secp521r1 | A      |
| K-233, sect233k1 | A      |
| K-283, sect283k1 | A      |
| K-409, sect409k1 | A      |
| K-571, sect571k1 | A      |
| B-233, sect233r1 | A      |
| B-283, sect283r1 | A      |
| B-409, sect409r1 | A      |
| B-571, sect571r1 | A      |
| Curve448         | A      |
| Curve25519       | A      |
| MODP-2048        | A      |
| MODP-3072        | A      |
| MODP-4096        | A      |
| MODP-6144        | A      |
| MODP-8192        | A      |
| ffdhe2048        | A      |
| ffdhe3072        | A      |
| ffdhe4096        | A      |
| ffdhe6144        | A      |
| ffdhe8192        | A      |

## メッセージ認証コード (MAC)

メッセージ認証コード (MAC) は、メッセージの完全性と真正性を検証するために使用される暗号構造です。MAC は、メッセージと共有鍵 (secret key) を入力として受け取り、固定サイズのタグ (MAC 値) を生成します。MAC は安全な通信プロトコル (TLS/SSL など) で広く使用され、パーティ間で交換されるメッセージが本物で無傷であることを確保します。

| MAC アルゴリズム  | リファレンス | ステータス |
| ----------    | --------------- |:-:|
| HMAC-SHA-256  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A |
| HMAC-SHA-384  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A |
| HMAC-SHA-512  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A |
| KMAC128       | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | A |
| KMAC256       | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | A |
| BLAKE3 (keyed_hash mode) | [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf)  | A |
| AES-CMAC      | [RFC 4493](https://datatracker.ietf.org/doc/html/rfc4493) & [NIST SP 800-38B](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38b.pdf) | A |
| AES-GMAC      | [NIST SP 800-38D](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf)            | A |
| Poly1305-AES  | [The Poly1305-AES message-authentication code](https://cr.yp.to/mac/poly1305-20050329.pdf)                  | A |
| HMAC-SHA-1    | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | L |
| HMAC-MD5      | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                | D      |

## デジタル署名

署名スキームは [NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に従って承認された鍵サイズとパラメータを使用しなければなりません (MUST):

| 署名アルゴリズム               | リファレンス                                               | ステータス |
| ------------------------------ | ---------------------------------------------              | :-:    |
| EdDSA (Ed25519, Ed448)         | [RFC 8032](https://www.rfc-editor.org/info/rfc8032)        | A      |
| XEdDSA (Curve25519, Curve448)  | [XEdDSA](https://signal.org/docs/specifications/xeddsa/)   | A      |
| ECDSA (P-256, P-384, P-521)    | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-5/final)  | A      |
| RSA-RSSA-PSS                   | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)        | A      |
| RSA-SSA-PKCS#1 v1.5            | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)        | D      |
| DSA (any key size)             | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-4/final)  | D      |

## ポスト量子暗号標準

ポスト量子暗号 (PQC) 実装は [FIPS-203](https://csrc.nist.gov/pubs/fips/203/ipd)、[FIPS-204](https://csrc.nist.gov/pubs/fips/204/ipd)、[FIPS-205](https://csrc.nist.gov/pubs/fips/205/ipd) に準拠する必要があります。現時点では、これらの標準に対応した堅牢化されたコード例やリファレンス実装は多くありません。詳細については、[NIST による最初の三つの最終決定されたポスト量子暗号標準に関する発表 (2024年8月)](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards) を参照してください。

提案されている [mlkem768x25519](https://datatracker.ietf.org/doc/draft-kwiatkowski-tls-ecdhe-mlkem/03/) ポスト量子ハイブリッド TLS 鍵交換方式は、[Firefox リリース 132](https://www.mozilla.org/en-US/firefox/132.0/releasenotes/) や [Chrome リリース 131](https://security.googleblog.com/2024/09/a-new-path-for-kyber-on-web.html) などの主要なブラウザでサポートされています。暗号テスト環境や、業界や政府が承認したライブラリ内で利用可能な場合に使用できます (MAY)。
