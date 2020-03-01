# リモートデスクトップのセットアップ

xrdpを用いて計算機にリモートデスクトップ接続する方法を記載．

## 1　xrdpのインストール

端末を起動し，以下のコマンドを実行．

```bash
sudo apt install xrdp lxde   # xrdpのインストール
```

## 2　xrdpの設定

1. 以下のコマンドを実行．

    ```bash
    $ echo lxsession -s LXDE -e LXDE > ~/.xsession   # よく分からん
    ```

2. 以下のコマンドを実行してファイルを編集する．

    ```bash
    $ sudo vi /etc/xrdp/xrdp.ini   # 該当ファイルを開く
    ```

    - `crypt_level`を`low`から`high`にする
    - xrdp1セクションの`port=-1`を`ask=-1`にする

    端末でのテキストの編集方法は[こちら](https://language-and-engineering.hatenablog.jp/entry/20121207/p1)
    を参照のこと．

3. 以下のコマンドを実行し，xrdpを起動．

    ```bash
    $ gsettings set org.gnome.Vino require-encryption false   # よく分からん
    $ sudo service xrdp restart   # xrdpを起動
    ```

## 3　xrdpのキー配列日本語化

以下のコマンドを順番に実行．

```bash
$ cd /etc/xrdp   # xrdpのディレクトリに移動
$ cd /tmp; wget http://w.vmeta.jp/temp/km-0411.ini   # 日本語化に必要なやつをダウンロード
$ sudo cp /tmp/km-0411.ini .   # よく分からん
$ sudo ln -s km-0411.ini km-e0200411.ini   # よく分からん
$ sudo ln -s km-0411.ini km-e0010411.ini   # よく分からん
```
