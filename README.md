# postfix

![ansible ci](https://github.com/link-u/ansible-roles-v2_postfix/workflows/ansible%20ci/badge.svg)

## 概要

postfix設定role。postfixなので送信のみ。

## 動作確認バージョン

* Ubuntu: 18.04, 20.04
* ansible: 2.8, 2.9

<br>

### Role variables

```yaml
### インストール設定 ###############################################################################
## 基本設定
postfix_install_flag: True  # インストールフラグ


### main.cf の設定 ################################################################################
## 基本設定
postfix_domain: "link-u.co.jp"
postfix_hostname: "{{ postfix_domain }}"              # デフォルトでは postfix_domain
postfix_destination: "{{ postfix_domain }}"           # デフォルトでは postfix_domain
postfix_network:
  - "127.0.0.0/8"
  - "[::ffff:127.0.0.0]/104"
  - "[::1]/128"
postfix_enable_reception: no                          # インターネットからメールを受信したい場合は yes にする.
postfix_mailbox_size_limit: "{{ 100 * 1024 * 1024 }}" # 100MB

## virtual alias setting
#   * 辞書のマージに対応
#   * 辞書変数の一部だけを修正可能.
postfix_virtual_alias:
  domains: []
  map_path: /etc/postfix/virtual
  maps: {}

## main.cf への追加設定
postfix_extra_main_confs: {}
### 設定方法 (追加 or 書き換えたい key と value のみ記述)
# postfix_extra_main_confs:
#   key1: "value1"
#   key2: "value2"
#   (省略)
#
### 簡単な説明
# ※1 ansible の lineinfile モジュールを使って適用する
# ※2 参考 URL リスト
#     - http://www.postfix.org/documentation.html (公式ドキュメントのトップページ)
#     - http://www.postfix.org/postconf.5.html (Postfix Configuration Parameters)

## alias の設定
postfix_aliases: {}
### 設定例
# postfix_aliases:
#   postmaster: root
#   script: "php /path/to/php/script.php"
```

### Example playbook

```yaml
- hosts:
    - servers
  become: True
  roles:
    - { role: postfix, tags: ["postfix"] }
```

## License

Unless otherwise stated this repository licensed under MIT license. Please read [LICENSE](LICENSE) for the details.

### templates/main.cf.j2

The original version of [templates/main.cf.j2](templates/main.cf.j2) is obtained from http://www.postfix.org/ and is licensed under the Eclipse Public License. Please read [LICENSE.postfix](LICENSE.postfix) for the details.
