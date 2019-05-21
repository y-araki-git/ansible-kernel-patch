# Ansibleでカーネルパッチ適用

カーネルパッチ適用をAnsibleで自動化できたらと思い、
自分なりに作成してみました。

## 対応OSとバージョン

- CentOS 7 Debian 8

## パッチ適用について

roles/patch/tasks/patch.ymlがパッチ適用の処理です。

CentOSの場合:update kernel for centosのtaskにアップデートしたいカーネル記載。
Debianの場合:あてたいパッチをこのansibleファイルの最上位フォルダに配置。

## inventory ファイル

patchにはpatch適用するIPもしくはhostnameを記述

kernel_checkにはkernelを確認したいIPもしくはhostnameを記述

    [patch]
    xxx.xxx.xxx.xxx
    xxx.xxx.xxx.xxx
    [kernel_check]
    xxx.xxx.xxx.xxx
    [all:vars]
    stage=stg


## 実行例

→ ansibleコマンド実行前にパスフレーズをキャッシュさせます

    # eval `ssh-agent`
    # ssh-add ../.ssh/ansible_private_key

→ patch適用ansible

    # ansible-playbook -i prd patch.yml -u ユーザ名

→ kernel確認用ansible(log/result.txtに結果出力)

patch適用ansibleを流す前にこのansibleを流しpatch適用前情報としてディレクトリをコピーをし、

patch適用後にもう一度本ansibleを流し二つのディレクトリの内容の差分を比較する

    # ansible-playbook -i prd kernel_check.yml -u ユーザ名

