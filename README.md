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

* ansible(実行可能な)サーバと、zabbix(導入する)サーバの2台が必要

* ansibleサーバからzabbixサーバへは、ssh(id=root)で接続可能であること

* インターネット接続可能であること( yum/rpm実行するため )

### playbook実行の前に(設定変更必要なポイント)

* zabbixインストール先のサーバのIP変更 (zabbixサーバのIPに変更)

	+ 変更ファイル: zabbix30/inventory/inventory.ini
```
192.168.10.83 ansible_ssh_user=root  <--192.168.10.83をzabbixサーバIPに変更
```

### playbook実行

zabbix3.0の自動インストールが開始。

ansibleサーバで実行
```
git clone https://github.com/mishikawan/zabbix30-ansible.git
cd zabbix30-ansible/
ansible-playbook -i zabbix30/inventory/inventory.ini zabbix30/zabbix30_deploy.yml
```

無事完了すると、zabbixサーバ上でzabbixサーバが稼働しています。以下、URLでアクセス可能。
```
http://{zabbix-ip}/zabbix
  ID = Admin
  PASS = zabbix
```
