## 概要 

CentOS7環境に、Zabbix3.0(zabbix-repo提供のRPM)を自動インストールするAnsibleのplaybookです。

## システム構成

* CentOS 7.3
* Mariadb
	+ 管理者ID/PASS = root / なし
	+ Zabbix用ID/PASS = zabbix / password 

* Zabbix3.0
	+ 管理者ID/PASS = Admin / zabbix  (zabbixの初期状態のまま)

### playbook実行の前に(実行要件)

* ansible(実行可能な)サーバからzabbix導入するサーバへは、ssh(id=root)で接続可能であること
* インターネット接続可能であること( zabbix-repo等取得します )

### playbook実行の前に(設定変更可能なポイント)

* Mariadbのパスワード(id=zabbix)を変更したい場合(2箇所修正)
 
	+ 変更対象(1): zabbix30/roles/application/files/zabbix.conf.php

```
$DB['PASSWORD'] = 'password';    <--passwordを指定したいパスワードに変更
```

	+ 変更対象(2): zabbix30/roles/application/vars/main.yml

```
- zabbix_mariadb_password: password   <--passwordを指定したいパスワードに変更
```

* インストール先のサーバのIP変更をしたい場合 

	+ 変更対象: zabbix30/inventory/inventory.ini
```
192.168.10.83 ansible_ssh_user=root <--192.168.10.83を指定したいパスワードに変更
```

### playbook実行

zabbix3.0の自動インストールが開始されます。

```
ansible-playbook -i zabbix30/inventory/inventory.ini zabbix30/zabbix30_deploy.yml
```

