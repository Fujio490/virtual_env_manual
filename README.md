# virtual_env_manual
## 目次
- vagrant

## バージョン一覧
|  | version |
| :-- | :-- |
| OS | centOS7 |
| DB | MySQL5.7 |
| WEBserver | Nginx |
| PHP | 7.3 |
| Laravel | 6.0 |

## 構築手順
- vagrant
### boxをダウンロードする
```shell
$ vagrant box add centos/7

1) hyperv
2) libvirt
3) virtualbox
4) vmware_desktop

Enter your choice: 3
```
VirtualBoxのため3を入力して実行  
成功時、以下のような出力が返る
```
Successfully added box 'centos/7' (v1902.01) for 'virtualbox'!
```

### Vagrantのディレクトリ作成・移動
```shell
$ mkdir virtual_test
$ cd virtual_test
```

### ダウンロードしたboxを使用
```shell
$ vagrant init centos/7
```
成功時、以下のような表示が返る
```
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```

### Vagrantfileの編集
エディタから virtual_test/Vagrantfile を編集
```
# config.vm.network "forwarded_port", guest: 80, host: 8080

↓以下に編集

config.vm.network "forwarded_port", guest: 80, host: 2020
--------------------------
# config.vm.network "private_network", ip: "192.168.33.10"

↓以下に編集

config.vm.network "private_network", ip: "192.168.33.10"
--------------------------
config.vm.synced_folder "../data", "/vagrant_data"

↓以下に編集

config.vm.synced_folder "./", "/vagrant", type:"virtualbox"
```

### Vagrantプラグインインストール
```
$ vagrant plugin install vagrant-vbguest
```
プラグインのインストールを確認  
```
$ vagrant plugin list
```
成功時、以下のような表示が返る
```
vagrant-vbguest (0.30.0, global)
```

### ゲストOSを起動
```
$ vagrant up
```
以下のようにマウントでいていないと表示される
```
umount: /mnt: not mounted
```
ゲストOSに接続後、この対処を行う

### ゲストOSに接続
```
$ vagrant ssh
```

### カーネルのアップデート
```shell
$ sudo yum -y update kernel
```

### ゲストOSから切断
```
$ exit
```

### Vagrantをプロビジョニングし再起動
```
$ vagrant reload --provision
```

### ゲストOSに再接続
```
$ vagrant ssh
```

### グループパッケージをインストール
```
$ sudo yum -y groupinstall "development tools"
```

### PHPのインストール
```
$ sudo yum -y install epel-release wget
$ sudo wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
$ sudo rpm -Uvh remi-release-7.rpm
$ sudo yum -y install --enablerepo=remi-php72 php php-pdo php-mysqlnd php-mbstring php-xml php-fpm php-common php-devel php-mysql unzip
$ php v
```

## 所感

## 参考サイト
- [きれいな手順書を書く](https://nishinatoshiharu.com/tidy-procedure/)
- [# Vagrant + VirtualBOx で 最新のCentOS7 vbox(centos/7 2004.01)でマウントできない問題 - Qiita](https://qiita.com/mao172/items/f1af5bedd0e9536169ae)