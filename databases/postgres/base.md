install
。。。。

init
initdb /usr/local/var/postgres --encoding=UTF-8 --username=postgres -A md5 -W
initdb --help



/opt/homebrew/Cellar/postgresql@14/14.5_3/bin/initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@14 --username=postgres -A md5 -W

initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@15 --username=postgres -A md5 -W


base operation
psql -U [user] -d [database] -h [host] -p [post]

DROP DATABASE postgres;
CREATE USER asset WITH PASSWORD 'password';
DROP USER postgres
CREATE DATABASE postgres OWNER postgres;
GRANT ALL PRIVILEGES ON DATABASE asset to asset;

CREATE USER sonar WITH PASSWORD 'Sonar#123';
ALTER USER postgres WITH PASSWORD 'Postgres#$%345'

CREATE DATABASE sonardb OWNER sonar;

ALTER ROLE postgres CREATEDB;

GRANT ALL PRIVILEGES ON DATABASE sonardb to sonar;


pg_resetwal -f /usr/local/var/postgres

docker run -d --name sonarqube -p 9000:9000 --volume sonarqube_data:/opt/data/sonarqube

docker run -d -p 9000:9000 --name sonarqube -v /opt/data/sonarqube:/opt/sonarqube

DROP TABLE  IF EXISTS USER;

base command
\password：设置当前登录用户的密码
\h：查看SQL命令的解释，比如\h select。
\?：查看psql命令列表。
\l：列出所有数据库。¥d
\c [database_name]：连接其他数据库。
\d：列出当前数据库的所有表格。
\d [table_name]：列出某一张表格的结构。
\du：列出所有用户。
\e：打开文本编辑器。
\conninfo：列出当前数据库和连接的信息。
\password [user]: 修改用户密码
\q：退出


-- Copy CSV data, with appropriate munging:
COPY land_registry_price_paid_uk FROM '/path/to/pp-complete.csv' with (format csv, encoding 'win1252', header false, null '', quote '"', force_null (postcode, saon, paon, street, locality, city, district));

