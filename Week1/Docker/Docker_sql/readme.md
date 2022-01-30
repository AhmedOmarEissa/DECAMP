video https://www.youtube.com/watch?v=2JM-ziJt0WI&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb&index=4

https://www.youtube.com/watch?v=hCAIVe9N0ow&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb&index=6
https://www.youtube.com/watch?v=B1WwATwf-vY&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb&index=6
note https://docs.google.com/document/d/e/2PACX-1vRJUuGfzgIdbkalPgg2nQ884CnZkCg314T_OBq-_hfcowPxNIA0-z5OtMTDzuzute9VBHMjNYZFTCc1/pub


# RUN A DOCKER
docker build -t test:firstrun .
docker run -it test:firstrun

# RUN PG SQL
docker run -it \
-e POSTGRES_USER="root" \
-e POSTGRES_PASSWORD="root" \
-e POSTGRES_DB="ny_taxi" \
-v "/Users/ahmed.omar/Documents/GitHub/ZoomCamp/DECAMP/Docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data" \
-p 5432:5432 \
postgres:13

# RUN PG ADMIN
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4


## Network 
docker network create pg-network 

docker run -it \
-e POSTGRES_USER="root" \
-e POSTGRES_PASSWORD="root" \
-e POSTGRES_DB="ny_taxi" \
-v "/Users/ahmed.omar/Documents/GitHub/ZoomCamp/DECAMP/Docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data" \
-p 5432:5432 \
--network=pg-network \
--name=pg-database \
postgres:13

docker run -it \
-e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
-e PGADMIN_DEFAULT_PASSWORD="root" \
-p 8080:80 \
--network=pg-network \
--name=pg-admin \
dpage/pgadmin4



## Docker Useful Info
docker ps
docker stop 6b52da1c9d3b
docker rm 5cab6731d072870e0ab11bdd1b7fb02842761c320106ae0c146a35a20ec990bb
docker kill 6b52da1c9d3b
docker network ls


https://stackoverflow.com/questions/38088279/communication-between-multiple-docker-compose-projects

## CONDA
conda info --envs



## Python 
URL="https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2021-01.csv"

/Users/ahmed.omar/opt/anaconda3/bin/python ingest_data.py --user=root --password=root --host=localhost --port=5432 --db=ny_taxi --table_name=yellow_taxi_trips --url=${URL}


## Python Dockerized 
Build the image

docker build -t taxi_ingest:v001 .

Run the script with Docker

URL="https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2021-01.csv"

docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}




## COMPOSE 

docker-compose up
docker-compose down 

docker-compose up -d


docker_sql_default

## USING DOCKER Network 
docker run -it \
  --network=docker_sql_default \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}

