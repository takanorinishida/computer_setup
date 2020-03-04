# Python仮想環境構築

Minicondaを用いてPython仮想環境を構築する方法を記載．

## 目次

1. [Minicondaのインストール](#1minicondaのインストール)

2. [Minicondaによる仮想環境の構築](#2minicondaによる仮想環境の構築)

    1. [仮想環境の作成](#21仮想環境の作成)
    2. [ライブラリのインストール](#22ライブラリのインストール)

3. [Minicondaのコマンド一覧](#3minicondaのコマンド一覧)

    1. [仮想環境の編集系](#31仮想環境の編集系)
    2. [ライブラリの編集系](#32ライブラリの編集系)
    3. [仮想環境のファイル出力](#33仮想環境のファイル出力)

## 1　Minicondaのインストール

1. [Miniconda Linux 64-bit (bash installer)](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)
をダウンロード．

2. 端末を起動し，以下のコマンドを実行してインストール．

    ```bash
    $ bash /home/user/Downloads/Miniconda3-latest-Linux-x86_64.sh   # Minicondaのインストール
    ```

3. `License agreement`を読む．

4. `Do you accept the license terms?`と聞かれるので`yes`と打つ

5. Minicondaのインストール先を聞かれるので次を保存先に指定して実行．
    - `/home/user/libraries/Miniconda3`

6. PATHを追加するか？→`yes`

7. 端末を再起動させて，`conda –V` でバージョンが出ればOK．

## 2　Minicondaによる仮想環境の構築

### 2.1　仮想環境の作成

端末を起動し，以下のコマンドを実行して仮想環境を作成する．

```bash
$ conda create –n <仮想環境名> python=<好きなversion>
```

### 2.2　ライブラリのインストール

1. 端末で次のコマンドを実行し，仮想環境に入る．

    ```bash
    $ conda activate <仮想環境名>   # 仮想環境に入る
    $ source activate <仮想環境名>   # condaが使えないとき
    ```

2. 次のコマンドを実行し，ライブラリをインストールする．

    ```bash
    $ conda install <ライブラリ名>   # ライブラリのインストール
    $ pip install <ライブラリ名>   # condaが使えないとき
    ```

## 3　Minicondaのコマンド一覧

### 3.1　仮想環境の編集系

```bash
$ conda create –n <仮想環境名> python=<バージョン(3.5や3.6など)>   # 仮想環境の作成
$ conda remove –n <仮想環境名> –all   # 仮想環境の削除
$ conda info –e   # 仮想環境の一覧表示
$ source activate [仮想環境名]   # 仮想環境に入る
$ source deactivate   # 仮想環境から出る
```

### 3.2　ライブラリの編集系

```bash
$ conda install <ライブラリ名>   # ライブラリのインストール
$ conda install <ライブラリ名>=<バージョン>   # ライブラリのインストール(バージョン指定)
$ pip install <ライブラリ名>   # ライブラリのインストール(condaが使えないとき)
$ conda uninstall <ライブラリ名>   # ライブラリのアンインストール
$ conda update <ライブラリ名>   # ライブラリのアップデート
$ conda list   # 仮想環境内のライブラリ一覧の表示
```

### 3.3　仮想環境のファイル出力

Minicondaの仮想環境は`.yaml`ファイルに描き出すことができる．

```bash
$ conda activate <仮想環境名>                     # 仮想環境に入る
$ conda env export > <出力先のpath>               # 仮想環境の書き出し
$ conda env create --file <.yamlファイルのpath>   # .yamlファイルから仮想環境を作成
```
