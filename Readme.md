# Getting Started with LDAP

We will use the [docker-openldap](https://github.com/osixia/docker-openldap) project. It's a Docker container running OpenLDAP. To manage our LDAP server easily, we will use [docker-phpLDAPadmin](https://github.com/osixia/docker-phpLDAPadmin) project, it provides a fancy web interface that will save us from fiddling with LDAP CLI.

## Installation

The [docker-phpLDAPadmin](https://github.com/osixia/docker-phpLDAPadmin) project provides an [OpenLDAP & phpLDAPadmin](https://github.com/osixia/docker-phpLDAPadmin#openldap--phpldapadmin-in-1) in a helper script. Paste it in a file, execute it (no need to be root), and voilà! OpenLDAP running with a management interface. 

```bash
#!/bin/bash -e
# You can change the first value of the port designations to whichever port you want, e.g. instead of 6443, you can use 8443.
docker run --name ldap-service -p 389:389 -p 636:636 --hostname ldap-service --detach osixia/openldap:latest
docker run --name phpldapadmin-service -p 6443:443 --hostname phpldapadmin-service --link ldap-service:ldap-host --env PHPLDAPADMIN_LDAP_HOSTS=ldap-host --detach osixia/phpldapadmin:latest
PHPLDAP_IP=$(docker inspect -f "{{ .NetworkSettings.IPAddress }}" phpldapadmin-service)
echo "Go to: https://$PHPLDAP_IP or https://localhost:6443"
echo "Make sure to set ldap.url=ldap://localhost:389 in sonar.properties"
echo "Login DN: cn=admin,dc=example,dc=org"
echo "Password: admin"
```

## Usage

Go to: [https://localhost:6443](https://localhost:6443)

Login DN: cn=admin,dc=example,dc=org

Password: admin

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)