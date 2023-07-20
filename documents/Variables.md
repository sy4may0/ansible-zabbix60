# Zabbix ServerとZabbix Agentの適用
Zabbix Serverをインストールするフラグを定義します。
```yml
zabbix_is_server: true
```

値が`true`の場合は**Zabbix Server**がインストールされます。

値が`false`の場合は**Zabbix Agent**のみインストールされます。

# 必要なリソースのURL設定
以下のリソースのURLを定義します。

**mariadb**のリポジトリURL及びGPGキーURL:
```yml
zabbix_mariadb_repos_baseurl: https://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/yum/10.8/rhel8-amd64
zabbix_mariadb_repos_gpgkey: https://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/yum/RPM-GPG-KEY-MariaDB
```

**zabbix**のリリースrpmのURL:
```yml
zabbix_repos_rpm_url: https://repo.zabbix.com/zabbix/6.0/rhel/8/x86_64/zabbix-release-6.0-4.el8.noarch.rpm
```

**zabbix snmptrap用Perlスクリプト**のURL:
```yml
zabbix_perlscript_source_url: https://cdn.zabbix.com/zabbix/sources/stable/6.0/zabbix-6.0.14.tar.gz
```

# データベース設定
mariadbのZabbix用データベース設定を定義します。

```yml
# Zabbix用データベース名
zabbix_db_name: zabbix

# Zabbix用データベースのcharacter_set
zabbix_db_character_set: utf8mb4

# Zabbix用データベースのcollation
zabbix_db_collation: utf8mb4_bin

# データベースアクセス用のユーザー名
zabbix_db_user: zabbix

# データベースアクセス用のユーザーパスワード
zabbix_db_password: zabbix00

# データベースアクセス用のユーザー権限
# (例は`zabbix_db_name`で定義したDBに*:ALLの権限を適用する。)
zabbix_db_privilege: "{{ zabbix_db_name }}.*:ALL"

# mariadbのrootログインパスワード
zabbix_db_root_password: root00

# mariadbのソケットファイル
zabbix_mysql_socket: /var/lib/mysql/mysql.sock
```

また、mariadbのチューニングパラメータ(`my.cnf -> mysql_tuning.cnf`)を定義します。
```yml
# innodb_buffer_pool_size設定
zabbix_mysql_buffer_pool_size: 128M 

# innodb_log_file_size設定
zabbix_mysql_log_file_size: 48M

# innodb_io_capacity設定
zabbix_mysql_io_capacity: 200 

# innodb_io_capacity_max設定
zabbix_mysql_io_capacity_max: 4000

# thread_cache_size設定
zabbix_mysql_thread_cache_size: 10

# innodb_flush_log_at_trx_commit設定
zabbix_mysql_flush_log_at_trx_commit: 2

# innodb_doublewrite設定
zabbix_mysql_doublewrite: 0

# max_connections設定
zabbix_mysql_max_connections: 100
```

mariadbチューニング設定は、Zabbix用にデフォルト値を変更しています。データの安全性を犠牲に書き込みパフォーマンスを向上させるような設定を行っています。

オプションパラメーターの説明は[mariadbd Options](https://mariadb.com/kb/en/mariadbd-options/)を確認してください。

# Zabbix Server設定
Zabbix Serverの設定(`zabbix_server.conf`)を定義します。
```yml
# 分割設定ファイルを格納するディレクトリ
zabbix_conf_directory: /etc/zabbix/zabbix_server.conf.d

# SNMPトラップログを書き込むファイル
zabbix_snmp_trapperfile: /var/log/snmptrap/snmptrap.log
```

また、各種チューニングパラメーターを定義します。
```yml
# StartSNMPTrapper設定
zabbix_start_snmptrapper: 1

# StartPollers設定
zabbix_start_pollers: 5

# StartPollersUnreachable設定
zabbix_start_pollers_unreachable: 1

# StartTrappers設定
zabbix_start_trappers: 5

# StartDiscoverers設定
zabbix_start_discoverers: 1

# StartHTTPPollers設定
zabbix_start_http_pollers: 1

# StartVMwareCollectors設定
zabbix_start_vmware_collectors: 0

# VMwareTimeout設定
zabbix_vmware_timeout: 10

# Timeout設定
zabbix_snmp_timeout: 4

# HistoryCacheSize設定
zabbix_history_cache_size: 48M

# ValueCacheSize設定
zabbix_value_cache_size: 24M

# CacheSize設定
zabbix_config_cache_size: 8M

# AllowRoot設定
zabbix_allow_root: 0
```

オプションパラメーターの説明は[プロセスの設定: Zabbixサーバー](https://www.zabbix.com/documentation/6.0/jp/manual/appendix/config/zabbix_server)を確認してください。

# Nginx設定
Nginxの設定(`/etc/nginx/conf.d/zabbix.conf`)を定義します。
```yml
# NginxのLISTENポート
zabbix_nginx_listen: 8080

# NginxのServer Name設定
zabbix_nginx_server_name: '_'
```

# その他Zabbix Server設定
ZabbixのWeb設定を定義します。
```yml
# Zabbix Webのサーバー名設定
zabbix_web_server_name: zabbix
```

設定ファイルのアノテーションに記載する情報を定義します。
```yml
# 設定編集者名
zabbix_annotation_author: 'Yamada Taro'
```

SNMPコミュニティ名を定義します。この設定は`snmptrapd`によるSNMPトラップ受信に適用されます。
```yml
# SNMPコミュニティ名
zabbix_snmp_community: public
```

# Zabbix Agent設定
Zabbix Agentの設定(`/etc/zabbix/zabbix_agentd.conf`)を定義します。
```yml
# ZabbixサーバーのIPアドレス/ホスト名
zabbix_server_address: 192.168.0.1

# Zabbixエージェントの登録ホスト名
zabbix_hostname: example

# ZabbixエージェントのAllowRoot設定
zabbix_agent_allow_root: 0
```
