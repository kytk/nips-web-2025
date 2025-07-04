# macOS native 環境での 準備方法

- 本セクションでは、Lin4Neuro を使わずに、macOS native 環境でのセットアップ方法を記載します。Apple Silicon でも対応可能です。ただし、この方法でのセットアップのサポートは限られることをご了承ください(個人個人で環境がかなり異なるためです)。

## 前提条件

- CPUは Intel でも Apple Silicon でも問いません
- ターミナルはデフォルトの zsh を使用することとします

## インストールが必要なソフトウェア
- git
- MRIcroGL
- XQuartz
- FSL
- MRtrix3
- ANTs
- Matlab
- SPM25


### git

#### インストール
- Command line tools for Xcode のインストールにより git を使うことが可能となります

```
xcode-select --install
```

#### 確認
- ターミナルから以下をタイプしていただき、バージョンが出力されれば大丈夫です

```
git --version
```


### MRIcroGL

#### インストール
- MRIcroGL は以下のリンクからインストーラーを入手できます
- https://github.com/rordenlab/MRIcroGL/releases/download/v1.2.20220720/MRIcroGL_macOS.dmg
- インストール後、以下のコマンドを実行し、.zprofileに設定を書き込みます。bashの方は.bash_profileに置き換えてください

```
echo '' >> ~/.zprofile
echo '#MRIcroGL' >> ~/.zprofile
echo 'PATH=$PATH:/Applications/MRIcroGL.app/Contents/Resources' >> ~/.zprofile
```

#### 確認
- 一度ターミナルを終了し、ターミナルを再度起動した後に、以下をタイプしてください

```
dcm2niix --version
```

この結果が、v1.0.20220720 と表示されれば大丈夫です

### XQuartz
- XQuartz は FSL の実行のために必要です

#### インストール
- 以下からインストーラーを入手し、実行します
- https://github.com/XQuartz/XQuartz/releases/download/XQuartz-2.8.1/XQuartz-2.8.1.dmg

#### 確認
- FSLが実行されればXQuartzもきちんとインストールされるのでここでは確認しません

### FSL (SPMコースでは不要です)
#### 事前準備
- FSLはインストーラーがpython3に対応しましたが、そのためにxcode-selectをインストールする必要があります。上でgitのインストールをしている場合は既に入っています。まだの方は、gitのところをご確認ください

#### インストール
- 以下をターミナルから実行し、fslinstaller.pyを入手し、実行します

```
cd ~/Downloads
curl -O https://fsl.fmrib.ox.ac.uk/fsldownloads/fslinstaller.py
cd ~/Downloads
/usr/bin/python3 ./fslinstaller.py 
```

- インストール完了後、FSLの設定は .profile に記載されるのですが、zshではうまく読み込まれないことがあるので、.zprofileにも記載します

- ターミナルから以下を実行します

```
echo '' >> ~/.zprofile
echo '# FSL Setup' >> ~/.zprofile
echo 'FSLDIR=/usr/local/fsl' >> ~/.zprofile
echo 'PATH=${FSLDIR}/bin:${PATH}' >> ~/.zprofile
echo 'export FSLDIR PATH' >> ~/.zprofile
echo '. ${FSLDIR}/etc/fslconf/fsl.sh' >> ~/.zprofile
```

- これが終わったら一度ターミナルを終了し、再びターミナルを起動します

#### 確認
- ターミナルから以下をタイプします
```
fsl
```

これでFSLが立ち上がればOKです

### MRtrix3 (SPMコースでは不要です)
#### インストール
- ターミナルから以下を実行します

```
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/MRtrix3/macos-installer/master/install)"
```

#### 確認
- ターミナルから以下を実行します

```
mrview
```

- MRViewが起動すれば大丈夫です

### ANTs (SPMコースでは不要です)
#### インストール
- ターミナルから以下を実行します

#### ANTsのインストール
- ターミナルに以下をコピペします

- intel macの場合

    ```
    cd ~/Downloads
    curl -O https://www.nemotos.net/l4n-abis/macOS_2025/ANTS_maci64.zip
    unzip ANTS_maci64.zip -d ~/
    grep '$HOME/ANTS/install/bin' ~/.zprofile > /dev/null
    if [ $? -eq 1 ]; then
      echo "" >> ~/.zprofile
      echo "#ANTs" >> ~/.zprofile
      echo 'export ANTSPATH=$HOME/ANTS/install/bin' >> ~/.zprofile
      echo 'export PATH=$PATH:$ANTSPATH' >> ~/.zprofile
    fi

    ```
- Apple Siliconの場合

    ```
    cd ~/Downloads
    curl -O https://www.nemotos.net/l4n-abis/macOS_2025/ANTS_arm64.zip
    unzip ANTS_arm64.zip -d ~/
    grep '$HOME/ANTS/install/bin' ~/.zprofile > /dev/null
    if [ $? -eq 1 ]; then
      echo "" >> ~/.zprofile
      echo "#ANTs" >> ~/.zprofile
      echo 'export ANTSPATH=$HOME/ANTS/install/bin' >> ~/.zprofile
      echo 'export PATH=$PATH:$ANTSPATH' >> ~/.zprofile
    fi

    ```




#### 確認
- "ANTs is installed" "Please close and re-run the terminal to reflect PATH setting" と出たら、ターミナルを一度閉じて、再度ターミナルを開きます

- ターミナルから以下を実行します

```
ANTS
```

- "call ANTS or ANTS --help" と出れば大丈夫です


### SPM25: Matlab をお持ちの場合
- SPM12 はMatlabを持っているか持っていないかでインストールの方法が変わります。Matlab をお持ちでない方は、次の "SPM25: Matlab をお持ちでない場合" に従ってセットアップをしてください

#### SPM25のインストール
- SPM12と共存させるために以下のようにします。

    ```
    cd #ホームディレクトリに移動します
    curl -L -O https://github.com/spm/spm/releases/download/25.01.02/spm_25.01.02.zip
    unzip spm_25.01.02.zip -d spm25

    ```

- この後、Matlabのパス設定で、/Users/ご自分のユーザ名/spm25/spm を指定してください

#### SPM25の確認
- Matlab から

    ```
    spm
    ```

とタイプし、SPMが起動すればOKです


### SPM25: Matlab をお持ちでない場合

#### ファイルの入手
- 以下でファイルを入手します。


    ```
    # Apple silicon
    cd /Applications #アプリケーションフォルダにインストールする場合
    curl -L -O https://github.com/spm/spm/releases/download/25.01.02/spm_standalone_25.01.02_macOS_Apple_Silicon.zip
    unzip spm_standalone_25.01.02_macOS_Apple_Silicon.zip
    ```

    ```
    # Intel mac
    cd /Applications #アプリケーションフォルダにインストールする場合
    curl -L -O https://github.com/spm/spm/releases/download/25.01.02/spm_standalone_25.01.02_macOS_Intel.zip
    unzip spm_standalone_25.01.02_macOS_Intel.zip
    ```

#### runtime のインストール (Apple silicon, Intel mac共通)

- ターミナルから以下のようにタイプします

    ```
    open /Applications/runtime_installer
    ```

- そうすると、Runtime_R2024b_for_spm_standalone_25.01.02 のアイコンが出てきます。こちらをダブルクリックしてインストールしてください。パスはデフォルトのまま変えないでください

#### ターミナルから起動するためのセットアップ

- 以下をターミナルにコピペします

    ```
    echo "" >> ~/.zprofile
    echo "# Alias for SPM25" >> ~/.zprofile
    echo "alias spm25='/Applications/spm_standalone/run_spm25.sh /Applications/MATLAB/MATLAB_Runtime/R2024b'" >> ~/.zprofile

    ```

- 一度ターミナルを閉じます。

### SPM25 standalone の確認

- GUIとターミナルのどちらからも起動できますが、ターミナルからの実行をおすすめします。ターミナルから起動すると、本来Matlabに出力される内容がすべてターミナルに出力されますが、GUIからの起動だとそれらを見ることができないからです。

- コマンドラインの場合は、ターミナルを起動した後、spm25 と入力すればSPMが起動します。ただ、初回は起動するまでに数分時間がかかるため、焦らずにお待ちください

    ```
    spm25
    ```

とタイプし、SPMが起動すればOKです。

- GUI の場合は、アプリケーションの中にある spm_standalone の中の spm25 をダブルクリックしてください。





