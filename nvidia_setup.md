# Deep Learning用計算機のセットアップ

Ubuntu OS搭載の計算機にDeep Learning環境を構築する方法を記載．

## 目次

1. [NVIDIA Driverのセットアップ](#1nvidia-driverのセットアップ)

    1. [NVIDIA Driverのダウンロード](#11nvidia-driverのダウンロード)
    2. [NVIDIA Driverのインストール](#12nvidia-driverのインストール)

2. [CUDAのセットアップ](#2cudaのセットアップ)

    1. [CUDAのダウンロード](#21cudaのダウンロード)
    2. [CUDA Toolkitのインストール](#22cuda-toolkitのインストール)
    3. [pathの設定](#23pathの設定)

3. [cuDNNのセットアップ](#3cudnnのセットアップ)
    1. [cuDNNのダウンロード](#31cudnnのダウンロード)
    2. [cuDNNのインストール](#32cudnnのインストール)

## 1　NVIDIA Driverのセットアップ

### 1.1　NVIDIA Driverのダウンロード

[ドライバー - NVIDIAドライバダウンロード](https://www.nvidia.co.jp/Download/index.aspx?lang=jp)にアクセスし，計算機のGPUに合う最新のドライバを検索し，ダウンロードする．

- GeForce GTX 1080 Ti，GeForce RTX 2080 Ti共に最新バージョンは`430.50`（2019/09/28時点）．
- ダウンロードしたファイル名は`NVIDIA-Linux-x86_64-<version>.run`のようになっているはず．

### 1.2　NVIDIA Driverのインストール

1. 再起動し，**Ctrl + Alt + F1**で仮想コンソール（CUI）に切り替え，ログインする．

2. 端末から以下のコマンドを実行する．

    ```bash
    $ sudo apt install dkms   # dkmsのインストール
    $ sudo systemctl stop lightdm   # X Window Systemの停止
    $ pkill Xorg
    ```

3. 古いNVIDIA Driver，CUDAをアンインストール．

    ```bash
    $ sudo apt-get update
    $ sudo apt-get --purge remove nvidia*   # NVIDIA Driverのアンインストール
    $ sudo apt-get --purge remove cuda*     # CUDAのアンインストール
    ```

4. nvidiaドライバをインストール．

    ```bash
    $ cd ./Downloads   # Downloadディレクトリに移動
    $ sudo bash NVIDIA-Linux-x86_64-<version>.run --no-opengl-files --no-libglx-indirect –dkms   # nvidiaドライバのインストール
    ```

    何度か質問されるが，下記以外は基本的にデフォルトのまま進めてOK

    - `The distribution-provided pre-install script failed! Are you sure want to continue?`
        - `Continue Installation`を選択．
    - `Would you like to run the nvidia-xconfig utility to automatically update your X configuration file so that the NVIDIA X driver will be used when you restart X? Any pre-existing X configuration file will be backed up.`
        - Xが再起動したときにNVIDIA Xドライバが使用されるように，自動的にX設定ファイルを更新する？みたいな感じ
        - `Yes`を選択（`No`を選ぶとログインループになった）．

5. 再起動・ログイン後，端末で以下のコマンドを実行してGPUの情報が表示されれば完了．

    ```bash
    $ nvidia-smi   # GPUの情報表示
    ```

## 2　CUDAのセットアップ

- CUDAは`10.0`をインストールすることをおすすめします（2019/09/28時点）．

### 2.1　CUDAのダウンロード

1. [CUDA Toolkit Archive | NVIDIA DEVELOPER](https://developer.nvidia.com/cuda-toolkit-archive)にアクセス．

2. `CUDA Toolkit 10.0`にアクセス．

3. `Select Target Platform`にて次のように選択し，ダウンロードする．

    - Operating System：`Linux`
    - Architecture：`x86_64`
    - Distribution：`Ubuntu`
    - Version：`16.04`（最近の計算機は`18.04`かも，要確認）
    - Installer Type：`deb(local)`

### 2.2　CUDA Toolkitのインストール

[2.1　CUDAのダウンロード](#21-cuda%e3%81%ae%e3%83%80%e3%82%a6%e3%83%b3%e3%83%ad%e3%83%bc%e3%83%89)を行うと端末での操作手順が出てくるはず．その手順通りに端末でコマンドを実行する．

- :warning: 手順の最後`sudo apt-get install cuda`はそのまま実行しない
    - 所望のversionに加えて最新versionがインストールされたり，NVIDIA Driverが勝手にインストールされログインループになる可能性あり．
- **`sudo apt-get install cuda-toolkit-10-0`を実行し，CUDA Toolkitのみインストールする**．

### 2.3　pathの設定

1. 以下のコマンドを実行し，パスを設定する．

    ```bash
    $ echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
    $ echo 'export PATH=/usr/local/cuda-10.0/bin:${PATH}' >> ~/.bashrc
    $ echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
    $ source ~/.bashrc
    ```

2. 以下のコマンドを実行し，パスを確認する．

    ```bash
    $ echo $PATH   # 出力に"/usr/local/cuda-10.0/bin"が含まれているか？
    $ echo $LD_LIBRARY_PATH   # 出力に"/usr/local/cuda-10.0/lib64"が含まれているか？
    $ which nvcc   # 出力が"/usr/local/cuda-10.0/bin/nvcc"になっているか？
    $ nvidia-smi   # NVIDIAのGPUの情報が表示されているか？
    ```

## 3　cuDNNのセットアップ

- ここでは**cuDNN7.6.4**(2019/2/20時点でのcuDNN 7の最新版，CUDA 10.0対応)をインストールする．
- インストールしたCUDAのversionに合ったcnDNNの最新版をインストールすると良い．

### 3.1　cuDNNのダウンロード

1. [NVIDIA cuDNN | NVIDIA Developer](https://developer.nvidia.com/cudnn)にアクセスし，`Download cnDNN`をクリック．

2. `NVIDIA Developer Program`のアカウントでログイン．持ってない人はアカウントを作成する．

3. 再び[NVIDIA cuDNN | NVIDIA Developer](https://developer.nvidia.com/cudnn)にアクセスし，`Download cnDNN`をクリック．

4. 適当にアンケートに答え，`I Agree To the Terms of the cuDNN Software License Agreement`にチェックをつけると，ダウンロードリンクが表示される．

5. `Download cuDNN ~~~, for CUDA 10.0`をクリックし，以下の2つをダウンロードする．
    - `cuDNN Runtime Library for Ubuntu16.04 (Deb)`
    - `cuDNN Developer Library for Ubuntu16.04 (Deb)`

### 3.2　cuDNNのインストール

端末を起動し，以下のコマンドを順番に実行してインストールする．

```bash
$ sudo dpkg -i /home/user/Downloads/<cuDNN Runtime Library for Ubuntu16.04 (Deb)のファイル名>
$ sudo dpkg -i /home/user/Downloads/<cuDNN Developer Library for Ubuntu16.04 (Deb)のファイル名>
```
