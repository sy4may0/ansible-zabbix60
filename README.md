zabbix60
=========
Zabbix Server 6.0及びZabbix Agentをインストールします。

Requirements
------------

RHEL8及びREHL8クローン(CentOS, AlmaLinux, RockyLinux)ベースのOSで使用できます。

Role Variables
--------------

変数のデフォルト値は`defaults/main.yml`を確認してください。必要に応じて、パラメーターをデフォルト値から変更する場合、`group_vars`等、他の変数定義ファイルを作成し値を上書きしてください。

変数の詳細な説明は[Variables](documents/Variables.md)を確認してください。

Dependencies
------------

None.

Example Playbook
----------------

Zabbix Serverインストール
```
- hosts: zabbix-servers
  vars:
    zabbix_is_server: true
  roles:
    - role: zabbix60
```

Zabbix Agentインストール
```
- hosts: zabbix-servers
  vars:
    zabbix_is_server: false
  roles:
    - role: zabbix60

```

License
-------

MIT

Author Information
------------------
This role was created in 2023 by [sy4may0](https://github.com/sy4may0).
