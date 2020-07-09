# iris_systems_task

## Features
* Based on Rails app [Shopping-App-IRIS](https://github.com/VithikShah/Shopping-App-IRIS) 
* All services are containerized.
* Nginx for reverse proxy and load balancing.
* Uses docker-compose.
* Can scale with load balancing using Nginx.
* Mysql data and Nginx conf file are persisted using volumes.
* Just Nginx entry port is exposed to host and other services communicate intternally in docker network.
* Nginx rate limiting added (set to 1000r/s).

## Steps 
1. Clone this repo.
```
git clone https://github.com/shivamanipatil/iris_systems_task.git
```
2. Build the images for containers.
```
sudo docker-compose build
```
3. Spins the containers.
```
docker-compose up --scale web=3
```
  Launches 3 instaces of rails application load balanced by Nginx.
  
4. For the first time create and run migrations for the database for rails.
```
docker-compose run --rm web rake db:create db:migrate
```
5. Application is live on localhost:8080
6. For daily(at 00:00 can be changed) database dump with name as shop1_development_date_time in created db_dump directory, add this line to crontabfile (maybe use crontab -e)
```
@daily docker-compose exec db /usr/bin/mysqldump -u root --password=complexpassword shop1_development > ~/db_dump/shop1_development_`date '+%Y_%m_%d__%H_%M_%S'`.sql
```
