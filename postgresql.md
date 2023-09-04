## Install postgresql 

Various OS support Linux/macOS/windows, and various architectures amd64, arm64


## Manage the service (under ubuntu)

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl stop postgreql
sudo systemctl disable postgresql
sudo systemctl status postgresql
```


## Get familiar with the psql (CLI tool)

Start the server (locally) as `postgres` user :  

```bash
sudo -iu postgres
```

Type in `psql` :  

```bash
postgres@server_name:~$ psql
```

You are now logged in as postgres user

```
postgres=#
```

Create an `user`, `database` :  

```bash
CREATE USER <name> WITH PASSWORD '<password>';
CREATE DATEBASE <dbname>;
ALTER USER <name> WITH PASSWORD '<password>';
```

Grant privileges :  

```bash
GRANT ALL PRIVILEGES ON DATABASE <dbname> TO <user>;
```

You can connect to the database locally (enter password) :   

```bash
psql -U <user> -d <dbname> -W 
```

## Connect to the server from the outside world

1) Open 5432 port 
2) Add ip addresses
3) Allow ip address/user to access database


1) Open port 5432

Install `ufw` (uncomplicated firewall if needed)

```bash
#sudo apt install ufw
sudo systemctl ufw enable
sudo ufw allow 5432/tcp
```

2) Edit `/etc/postgresql/15/main/postgresql.conf` (replace 15 with your postgres version):  
Replace `#listen_addresses = 'localhost'` with `listen_addresses = '*'`:

```bash
listen_addresses = '*'
```

3) Edit `/etc/postgresql/15/main/pg_hba.conf` (replace 15 with your postgres version):

Add those two lines to connect via `ipv4` and `ipv6` :  

```
host    all             <user>        0.0.0.0/0               md5
host    all             <user>        ::0/0                   md5
```


Restart the `postgresql` server :

```bash
sudo systemctl restart postgresql
```

You are good to go !

Connect remotely (Enter password when prompted):  

```bash
psql -U <user> -h <ip> -d <database -W 
```

