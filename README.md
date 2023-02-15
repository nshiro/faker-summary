# よく使う Faker のまとめです

詳しくは、旧本家サイトにて。　[旧本家サイト](https://github.com/fzaninotto/Faker)　[新本家サイト](https://github.com/fakerphp/faker)

Laravel 9.18 からは、fake() ヘルパー関数も使えます。例： fake()->name();

# 住所系

```php
$this->faker->postcode1();       	// 834（郵便番号3桁）
$this->faker->postcode2();       	// 8290（郵便番号4桁）
$this->faker->postcode();       	// 8348290（郵便番号7桁）
$this->faker->numerify('###-####');	// 834-8290（郵便番号3桁-4桁）
$this->faker->prefecture();     	// 東京都
$this->faker->ward();            	// 南区
$this->faker->city();            	// 鈴木市
$this->faker->streetAddress();   	// 斉藤町若松8-6-4
$this->faker->secondaryAddress();   	// ハイツ中村108号

$this->faker->areaNumber();		// 1～10 の間の値
$this->faker->buildingNumber();    	// 101～110 の間の値
```

# 個人情報（名前、メルアド、電話）

```php
$this->faker->name();			// 山田 太郎
$this->faker->firstName();		// 太郎
$this->faker->lastName();		// 山田
$this->faker->lastKanaName();		// ヤマダ
$this->faker->firstKanaName();		// タロウ or ハナコ（引数に'male', 'female' で性別指定可）
$this->faker->firstKanaNameMale();	// タロウ
$this->faker->firstKanaNameFemale();	// ハナコ

$this->faker->unique()->safeEmail();     // nakamura.ryohei@example.com（重複の無いメルアドで、実在しないドメイン）
$this->faker->phoneNumber(); 		// 0135-67-7343
```

# 文字列

```php
$this->faker->realText(10);    	// 日本語対応あり。最小 10～
    
$this->faker->sentence(8);            // タイトルなどに（英語）
$this->faker->paragraph(40);          // 本文などに（英語）
$this->faker->paragraphs(5, true);    // 改行コード付きの本文などに（英語）

$this->faker->regexify('[a-zA-Z0-9]{10}');   // 正規表現を使ったランダムな文字列
\Str::random(10);                            // laravelのヘルパー関数（英数字のみ）。例：「TkO41KdieO」
    
// 日本語でも改行コードほしい時は、以下。
preg_replace("/。/", "。\n\n", $this->faker->realText(150));
```

# 数字

```php
$this->faker->numerify('##');		// 2桁の数字（07 など 0 からスタートもあり）
$this->faker->numberBetween(1, 10);   // 1～10 の間の数字
$this->faker->biasedNumberBetween(1, 10, ['\Faker\Provider\Biased', 'linearLow']);
// 'linearLow'で、低い数字の出る確率大。反対は、'linearHigh'
```

# 配列

```php
$this->faker->randomElement(['a', 'b', 'c', 'd']);       // a～dの中からランダムに1つ
$this->faker->randomElements(['a', 'b', 'c', 'd'], 2);   // a～dの中からランダムに2つ（重複無し）
```

# 日時

```php
$this->faker->date('Y-m-d');			// 2002-12-10
$this->faker->time('H:i:00');			// 23:52:00
$this->faker->dateTime('now')->format('Y-m-d H:i:s');	  // 1977-08-13 09:40:21
$this->faker->dateTimeBetween('-3days', '3days')->format('Y-m-d');	// 2019-12-25

// フォーマットの指定が無い時は、DateTime オブジェクトが返る
```

# その他

```php
$this->faker->colorName();  	// Gold, Fuchsia, AntiqueWhite 等
$this->faker->url();         	// https://www.yahaoo.co.jp/
$this->faker->latitude(35.54915506146918, 36.06591802134296);    // 緯度
$this->faker->longitude(138.96409298125002, 140.30442501250002); // 経度
```

# 修飾子

### unique()

```php
// 重複しないようにデータを返す
// 但し、それ以上重複しない値を返せない時は、エラーとなる
$this->faker->unique()->safeEmail();
$this->faker->unique()->randomElement([1, 2, 3]);  // 4 回呼び出してしまうとエラー
```

### optional()

```php
// null（デフォルト値）を時折混ぜたい時に便利。
$this->faker->optional(0.1)->randomElement([1, 2, 3]);  // 90%の確率でnullを返す
```

# おまけ

```php
// laravel で Faker の日本語設定（config/app.php で）
'faker_locale' => 'ja_JP',

```
