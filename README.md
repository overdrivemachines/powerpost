# Sample Rails App with PostgreSQL

This is a sample app

## Version
- ruby 3.1.2
- Rails 7.0.3.1

## Install PostgreSQL
```sh
sudo apt update
sudo apt install postgresql postgresql-contrib libpq-dev
```

## App Setup Instructions

- Create New App:
```sh
rails new powerpost --database=postgresql
```

- Assign Production DB Password in `database.yml`
```yml
production:
  <<: *default
  database: powerpost_production
  username: deploy
  password: <%= Rails.application.credentials.dig(:prod_db_password) %>
```
- Add DB password in Rails Credentials:
```sh
EDITOR="subl --wait" rails credentials:edit
```
Add line `prod_db_password: mypassword`

- Stop/Start PostgreSQL:
```sh
sudo service postgresql stop
sudo service postgresql start
sudo service postgresql status
```

- Add User/Role and Database. The name must be the same as your username in Ubuntu. For production it will be deploy
```sh
sudo -u postgres createuser -s dipen -P
sudo -u postgres createdb dipen
```
-s = Add the superuser privilege<br>
-P = Password creation prompt

- Install App Dependencies on Production
```sh
bundle config set --local deployment 'true'
bundle config set --local without 'development test'
bundle install
```

- Create DB on Production
```sh
RAILS_ENV=production rails db:create
RAILS_ENV=production bundle exec rake assets:precompile db:migrate
sudo service nginx reload
```

- View DB exists:
```sh
psql
\l
```
\l - list all databases
\q - quit

- Add NGINX Configuration file
```sh
sudo nano /etc/nginx/sites-enabled/powerpost.conf
```

```
server {
    listen 80;
    server_name powerpost.dipenchauhan.com;

    # Tell Nginx and Passenger where your app's 'public' directory is
    root /home/deploy/www/powerpost/code/public;

    # Turn on Passenger
    passenger_enabled on;
    passenger_ruby /home/deploy/.rvm/rubies/ruby-3.1.2/bin/ruby;
}
```

Restart NGINX
```
sudo service nginx reload
passenger-config restart-app
```


## References

- Youtube: https://www.youtube.com/watch?v=5QUTfcO7BEw
- Install PostgreSQL: https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart
