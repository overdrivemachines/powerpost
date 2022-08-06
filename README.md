# Sample Rails App with PostgreSQL

This is a sample app

## Version
- ruby 3.1.2
- Rails 7.0.3.1

## Setup Instructions

- Create New App:
```sh
rails new powerpost --database=postgresql
```

- Stop/Start PostgreSQL:
```sh
sudo service postgresql stop
sudo service postgresql start
sudo service postgresql status
```

- Add User/Role and Database. The name must be the same as your username in Ubuntu.
```sh
sudo -u postgres createuser -s dipen -P
sudo -u postgres createdb dipen
```
-s = Add the superuser privilege<br>
-P = Password creation prompt

- Create DB
```sh
rails db:create
```

## References

- Youtube: https://www.youtube.com/watch?v=5QUTfcO7BEw
- Install PostgreSQL: https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart
