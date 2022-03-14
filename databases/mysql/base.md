
# 基本操作

## パスワード変更

``` code
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Mysql@password';
FLUSH PRIVILEGES;
```

## ユーザー作成

```code
CREATE USER 'zero'@'%' IDENTIFIED BY 'Mysql@password';
CREATE USER 'zero'@'%localhost' IDENTIFIED BY 'Mysql@password';
FLUSH PRIVILEGES;
```

##　権限追加

```code
GRANT ALL PRIVILEGES ON *.* TO 'zero'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'zero'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
