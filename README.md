## 概要

Ansible用playbookです。
CentOS7環境に、Zabbix-server(rpm)とZabbix-agent(rpm)の機能を自動設定する。


## 対象となる環境

* CentOS7 ( RHEL7 )
* インターネットにつながり、ansibleサーバからrootユーザで直接sshできること

## 自動設定内容

* os(共通(common))
	+ selinux無効化
	+ timezoneの設定(zone情報指定可能)
	+ chrony(時刻同期先IP指定可能)
	+ zabbix-repoの登録

* zabbix-server
	+ zabbix3.0(zabbix official repo)
	+ mariadb( DB名"zabbix"のユーザ"zabbix"のパスワード指定可能)
	+ httpd
	+ snmptrapd( snmptrapの受付コミュニティ名指定可能)
	+ snmptt(epel repo)

* zabbix-agent
	+ zabbix-agent3.0( zabbix-serverのIP指定可能)

# 指定可能なインストール先

* zabbix30/inventory/inventory.ini

```
[zabbix_servers] ... zabbix server
[zabbix_agents] ... zabbix-agent client
```

# 指定可能な設定内容

* zabbix30/roles/common/vars/main.yml

```
common:
- timezone: "Asia/Tokyo"  ... Timezone指定
    timeservers:
      - "192.168.10.1" ... TimeServer #1
      - "192.168.10.2" ... TimeServer #2
      ...............  ... TimeServer #..
zabbix_setup:
  - zabbix_mariadb_password: "password" ... MariaDB, zabbix's password
    snmptrap_community: "public"     ... snmptrapd 's community name
    zabbix_server_ip: "192.168.10.86" ... Zabbix server's IP
```

### playbook実行

ansibleサーバで実行
```
git clone https://github.com/mishikawan/zabbix30-ansible.git
cd zabbix30-ansible/
ansible-playbook -i zabbix30/inventory/inventory.ini zabbix30/site.yml
```

無事完了すると、zabbixサーバ上でzabbixサーバが稼働しています。以下、URLでアクセス可能。
```
http://{zabbix-ip}/zabbix
  ID = Admin
  PASS = zabbix
```
