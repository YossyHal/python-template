# Python環境構築手順(Pyenv+Poetry)

## Pyenvの環境構築

<https://github.com/pyenv/pyenv> をよく読んで、漏れが無いように実施すること  
日本語で書かれたブログ等もあるが、インストール手順が変わっていたり、一部の手順が抜けてしまっていることもよくあるので、必ず公式ドキュメントを参照する

### 【参考】EC2(Ubuntu)でPyenvをインストールした際の実行例

```sh
# 1. pyenvでPythonをインストールするのに必要なライブラリをインストール
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# 2. Pyenvをダウンロード
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# 3. ~/.profile と ~/.bashrc に追記をしてPATHを通す
sed -Ei -e '/^([^#]|$)/ {a \
export PYENV_ROOT="$HOME/.pyenv"
a \
export PATH="$PYENV_ROOT/bin:$PATH"
a \
' -e ':a' -e '$!{n;ba};}' ~/.profile
echo 'eval "$(pyenv init --path)"' >>~/.profile

# 4. 再起動(~/.profileは起動時に読み込まれるため、再起動しないとpyenvコマンドが通らない)
# コマンドから再起動してもIPアドレスは変わらないので大丈夫
sudo reboot
```

## Poetryの環境構築

<https://python-poetry.org/docs> をよく読んで、漏れが無いように実施すること

## よく使うコマンド

### Pyenv

<https://github.com/pyenv/pyenv/blob/master/COMMANDS.md>

```sh
# コマンド一覧を確認
pyenv

# インストール可能なPythonバージョンの一覧を確認する
pyenv install --list

# Pythonをインストールする
pyenv install 3.10.4

# 新しいPythonをインストールした後に必ず実施する
pyenv rehash

# ローカルのPythonバージョンを指定する
pyenv local 3.10.4
```

### Poetry

<https://python-poetry.org/docs/cli/>

```sh
# コマンド一覧を確認
poetry

# プロジェクト直下で仮想環境(.venv)を作成するようにPoetryの設定を変更
# これを実施しておかないとVSCodeが仮想環境を認識しない
poetry config virtualenvs.in-project true

# poetryで初期化し、pyproject.tomlを作成(既に存在する場合は実施する必要無し)
poetry init

# pyproject.tomlを読み取ってインストール(直下に仮想環境を作成)
poetry install

# ライブラリを追加
poetry add hoge

# 開発用ライブラリを追加
poetry add -D huga

# ライブラリを削除
poetry remove piyo

# poetryの仮想環境を有効化
poetry shell

# poetryの仮想環境から抜ける
exit
```

#### (参考)本リポジトリのpyproject.tomlを生成するのに使用したコマンド

```sh
poetry config virtualenvs.in-project true
poetry init -n
poetry shell

# pipを最新化
python -m pip install --upgrade pip

# 動作させるのに必要なライブラリ
# poetry add hoge

# 開発用ライブラリ
poetry add -D black
poetry add -D flake8
poetry add -D mypy
```
