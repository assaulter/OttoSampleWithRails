# これはなに？

* Ottoを使っての開発環境構築をやってみたいので、そのサンプルのプロジェクト
* 簡単に以下

```
$ be rails -v
Rails 4.2.4
$ ruby -v
ruby 2.2.0p0 (2014-12-25 revision 49005) [x86_64-darwin13]
```

userをscaffoldしdb:migrateしただけ。しかもsqlite3

```
$ be rails generate scaffold User name:string email:string
$ be rake db:migrate
```

こいつをOttoが勝手に判断して、vagrantfileを吐いてくれる。筈。
