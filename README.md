# よく使う Faker のまとめです

詳しくは、旧本家サイトにて。　[旧本家サイト](https://github.com/fzaninotto/Faker)　[新本家サイト](https://github.com/fakerphp/faker)

Laravel 9.18 からの、fake() ヘルパー関数形式で書いています。<br>
プロパティ形式で書くのは、古い書き方で、いずれ機能しなくなりますので、メソッドで形式で書いて下さい。<br>
× fake()->name<br>
○ fake()->name()<br>

# 住所系

```php
fake()->postcode1();       	// 834（郵便番号3桁）
fake()->postcode2();       	// 8290（郵便番号4桁）
fake()->postcode();       	// 8348290（郵便番号7桁）
fake()->numerify('###-####');	// 834-8290（郵便番号3桁-4桁）
fake()->prefecture();     	// 東京都
fake()->ward();            	// 南区
fake()->city();            	// 鈴木市
fake()->streetAddress();   	// 斉藤町若松8-6-4
fake()->secondaryAddress();   	// ハイツ中村108号

fake()->areaNumber();		// 1～10 の間の値
fake()->buildingNumber();    	// 101～110 の間の値
```

# 個人情報（名前、メルアド、電話）

```php
fake()->name();			// 山田 太郎
fake()->firstName();		// 太郎
fake()->lastName();		// 山田
fake()->lastKanaName();		// ヤマダ
fake()->firstKanaName();		// タロウ or ハナコ（引数に'male', 'female' で性別指定可）
fake()->firstKanaNameMale();	// タロウ
fake()->firstKanaNameFemale();	// ハナコ

fake()->unique()->safeEmail();     // nakamura.ryohei@example.com（重複の無いメルアドで、実在しないドメイン）
fake()->phoneNumber(); 		// 0135-67-7343

fake()->company();  		// 株式会社 伊藤
```

# 文字列

```php
fake()->realText(10);    	// 日本語対応あり。最小 10～
    
fake()->sentence(8);            // タイトルなどに（英語）
fake()->paragraph(40);          // 本文などに（英語）
fake()->paragraphs(5, true);    // 改行コード付きの本文などに（英語）

fake()->regexify('[a-zA-Z0-9]{10}');   // 正規表現を使ったランダムな文字列
\Str::random(10);                            // laravelのヘルパー関数（英数字のみ）。例：「TkO41KdieO」
    
// 日本語でも改行コードほしい時は、以下。
preg_replace("/。/", "。\n\n", fake()->realText(150));
```

# 数字

```php
fake()->numerify('##');		// 2桁の数字（07 など 0 からスタートもあり）
fake()->numberBetween(1, 10);   // 1～10 の間の数字
fake()->biasedNumberBetween(1, 10, ['\Faker\Provider\Biased', 'linearLow']);
// 'linearLow'で、低い数字の出る確率大。反対は、'linearHigh'
```

# 配列

```php
fake()->randomElement(['a', 'b', 'c', 'd']);       // a～dの中からランダムに1つ
fake()->randomElements(['a', 'b', 'c', 'd'], 2);   // a～dの中からランダムに2つ（重複無し）
```

# 日時

```php
fake()->date('Y-m-d');			// 2002-12-10
fake()->time('H:i:00');			// 23:52:00
fake()->dateTime('now')->format('Y-m-d H:i:s');	  // 1977-08-13 09:40:21
fake()->dateTimeBetween('-3days', '3days')->format('Y-m-d');	// 2019-12-25

// フォーマットの指定が無い時は、DateTime オブジェクトが返る
```

# その他

```php
fake()->colorName();  	// Gold, Fuchsia, AntiqueWhite 等
fake()->url();         	// https://www.yahaoo.co.jp/
fake()->latitude(35.54915506146918, 36.06591802134296);    // 緯度
fake()->longitude(138.96409298125002, 140.30442501250002); // 経度
```

# 修飾子

### unique()

```php
// 重複しないようにデータを返す
// 但し、それ以上重複しない値を返せない時は、エラーとなる
fake()->unique()->safeEmail();
fake()->unique()->randomElement([1, 2, 3]);  // 4 回呼び出してしまうとエラー
```

### optional()

```php
// null（デフォルト値）を時折混ぜたい時に便利。
fake()->optional(0.1)->randomElement([1, 2, 3]);  // 90%の確率でnullを返す
```

# おまけ

```php
// laravel で Faker の日本語設定
APP_FAKER_LOCALE=ja_JP        // .env の場合（Laravel 11～）
'faker_locale' => 'ja_JP',    // config/app.php の場合
```
