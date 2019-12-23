# よく使う Faker のまとめです

詳しくは、本家サイトにて。　[本家サイト](https://github.com/fzaninotto/Faker)　[本家サイト（日本語ファイル）](https://github.com/fzaninotto/Faker/tree/master/src/Faker/Provider/ja_JP)

# 住所系

```php
$faker->postcode1;       	// 834（郵便番号3桁）
$faker->postcode2;       	// 8290（郵便番号4桁）
$faker->postcode;       	// 8348290（郵便番号7桁）
$faker->prefecture;     	// 東京都
$faker->city;            	// 鈴木市
$faker->ward;            	// 南区
$faker->streetAddress;   	// 斉藤町若松8-6-4
$faker->secondaryAddress;   	// ハイツ中村108号

$faker->areaNumber;		// 1～10 の間の値
$faker->buildingNumber    	// 101～110 の間の値
```

# 個人情報（名前、メルアド、電話）

```php
$faker->name;			// 山田 太郎
$faker->firstName;		// 太郎
$faker->lastName;		// 山田
$faker->lastKanaName;		// ヤマダ
$faker->firstKanaName();	// タロウ or ハナコ（引数に'male', 'female' で性別指定可）
$faker->firstKanaNameMale;	// タロウ
$faker->firstKanaNameFemale;	// ハナコ

$faker->unique()->safeEmail     // nakamura.ryohei@example.com（重複の無いメルアド）
$faker->phoneNumber 		// 0135-67-7343
```

# 文字列

```php
$faker->realText(10)    	// 日本語対応あり。最小 10～
    
$faker->sentence(8)            // タイトルなどに（英語）
$faker->paragraph(40)          // 本文などに（英語）
$faker->paragraphs(5, true)    // 改行コード付きの本文などに（英語）
    
\Str::random(10)               // laravelのヘルパー関数（英数字のみ）。例：「TkO41KdieO」
    
// 日本語でも改行コードほしい時は、以下。
preg_replace("/。/", "。\n\n", $faker->realText(150));
```

# 数字

```php
$faker->numerify('##');		// 2桁の数字（07 など 0 からスタートもあり）
$faker->numberBetween(1, 10);   // 1～10 の間の数字
$faker->biasedNumberBetween(1, 10, ['\Faker\Provider\Biased', 'linearLow']);
// 'linearLow'で、低い数字の出る確率大。反対は、'linearHigh'
```

# 配列

```php
$faker->randomElement(['a', 'b', 'c', 'd']);       // a～dの中からランダムに1つ
$faker->randomElements(['a', 'b', 'c', 'd'], 2);   // a～dの中からランダムに2つ（重複無し）
```

# 日時

```php
$faker->date('Y-m-d');			// 2002-12-10
$faker->time('H:i:00');			// 23:52:00
$faker->dateTime('now')->format('Y-m-d H:i:s');	  // 1977-08-13 09:40:21
$faker->dateTimeBetween('-3days', '3days')->format('Y-m-d');	// 2019-12-25

// フォーマットの指定が無い時は、DateTime オブジェクトが返る
```

# その他

```php
$faker->colorName  	// Gold, Fuchsia, AntiqueWhite 等
$faker->url         	// https://www.kijima.jp/sed-id-rerum-quas
$faker->latitude(35.54915506146918, 36.06591802134296)    // 緯度
$faker->longitude(138.96409298125002, 140.30442501250002) // 経度
```

# 修飾子

### unique()

```php
// 重複しないようにデータを返す
// 但し、それ以上重複しない値を返せない時は、エラーとなる
$faker->unique()->safeEmail;
$faker->unique()->randomElement([1, 2, 3]);  // 4 回呼び出してしまうとエラー
```

### optional()

```php
// null（デフォルト値）を時折混ぜたい時に便利。
$faker->optional(0.1)->randomElement([1, 2, 3]);  // 10 回に 1 回の確率で、nullを返す
```

# おまけ

```php
// laravel で Faker の日本語設定（config/app.php で）
'faker_locale' => 'ja_JP',

// 走る度に同じランダムデータを生成させる
$factory->define(Xxxx::class, function (Faker $faker) {
    static $seed = 0;
    $faker->seed($seed++);
```
