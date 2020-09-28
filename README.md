install_zabbix_nginx_ssl_role
=========

Ansible Role to install zabbix monitoring server and nginx with ssl certificate, generated by letsencrypt.
Also the role does automatic action like: creating users, creating users groups, change logo and icon, install languages and configure Email settings.

Role Variables
--------------
Variables are located in [deafults/main.yml](defaults/main.yml) file.

###General Parameters###
---------------------

**hostname** --> Full name of the DNS pointed to the Machine IP (server.my.domain).
**domain** --> The domain if the DNS of the server.
**hostname_no_fqdn** --> The name of the server without the domain ending.
**install_nginx** --> Set true to install nginx (must be **true** for first installation).
**install_mariadb** --> Set true to install mariaDB (must be **true** for first installation).
**install_php** --> Set true to install php (must be **true** for first installation).
**install_zabbix** --> --> Set true to install zabbix (must be **true** for first installation).
**add_host** --> Set true just if you are installing zabbix again with existig DB and sql user.

**MariaDB parameters**
---------------------
**mysql_root_password** --> Root Password for mariaDB.


**letsencrypt parameters**
---------------------
**certbot_admin_email** --> Email Address for letsencrypt certificate generation.

**zabbix parameters**
---------------------
**version** --> General version (5.0,4.0)
**sub_version** '5.0-1' # Specifip version
**zabbix_db_name** --> zabbix Database name.
**zabbix_db_user** --> zabbix Database user.
**zabbix_db_password** --> zabbix Database user password.
**admin_user** --> Admin user fir zabbix.
**admin_password** --> Admin password for zabbix.

**zabbix Favicon parameters
--------------------------

**change_favicon** --> Set True for changing favicon.
**favicon_src** --> Path to favicon on the local host.

**Zabbix logo parameters**
--------------------------

**change_logo** --> Set True for changing logo.
**logo_src** --> Path to logo svg file on the local host.

**Zabbix Creating user parameters**
-----------------------------------
When you want to add one or more users, add to the list a dictionary contains all the below keys and give them values by your choice.

**create_user** --> Set true to create users.
**username** --> Username.
**user_password** --> User Password
**user_type** --> 1- Zabbix user, 2- Zabbix Admin, 3- Zabbix Super Admin.
**user_id:** --> IMPORTANT! Every run increase this var by 1, if you forgot the counting, set random nuber ( like 85, 869, 7996...) **Begin WITH 3!**.
**add_to_group** --> Group to add, can be Guests or Disabled or Zabbix administrators.
**adding_group_id** --> {{ (<user_id> + 2 }} IMPORTANT! the left opretor is user_id!

**Example**

```bash
create_user: false
users:
  - user_id: 8
    username: gal8 
    name: gal
    surename: Birkman
    user_password: 'Aa123456' 
    user_type: 3  
    add_to_group: 'Zabbix administrators'
    adding_group_id: "{{ 8 + 2 }}" ## IMPORTANT! the left opretor is user_id!

  - user_id: 9
    username: gal9
    name: Gal
    surename: Birk
    user_password: 'Aa123456'
    user_type: 1
    add_to_group: 'Clients'
    adding_group_id: "{{ 9 + 2 }}"
```

**Zabbix Creating user groups Parametes**
-----------------------------------------
Creating one or multiple user groups, add to the list a dictionary contains all the below keys and give them values by your choice.

**create_user_group:** --> Set true to create users.
**user_group_id** --> **IMPORTANT!** Every run or user group increase this var by 1, if you forgot the counting, set random nuber ( like 85, 869, 7996...), **Begin with 13**.
**user_group_name** --> user group name
**gui_access** --> Set 0 for system default gui Access
**users_status** --> Set 1 for user status ( prefer set 0)
**debug_mode** --> Set 1 for debug mode

```bash
user_groups:
  - user_group_id: 13
    user_group_name: 'Clients'
    gui_access: 1
    users_status: 0
    debug_mode: 0

  - user_group_id: 14
    user_group_name: 'Support'
    gui_access: 1
    users_status: 0
    debug_mode: 0
 ```
 
 **Zabbix Configure mail media type parameters**
 ----------------------------------------------
 
**config_email** --> Set true to config Email settings.
**smtp_server** --> SMTP server
**smtp_domain** --> SMTP provider domain name
**smtp_email** --> SMTP email account
**smtp_authentication** --> Set 0 for None 1 - username and password
**smtp_port** --> SMTP Port.
**smtp_security** --> Set 0 for None 1 for STARTTLS 2 for TLS/SSL
**smtp_verify_peer** --> Set 1 for true 0 for false (smtp security sub parameter)
**smtp_verify_host** --> Set 1 for true 0 for false (smtp security sub parameter)
**username** --> username for smtp authentication
**password** --> password for smtp authentication

**Zabbix Configure Email (HTML) media type parameters**
-------------------------------------------------------

**config_email_html** --> Set true to config Email settings.
**html_smtp_server** --> SMTP server
**html_smtp_domain** --> SMTP provider domain name
**html_smtp_email** --> SMTP email account
**html_smtp_authentication** --> Set 0 for None 1 - username and password
**html_smtp_port** --> SMTP Port.
**html_smtp_security** --> Set 0 for None 1 for STARTTLS 2 for TLS/SSL
**html_smtp_verify_peer** --> Set 1 for true 0 for false (smtp security sub parameter)
**html_smtp_verify_host** --> Set 1 for true 0 for false (smtp security sub parameter)
**html_username** --> username for smtp authentication
**html_password** --> password for smtp authentication


**Zabbix Parameters for installing languages**
----------------------------------------------

**install_language** --> Set true if you want to install languages.

Languages options:
english: en_US
Chinese: zh-CN
Czech: cs_CZ
French: fr_FR
Hebrew: he_IL
Italian: it_IT
Korean: ko_KR
Japanese: ja_JP
Norwegian: nb_NO
Polish: pl_PL
Portuguese_Portogal: pt_PT
Portuguese_Brazil: pt_BR
Russian: ru_RU
Slovak: sk_SK
Turkish: tr_TR
Ukrainian: uk_UA

**languages** --> For every language you want to install add line in the list with the value"{{ <language name> }}", all the options are above.
  
```bash
languages:
  - "{{ Czech }}"
  - "{{ Polish }}"
  - "{{ Italian }}"
```

Example Playbook
----------------
Before you run fill the paraneters in [deafults/main.yml](defaults/main.yml), <b>OR</b> set vars section in the playbook file:

```bash
- hosts: servers
  vars:
    - var1
    - var2
  roles:
    - install_zabbix_nginx_ssl_role
```
Every variable that will not entered above, his value will be taken from the defaults file.

Playbook after filling [deafults/main.yml](defaults/main.yml).

    - hosts: servers
      roles:
         - install_zabbix_nginx_ssl_role

License
-------

BSD

Author Information
------------------

<b>Gal Birkman, DevOps Engineer.</b><br>
<b>email:</b> galbirkman@gmail.com<br>
<b>GitHub:</b> https://github.com/galbirk
