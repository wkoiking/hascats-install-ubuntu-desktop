# Ubuntu Desktopの設定

## Networkの設定

下記の内容の`/etc/netplan/01-network-manager-all.yaml`を作成する

~~~
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eno1:
      dhcp4: no
      addresses: [172.21.102.22/16]
~~~

下記の内容の`/etc/netplan/02-network-manager-all.yaml`を作成する

~~~
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp1s0:
      addresses: [172.22.102.22/16]
~~~

下記のコマンドを実行する

~~~
$ sudo netplan apply
~~~

[Reference](https://vitux.com/how-to-configure-networking-with-netplan-on-ubuntu/)

## OpenSSH Serverの設定

インターネットに接続して下記のコマンドを実行する

~~~
$ sudo apt update
$ sudo apt install openssh-server
$ sudo ufw allow ssh
~~~

[Reference](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/)

## Window Managerの設定

デスクトップ環境を起動せずに、直接hascats-exeを起動するようにする：

下記の内容の`/usr/share/xsessions/custom.desktop`を作成する

~~~
[Desktop Entry]
Name=XSession
Comment=This session uses the custom xsession file
Exec=/etc/X11/Xsession
~~~

下記の内容の`~/.xsession`を作成する

~~~
#!/usr/bin/env bash

xsetroot -cursor_name left_ptr &

exec /home/mega/.local/bin/hascats-exe
~~~

[XSessionの設定](https://wiki.ubuntu.com/CustomXSession)

[マウスカーソルの設定](https://wiki.archlinux.jp/index.php/%E3%82%AB%E3%83%BC%E3%82%BD%E3%83%AB%E3%83%86%E3%83%BC%E3%83%9E#.E5.BD.A2.E3.81.8C_X_.E3.81.AE.E3.83.87.E3.83.95.E3.82.A9.E3.83.AB.E3.83.88.E3.82.AB.E3.83.BC.E3.82.BD.E3.83.AB.E3.81.AE.E5.A4.89.E6.9B.B4)

## NTPの設定

[https://www.yokoweb.net/2018/05/14/ubuntu-18_04-timesyncd/]

## インストーラー（.debファイル）の作成とインストール

`hascats.deb`を作る：

はじめにバイナリを`/hascats/home/mega/.local/bin/hascats-exe`を置く。e.g.,

~~~
cp /home/mega/.local/bin/hascats-exe /home/mega/hascats-install-ubuntu-desktop/hascats/home/mega/.local/bin/hascats-exe
~~~

hascatsディレクトリを`.deb`に固める。

~~~
$ cd /home/mega/hascats-install-ubuntu-desktop
$ fakeroot dpkg-deb --build hascats
~~~

※fakeroot がないと言われた場合は`sudo apt-get install fakeroot`

`hascats.deb`をインストールする：

~~~
$ sudo dpkg -i hascats.deb
~~~

[Reference](https://blog.cybozu.io/entry/2016/05/16/111500)
