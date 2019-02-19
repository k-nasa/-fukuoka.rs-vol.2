## Rustでウェブアプリケーションを書いた話

---

## About me
name: nasa (近藤アサン)

所属: 福岡工業大学, m0neyf0rward

就活が憂鬱である

カレーマンと呼ばれ始めた

Ruby, CoffeeScript(React) coder

---
## ISUCONとは

- I   いい感じ
- SU  スピードアップ
- CON コンテスト

ウェブアプリケーションのチューニングのスコアを競う大会

*賞金100万円!!*

---

## 何を作ったか

はてなキーワードみたいなやつ

ISUCON6回予選のアプリケーションをRustで書き直した

Rustで爆速化計画(c++で書いた人たちに触発された)

---

![はてなキーワード](/assets/keyword.png)

---


## Webアプリケーションフレームワーク

今回は完全趣味で決めた

- Rocket 今回はこれを採用、使ったことがあったので
- actix-web 本当はこれを使いたかった、時間の関係でボツ

WAFベンチマーク７位だった（はやい）

---

## Mysql

- diesel を使うつもりだったけど、多言語の実装見てもORMは使ってないみたい
- mysql(rust-mysql-simple)というやつを使用

```rust
let pool = pool::MyPool::new(opts).unwrap();
let mut stmt = pool.prepare("select * from test;").unwrap();
let result = stmt.execute(&[]).unwrap();
for row in result {
    match row {
        Ok(v) => {
            let name = from_value::<Vec<u8>>(&v[1]);
            let name = String::from_utf8(name).unwrap();
            println!("id: {}, name: {}",  from_value::<i32>(&v[0]), name );
        },
        Err(e) => println!("error! => {:?}", e)
    }
}
```

---
## ごりごり実装していざ計測！

---
## パフォーマンス

![タイムアウト](/assets/first_time_out.png)

---

ファッ！
![odoroki](/assets/odoroki.jpg)

---

release buildしてなかった

てへぺろ！

![tehepero](/assets/a0253145_18322987.jpeg)

---
## 気を取り直していざ！

---
## パフォーマンス

![タイムアウト](/assets/first_time_out.png)

---

## 結果

- 何も考えずに早い言語で書いたからと言って早くならないよね〜

- ボトルネック解消したらまた違う結果になるかも？
