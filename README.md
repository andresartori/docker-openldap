# docker-openldap
Simple docker image for OpenLDAP

### How to run it
```
docker run \
      --name openldap-server \
        -p 389:389 \
        -p 636:636 \
        --hostname ldap.server.local \
	--env LDAP_ORGANISATION="ORG" \
	--env LDAP_DOMAIN="server.local" \
	--env LDAP_ADMIN_PASSWORD="Password" \
        --env LDAP_BASE_DN="dc=server,dc=local" \
        --volume /data/slapd/database:/var/lib/ldap \
        --volume /data/slapd/config:/etc/ldap/slapd.d \
	--detach osixia/openldap:latest
```

### Manager
```
docker run \
    --name phpldapadmin \
    -p 10080:80 \
    -p 10443:443 \
    --hostname phpldapadmin-service \
    --link openldap-server:ldap-host \
    --env PHPLDAPADMIN_LDAP_HOSTS=ldap.server.local \
    --detach osixia/phpldapadmin:latest
```
# Para verificar os usuários
```
docker-compose exec openldap bash
ldapsearch -x -h openldap -D "uid=ruan,ou=people,dc=example,dc=org" -b "ou=people,dc=example,dc=org" -w "$PASSWORD" -s base 'uid=ruan'
Or
ldapwhoami -vvv -h ldap://openldap:389 -p 389 -D 'uid=ruan,ou=people,dc=example,dc=org' -x -w "$PASSWORD"
```

