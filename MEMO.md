# Ottoメモ

時間なさ気なので、まずは公式チュートリアルをやるよ。

## ざっくり感想

* どうやらVagrantの後継らしい
* Vagrant + Chef-soloで開発環境一発構築！みたいなのをやってくれそうな雰囲気
（後述するcompileコマンドでVagrant用の設定ファイルを吐きます）
* [Terraform](https://terraform.io)を使ったインフラ構築や、デプロイ用イメージに固める、デプロイ等もやってくれるらしい（ここ未検証っす）

* 今回は簡単に開発環境ができたらいいなぁということで、チュートリアルからやってみました。

## 事前準備
[公式](https://hashicorp.com/blog/otto.html)
からバイナリ落としてきてPATHを通しておく

```
$ otto -v
Otto v0.1.1
```

## Getting Startedをやる

[これ](https://ottoproject.io/intro/getting-started/dev.html)

公式のシンプルなRackアプリをcloneする。

```
$ git clone https://github.com/hashicorp/otto-getting-started.git
$ cd otto-getting-started
$ otto compile
```

### otto compile

```
For our example, otto compile detected our application is Ruby and used opinionated defaults for the rest. In a future step, we'll write an Appfile to more precisely configure Otto.
```
すげぇ！と言いたいけど、ちょっと複雑になるとアウト臭い。bundle install的なこともやってくれるわけではない。

（studyplus-webでやってみたらなぜかbundlerすら入らんという。なぜ...？

シンプルなRailsとかLaravelとかExpress程度ならイケるっぽい。

気を取り直して、実行すると.ottoフォルダができる

```
[otto-getting-started] tree .otto
.otto
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
│   │       │   └── main.sh
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
│   └── infra-otto-getting-started
│       ├── main.tf
│       └── outputs.tf
└── data
    ├── dev_ip
    └── vagrant
        └── machines
            └── default
                └── virtualbox
                    ├── action_provision
                    ├── action_set_name
                    ├── creator_uid
                    ├── id
                    ├── index_uuid
                    ├── private_key
                    └── synced_folders
```

.tfはTerraFormの設定ファイル？
今回見るのはVagrantfileだけ


## Vagrantfile

```
$script_ruby = <<SCRIPT
set -o nounset -o errexit -o pipefail -o errtrace

error() {
   local sourcefile=$1
   local lineno=$2
   echo "ERROR at ${sourcefile}:${lineno}; Last logs:"
   grep otto /var/log/syslog | tail -n 20
}
trap 'error "${BASH_SOURCE}" "${LINENO}"' ERR

# otto-exec: execute command with output logged but not displayed
oe() { $@ 2>&1 | logger -t otto > /dev/null; }

# otto-log: output a prefixed message
ol() { echo "[otto] $@"; }

# Make it so that `vagrant ssh` goes directly to the correct dir
echo "cd /vagrant" >> /home/vagrant/.bashrc

# Configuring SSH for faster login
if ! grep "UseDNS no" /etc/ssh/sshd_config >/dev/null; then
  echo "UseDNS no" | sudo tee -a /etc/ssh/sshd_config >/dev/null
  oe sudo service ssh restart
fi

export DEBIAN_FRONTEND=noninteractive

ol "Adding apt repositories and updating..."
oe sudo apt-get update
oe sudo apt-get install -y python-software-properties software-properties-common apt-transport-https
oe sudo add-apt-repository -y ppa:chris-lea/node.js
oe sudo apt-add-repository -y ppa:brightbox/ruby-ng
oe sudo apt-get update

# TODO: parameterize ruby version as input
export RUBY_VERSION="2.2"

ol "Installing Ruby ${RUBY_VERSION} and supporting packages..."
export DEBIAN_FRONTEND=noninteractive
oe sudo apt-get install -y bzr git mercurial build-essential \
  libpq-dev zlib1g-dev software-properties-common \
  libsqlite3-dev \
  nodejs \
  ruby$RUBY_VERSION ruby$RUBY_VERSION-dev

ol "Installing Bundler..."
oe gem install bundler --no-ri --no-rdoc

ol "Configuring Git to use SSH instead of HTTP so we can agent-forward private repo auth..."
oe git config --global url."git@github.com:".insteadOf "https://github.com/"
SCRIPT
```

## Vagrant起動その他

```
$ otto dev
```

VirtualBox ← うろ覚え
Vagrant
BoxImage

あたりは自動で入った

```
$ otto dev ssh
```

で仮想環境にログイン

```
vagrant@precise64:/vagrant$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=12.04
DISTRIB_CODENAME=precise
DISTRIB_DESCRIPTION="Ubuntu 12.04 LTS"
```

## ちょいカスタマイズ

Appfileをプロジェクト直下に置くとそっちを見てくれる。

```
[OttoSampleWithRails] cat Appfile
customization "ruby" {
  ruby_version = "2.1"
}
```

otto compile → otto dev でいけるかと思ったけど、一旦otto dev vagrant destroy しないとインストールが走らなかった。

ちなみにotto dev vagrant "以降vagrant向けコマンド" でvagrantコマンドが使える。
