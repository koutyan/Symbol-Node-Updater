# Symbol Node Updater

Easily update Symbol nodes.

## Overview

Symbolノードの更新を簡単に行えるツールです。設定を少しだけ編集してコマンドを実行するだけで簡単にSymbolノード(symbol-bootstrap)の更新を実施できます。複数ノードの一括更新にも対応しています。

This is a tool that makes it easy to update Symbol nodes. You can easily update Symbol nodes (symbol-bootstrap) by editing a few settings and executing a command. It also supports batch updating of multiple nodes.

## Attention

- このツールはSymbolノード(symbol-bootstrap)の更新のみをサポートしています。Symbolノードの構築自動化ツールについては、[Symbol Node Builder](https://github.com/koutyan/Symbol-Node-Builder) を参照ください。

- このツールのサポートOSは **Ubuntu** です。作者は *Ubuntu 20.04* でのテストを実施しています。**CentOS系では動作しません**。(ご要望があれば今後対応するかもしれません)

- 本ツールの使用は自己責任とします。本ツールを使用したことによる損害などの一切の責任を作者は負いません。

--

- This tool only supports updating Symbol nodes (symbol-bootstrap); for an automated tool to build Symbol nodes, see [Symbol Node Builder](https://github.com/koutyan/Symbol-Node-Builder).

- The supported OS for this tool is **Ubuntu**. The author has tested it on *Ubuntu 20.04*. **It does not work on CentOS systems**. (We may support this in the future if requested).

- Use of this tool is at your own risk. The author assumes no responsibility for any damage caused by the use of this tool.

## How to use

以下の操作は Ubuntu 上で実施してください。

1： ツールをダウンロードして解凍します。

  ```(text)
  $ wget https://github.com/koutyan/Symbol-Node-Updater/archive/refs/tags/v1.0.tar.gz
  $ tar xzvf v1.0.tar.gz
  $ cd Symbol-Node-Updater-1.0/
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

4： 最後に以下のコマンドを実行します。ユーザーパスワードとrootパスワードを求められるので、入力してください。Symbolノードの更新が実施されます。`WARNING`などの表示は気にしないでください。

```(text)
$ ansible-playbook playbook.yml -vvv
```

5： しばらく待って、実施結果が表示され、`failed=0` となっていれば成功です。symbol-bootstrapが格納されているディレクトリに移動して、`symbol-bootstrap -v` を実行して、ノードが更新されていることを確認してください。以上で完了です。

```(text)
PLAY RECAP **************************************************************************************************************************
localhost                  : ok=12   changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ cd ~/symbol-bootstrap
$ symbol-bootstrap -v
symbol-bootstrap/1.0.5 linux-x64 node-v14.16.1
```

**今後symbol-bootstrapの更新があった場合、手順4のコマンドを実行するだけで簡単にSymbolノードを更新できます。**

--

The following operations should be performed on Ubuntu.

1： Download and unzip the tool.

  ```(text)
  $ wget https://github.com/koutyan/Symbol-Node-Updater/archive/refs/tags/v1.0.tar.gz
  $ tar xzvf v1.0.tar.gz
  $ cd Symbol-Node-Updater-1.0/
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

3： Execute the following command. You will be asked for a password, enter it. You will be asked for your password. When "Setup done" is displayed, you are done.

```(text)
$ chmod +x setup.sh
$ ./setup.sh
```

4： Finally, execute the following command, then you will be prompted for your user password and root password, enter them: Symbol node updating will be performed. Don't worry about the `WARNING` and other indications.

```(text)
$ ansible-playbook playbook.yml -vvv
```

5： Wait a few minutes, if the result is `failed=0`, you have succeeded. Go to the directory where symbol-bootstrap is stored and run `symbol-bootstrap -v` to make sure that the node is updated. You are done.

```(text)
PLAY RECAP **************************************************************************************************************************
localhost                  : ok=12   changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ cd ~/symbol-bootstrap
$ symbol-bootstrap -v
symbol-bootstrap/1.0.5 linux-x64 node-v14.16.1
```

**If there is an update of symbol-bootstrap in the future, you can easily update the Symbol node by simply executing the command in step 4.**

## Tips

- 手順4での実行時でのパスワード入力を省略したい場合は、対象ノードと鍵交換を実施しておいてください。そのうえで、**ansible.cfg** の`ask_pass`と`become_ask_pass`の値をfalseにしてください。

    ```(text)
    [defaults]
    inventory = ./conf/inventory
    ask_pass = false           # ← これ

    [privilege_escalation]
    become_method = sudo
    become_user = root
    become_ask_pass = false    # ← これ
    ```

- 失敗した場合は、[Issue page](https://github.com/koutyan/Symbol-Node-Updater/issues) に以下の情報を添えて投稿してください。また、すでに解決している問題の可能性も高いため、投稿する前に過去のIssueを確認してください。(なお、手順3と4を何度実行してもノードに影響を及ぼさない設計としています。)

  - 手順のどこで失敗したのか (例: 手順4で失敗した)
  - エラー内容 (長い場合はコピペして別途ファイルにペーストして添付するのが望ましい)

- その他、ご意見やご要望なども上記Issue pageにて受け付けております。

--

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

- If this fails, please post the issue on the [Issue page](https://github.com/koutyan/Symbol-Node-Updater/issues) with the following information. It is also likely that the problem has already been solved, so please check the previous issues before posting. (Note that the design does not affect the nodes no matter how many times steps 3 and 4 are executed.)

  - Where in the procedure did it fail (e.g., step 4 failed)
  - Details of the error (if it is long, it is preferable to copy and paste it into a separate file and attach it)

- Other comments and requests are also welcome on the above Issue page.

## Donate

- Symbol (XYM): `NCTFRL5RGOAKAW4B3HZLUMEM6YGWI3WRK4V2OKY`
- My Symbol Node: `https://001symbol.blockchain-node.tech:3001`
