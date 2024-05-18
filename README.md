# Symbol Node Updater

Easily update Symbol nodes.

## Overview

Symbolノードの更新を簡単に行えるツールです。以下の機能に対応しています。

- symbol-bootstrap の最新バージョンへの更新
- `preset.yml` (プリセットファイル) の更新適用
- 複数ノードの一括更新

--

This is a tool that makes it easy to update Symbol nodes. The following functions are supported.

- Update symbol-bootstrap to the latest version.
- Apply updates of `preset.yml` (preset file).
- Batch update of multiple nodes.

## Attention

- このツールはSymbolノードの更新のみをサポートしています。Symbolノードの構築自動化ツールについては、[Symbol Node Builder](https://github.com/koutyan/Symbol-Node-Builder) を参照ください。

- このツールのサポートOSは **Ubuntu** です。作者は *Ubuntu 20.04* でのテストを実施しています。**CentOS系では動作しません**。(ご要望があれば今後対応するかもしれません)

- 本ツールの使用は自己責任とします。本ツールを使用したことによる損害などの一切の責任を作者は負いません。

--

- This tool only supports updating Symbol nodes; for an automated tool to build Symbol nodes, see [Symbol Node Builder](https://github.com/koutyan/Symbol-Node-Builder).

- The supported OS for this tool is **Ubuntu**. The author has tested it on *Ubuntu 20.04*. **It does not work on CentOS systems**. (We may support this in the future if requested).

- Use of this tool is at your own risk. The author assumes no responsibility for any damage caused by the use of this tool.

## How to use

以下の操作は Ubuntu 上で実施してください。

1： ツールをダウンロードして解凍します。

  ```(text)
  $ wget https://github.com/koutyan/Symbol-Node-Updater/archive/refs/tags/v1.0.2.tar.gz
  $ tar xzvf v1.0.2.tar.gz && rm v1.0.2.tar.gz
  $ cd Symbol-Node-Updater-1.0.2/
  ```

2： **/confディレクトリ内**のファイルを編集して、自分に合った設定にします。

- **inventory**：実行対象ノードのホスト名またはIPアドレスを指定します。基本的にはデフォルトの`localhost`で問題ないです。もし、現在操作しているサーバーとは他のサーバーを更新したい場合はそのサーバーのIPアドレスに置き換えてください。また、複数ノードを一括更新したい場合は、このファイルに対象サーバーのIPアドレスを改行して記載してください。

- **vars.yml**： 以下の値を適切に設定してください。
  - `username`: 更新するノードのユーザー名
  - `symbol_path`: symbol-bootstrapが格納されているパス名 (`/home/<ユーザー名>/`のあとに続くパス。`/home/ubuntu/symbol-bootstrap` に格納されていたら下の例のように`symbol-bootstrap`と記載する)
  - `preset_name`: プリセットファイル名を指定する
  - `symbol_password`: Symbolノードの設定ファイル暗号化パスワードを指定する
  - `https_portal`: HTTPS化にhttps-portalを使用している場合はtrue、使用していない場合はfalseに設定する
  - `https_portal_path`: (https-portal使用者のみ設定する) https-portalが格納されているパス名 (`/home/<ユーザー名>/`のあとに続くパス。`/home/ubuntu/https-portal` に格納されていたら下の例のように`https-portal`と記載する)

    ```(text)
    ---
    username: ubuntu
    symbol_path: symbol-bootstrap
    preset_name: preset.yml
    symbol_password: hogehoge
    https_portal: true
    https_portal_path: https-portal
    ```

3： 以下のコマンドを実行します。パスワードを聞かれますので入力します。更新前の準備が行われます。"Setup done" と表示されたら完了です。

```(text)
$ chmod +x setup.sh
$ ./setup.sh
```

4： 最後に以下のコマンドを実行します。2回パスワードを求められるので、入力してください。Symbolノードの更新が実施されます。`WARNING`などの表示は気にしないでください。

```(text)
$ ansible-playbook playbook.yml -v
```

5： しばらく待って、実施結果が表示され、`unreachable=0` `failed=0` となっていれば成功です。symbol-bootstrapが格納されているディレクトリに移動して、`symbol-bootstrap -v` を実行して、ノードが更新されていることを確認してください。以上で完了です。

```(text)
PLAY RECAP **************************************************************************************************************************
localhost                  : ok=13   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ cd ~/symbol-bootstrap
$ symbol-bootstrap -v
symbol-bootstrap/1.0.5 linux-x64 node-v14.16.1
```

**今後Symbolノードの更新を実施する場合は、手順4のコマンドを実行するだけで実施できます。**

--

The following operations should be performed on Ubuntu.

1： Download and unzip the tool.

  ```(text)
  $ wget https://github.com/koutyan/Symbol-Node-Updater/archive/refs/tags/v1.0.2.tar.gz
  $ tar xzvf v1.0.2.tar.gz && rm v1.0.2.tar.gz
  $ cd Symbol-Node-Updater-1.0.2/
  ```

2： Edit the files **in the /conf directory** to make the settings suitable for you.

- **inventory**: Specify the hostname or IP address of the node to be executed. Basically, the default `localhost` is fine. If you want to update different servers than the one you are currently operating, replace it with the IP address of that server. If you want to update multiple nodes at once, please enter the IP address of the target server in this file with a new line.

- **vars.yml**: Please set the following values appropriately.
  - `username`: The user name of the node to be updated.
  - `symbol_path`: path name where symbol-bootstrap is stored (`/home/<username>/` followed by the path. If it is stored in `/home/ubuntu/symbol-bootstrap`, it should be written as `symbol-bootstrap`, as in the example below)
  - `preset_name`: Specify the name of your preset file.
  - `symbol_password`: Specify the encryption password for the Symbol node configuration file.
  - `https_portal`: Set to true if https-portal is used for HTTPS, or false if not used.
  - `https_portal_path`: (set only if you use https-portal) path name where https-portal is stored (the path after `/home/<username>/`, e.g.. If it is stored in `/home/ubuntu/https-portal`, it should be written as `https-portal` as in the example below)

    ```(text)
    ---
    username: ubuntu
    symbol_path: symbol-bootstrap
    preset_name: preset.yml
    symbol_password: hogehoge
    https_portal: true
    https_portal_path: https-portal
    ```

3： Execute the following command. You will be asked for your password, enter it. Pre-renewal preparations will be made. When "Setup done" is displayed, you are done.

```(text)
$ chmod +x setup.sh
$ ./setup.sh
```

4： Finally, execute the following command. You will be prompted for your password twice, enter it, and the Symbol node will be updated. Don't worry about the `WARNING` and other indications.

```(text)
$ ansible-playbook playbook.yml -v
```

5： Wait a few minutes, if the result is `unreachable=0` `failed=0`, you have succeeded. Go to the directory where symbol-bootstrap is stored and run `symbol-bootstrap -v` to make sure that the node is updated. You are done.

```(text)
PLAY RECAP **************************************************************************************************************************
localhost                  : ok=13   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ cd ~/symbol-bootstrap
$ symbol-bootstrap -v
symbol-bootstrap/1.0.5 linux-x64 node-v14.16.1
```

**If you want to update the Symbol node in the future, you can do so by simply executing the command in step 4.**

## Tips

- 手順4実行時にSSH connectionでエラーが発生した場合は、以下のそれぞれ(または両方)の可能性が考えられます。
  - パスワード認証を無効にしている：セキュリティ観点上、パスワード認証を無効にしている場合はツール使用時のみ有効にしてください。
  
    ```(text)
    $ sudo vi /etc/ssh/sshd_config

    PasswordAuthentication: yes

    $ sudo systemctl restart sshd
    ```

  - Port 22が閉じている：セキュリティ観点上、Port 22を閉じていてSSH用に別のポートを使用している場合は、conf/inventoryファイルを編集する必要があります。サーバーのIPアドレス(またはホスト名)の後ろに空白をあけ、以下の例のように`ansible_port=<SSH使用ポート>`と記載してください。単純に、22番ポートを閉じているだけ(SSH接続をしたことがない場合)であれば開放してください。

    ```(text)
    localhost ansible_port=55555
    ```

- 手順4実行時でのパスワード入力を省略したい場合は、対象ノードと鍵交換を実施しておいてください。そのうえで、**ansible.cfg** の`ask_pass`と`become_ask_pass`の値をfalseにしてください。

    ```(text)
    [defaults]
    inventory = ./conf/inventory
    ask_pass = false           # ← これ

    [privilege_escalation]
    become_method = sudo
    become_user = root
    become_ask_pass = false    # ← これ
    ```

--

- If an error occurs in the SSH connection when executing step 4, each (or both) of the following possibilities are possible.
  - Password authentication is disabled: For security reasons, if you have disabled password authentication, please enable it only when using the tool.
  
    ```(text)
    $ sudo vi /etc/ssh/sshd_config

    PasswordAuthentication: yes

    $ sudo systemctl restart sshd
    ```

  - Port 22 is closed: If you have closed Port 22 for security reasons and are using a different port for SSH, you need to edit the conf/inventory file. Leave a space after the IP address (or hostname) of the server and put `ansible_port=<port used for SSH>` as shown in the following example. Simply open port 22 if it is closed (if you have never made an SSH connection before).

    ```(text)
    localhost ansible_port=55555
    ```

- If you want to omit the password input at runtime in step 4, exchange keys with the target node. Then, set the values of `ask_pass` and `become_ask_pass` in **ansible.cfg** to false.

    ```(text)
    [defaults]
    inventory = ./conf/inventory
    ask_pass = false           # ← HERE

    [privilege_escalation]
    become_method = sudo
    become_user = root
    become_ask_pass = false    # ← HERE
    ```

## Donate

- Symbol (XYM): `NCTFRL5RGOAKAW4B3HZLUMEM6YGWI3WRK4V2OKY`
