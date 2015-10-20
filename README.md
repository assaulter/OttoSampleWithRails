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

[公式](https://hashicorp.com/blog/otto.html)
からバイナリ落としてきてPATHを通しておく

```
$ otto -v
Otto v0.1.1
```

# Otto compileする

```
$ otto compile
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

# Vagrant環境を作成する

```
$ otto dev
```

otto compileでvagrant用のファイルとか出来てるので、それが適用されたvagrant環境を作ります。
ローカルにVirtualBoxとVagrant入ってなくても勝手に入れてくれるっぽい。

そんなこんなで完成

```
==> Development environment successfully created!
    IP address: 172.16.1.253

    A development environment has been created for writing a generic
    Ruby-based app.

    Ruby is pre-installed. To work on your project, edit files locally on your
    own machine. The file changes will be synced to the development environment.

    When you're ready to build your project, run 'otto dev ssh' to enter
    the development environment. You'll be placed directly into the working
    directory where you can run 'bundle' and 'ruby' as you normally would.

    You can access any running web application using the IP above.
```

# rails起動アンドアクセス

```
$ bundle install
$ bundle exec rake db:migrate
$ bundle exec rails s -b 0.0.0.0
```
otto dev addressしたやつにアクセスすると繋がる。

超シンプルなRailsアプリとかならいけそうな気がした。
