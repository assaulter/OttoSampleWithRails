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

# Otto導入

公式からバイナリ落としてきてPATHを通しておく

```
$ otto -v
Otto v0.1.1
```

# Otto compileする

```
$ otto compilen
```

するとこんなのが生成される

```
~/Developer/ruby/OttoSample/ottoSample/.otto[assaulter]$ tree
.
├── appfile
│   ├── Appfile.compiled
│   └── version
├── compiled
│   ├── app
│   │   ├── build
│   │   │   ├── build-ruby.sh
│   │   │   └── template.json
│   │   ├── deploy
│   │   │   └── main.tf
│   │   ├── dev
│   │   │   └── Vagrantfile
│   │   └── foundation-consul
│   │       ├── app-build
│   │       │   ├── main.sh
│   │       │   └── upstart.conf
│   │       ├── app-deploy
│   │       │   └── main.sh
│   │       ├── app-dev
│   │       │   ├── main.sh
│   │       │   └── upstart.conf
│   │       ├── app-dev-dep
│   │       │   └── main.sh
│   │       └── deploy
│   │           ├── main.tf
│   │           ├── module-aws
│   │           │   ├── join.sh
│   │           │   ├── main.tf
│   │           │   ├── outputs.tf
│   │           │   └── variables.tf
│   │           ├── module-aws-simple
│   │           │   ├── main.tf
│   │           │   ├── outputs.tf
│   │           │   ├── setup.sh
│   │           │   └── variables.tf
│   │           └── variables.tf
│   ├── foundation-consul
│   │   ├── app-build
│   │   │   ├── main.sh
│   │   │   └── upstart.conf
│   │   ├── app-deploy
│   │   │   └── main.sh
│   │   ├── app-dev
│   │   │   ├── main.sh
│   │   │   └── upstart.conf
│   │   ├── app-dev-dep
│   │   │   └── main.sh
│   │   └── deploy
│   │       ├── main.tf
│   │       ├── module-aws
│   │       │   ├── join.sh
│   │       │   ├── main.tf
│   │       │   ├── outputs.tf
│   │       │   └── variables.tf
│   │       ├── module-aws-simple
│   │       │   ├── main.tf
│   │       │   ├── outputs.tf
│   │       │   ├── setup.sh
│   │       │   └── variables.tf
│   │       └── variables.tf
│   └── infra-ottoSample
│       ├── main.tf
│       └── outputs.tf
└── data
    └── dev_ip
```
