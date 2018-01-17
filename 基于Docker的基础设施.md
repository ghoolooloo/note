# MySQL

```shell
docker run --name mysql-jo
           -e MYSQL_ROOT_PASSWORD=root
           -e MYSQL_DATABASE=keycloak
           -e MYSQL_USER=keycloak
           -e MYSQL_PASSWORD=keycloak
           -v /var/jo/mysql/conf.d/:/etc/mysql/conf.d/
           -p 3307:3306
           -d
           mysql:5.7.21
```

自动创建的keycloak用户，会授予超级用户权限。

# Keycloak

```shell
docker run --name keycloak-jo
           --link mysql-jo:mysql
           -e KEYCLOAK_USER=admin
           -e KEYCLOAK_PASSWORD=admin
           -e MYSQL_DATABASE=keycloak
           -e MYSQL_USER=keycloak
           -e MYSQL_PASSWORD=keycloak
           -e MYSQL_PORT_3306_TCP_ADDR=172.17.0.4
           -e MYSQL_PORT_3306_TCP_PORT=3306
           -e PROXY_ADDRESS_FORWARDING=true
           -v /var/jo/keycloak/themes/jo:/opt/jboss/keycloak/themes/jo
           -p 8181:8080
           -d
           jboss/keycloak:3.4.3.Final
```

MYSQL_PORT_3306_TCP_ADDR：必须是mysql-jo容器的IP，而不是宿主机的IP。

MYSQL_PORT_3306_TCP_PORT：必须是mysql-jo容器中的端口，而不是映射到宿主机的端口。

另外，不建议将themes目录映射到宿主机上，因为这会造成该目录为空目录。需要手动将默认主题复制到对应的宿主机目录中。而只映射themes的子目录jo，这样宿主机上虽然只能看到jo目录，但在容器中，themes目录原有的内容还在。