# Zabbix example environment


This is a project that launch a Zabbix example environment for testing purposes using [Vagrant](http://www.vagrantup.com). It includes:

- Zabbix server (Debian 10): Zabbix Server 5.2, Apache and PostgreSQL 12 with TimescaleDB.
- Postgres example server (Debian 10): PostgreSQL 12 and Zabbix Agent 5.2.
- Apache example server (Debian 10): Apache HTTPD 12 and Zabbix Agent 5.2.

<p float="left">
  <img src="img/snapshot_1.png" width="400px" height="auto">
  <img src="img/snapshot_2.png" width="400px" height="auto">
  <img src="img/snapshot_3.png" width="400px" height="auto">
</p>

## What is Vagrant?

Vagrant is a tool that uses virtual machines to dynamically build configurable, lightweight, and portable virtual machines. Vagrant supports the use of either Puppet or Chef for managing the configuration. Much more information is available on the [Vagrant web site](http://www.vagrantup.com).

## How do I install Vagrant?

The VirtualBox version used is 6.1 and Vagrant version is v2.2.14.

- Download VirtualBox 6.1: https://www.virtualbox.org/wiki/Downloads
- Download Vagrant 2.2.14: https://www.vagrantup.com/downloads

## How do I run?

1. Start Zabbix server:

```bash
cd server
vagrant up
```

2. Get Zabbix server IP:

```bash
# Copy value of "HostName"
vagrant ssh-config
```

3. Start Postgres example server:

```bash
cd ..

cd postgres-server

ZABBIX_SERVER=PREVIOUS_IP vagrant up --zabbix-server 
```

4. Start Apache example server:

```bash
cd ..

cd postgres-server

ZABBIX_SERVER=PREVIOUS_IP vagrant up --zabbix-server 
```

5. Configure auto-registration in Zabbix:

- Open http://IP_ZABBIX_SERVER/ usding credentials user ```Admin``` and password ```zabbix```
- Add auto registration trigger: Configuration -> Action -> Autoregistration actions -> Create actions:
  - Postgres:  
    - Conditions:
      - "Host name" contains "postgres".
    - Operations:
      - "Add host"
      - "Link Template host" for templates "Linux by Zabbix agent" and "PostgreSQL by Zabbix agent 2"
  - Apache:  
    - Conditions:
      - "Host name" contains "apache".
    - Operations:
      - "Add host"
      - "Link Template host" for templates "Linux by Zabbix agent" and "Apache by Zabbix agent"
