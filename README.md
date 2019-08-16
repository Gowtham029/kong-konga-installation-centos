# Kong API Gateway with Konga UI

### Install Kong CentOs

```
$ sudo yum update –y
$ sudo yum install -y wget
$ sudo wget https://bintray.com/kong/kong-rpm/rpm -O bintray-kong-kong-rpm.repo
$ sudo export major_version=`grep -oE '[0-9]+\.[0-9]+' /etc/redhat-release | cut -d "." -f1`
$ sudo sed -i -e 's/baseurl.*/&\/centos\/'$major_version''/ bintray-kong-kong-rpm.repo
$ sudo mv bintray-kong-kong-rpm.repo /etc/yum.repos.d/
$ sudo yum update -y
$ sudo yum install -y kong
```

### Adding privilages to the installation

```
$ sudo chmod -R 777 /etc/kong/
$ sudo chmod -R 777 /usr/local/kong
$ sudo chmod -R 777 /usr/local/share/lua/5.1
$ sudo cp /etc/kong/kong.conf.default /etc/kong/kong.conf
$ sudo ln -s /usr/bin/kong /usr/local/bin/kong
```

### Modify Kong configuration
```
$ sudo vi /etc/kong/kong.conf
```
Replace the postgress database details

  * Postgress
  * pg_user = <kong>
  * pg_host = <hostname>
  * pg_password = ******
  * pg_database = <kong>
  * pg_schema = <public>

#### Run kong migrations
```
$ sudo kong migrations bootstrap [-c /etc/kong/kong.conf]
```

#### Start Kong
```
$ sudo kong start [-c /etc/kong/kong.conf]
```

#### Check Kong health
```
$ sudo kong health
```
you can see the follwing
```
nginx.......running
Kong is healthy at /usr/local/kong
```
## Install Git

```
$ sudo yum install git
$ git --version
```

## Install Node.js
```
$ yum install -y gcc-c++ make
$ curl -sL https://rpm.nodesource.com/setup_6.x | sudo -E bash -
$ yum install nodejs
$ node -v
$ nvm -v
```

## Install Konga UI

Get the sources from https://github.com/pantsel/konga.git
Create .env file and add the following config
```
    NODE_ENV=production
    KONGA_HOOK_TIMEOUT=120000
    DB_ADAPTER=postgres
    DB_HOST=<hostname>
    DB_USER=<username>
    DB_PASSWORD=<password>
    DB_DATABASE=<database>
    DB_PG_SCHEMA=public
    KONGA_LOG_LEVEL=warn
```
navigate to config/connections.js
```
$ sudo vi config/connections.js
```
Modify the postgress config
```
postgres: {
    adapter: 'sails-postgresql',
    host: process.env.DB_HOST || 'localhost',
    user:  process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || 'admin1!',
    database: process.env.DB_DATABASE ||'konga_database',
    schema: process.env.DB_PG_SCHEMA ||'public',
    poolSize: process.env.DB_POOLSIZE || 10,
    ssl: process.env.DB_SSL ? true : false // If set, assume it's true
  }

```

```
$ npm install
```
```
$npm start
```
check the browser at http://localhost:1337
