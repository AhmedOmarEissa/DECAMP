Week output
Website -> curl in airflow worker (Docker Compose) -> ingest in postgres (Dockercompose)




# docker-compose build

Review  of airflow output 
889d17ebdc14 = worker container
docker exec -it 889d17ebdc14 bash
ls 
exit



https://stackoverflow.com/questions/38088279/communication-between-multiple-docker-compose-projects


#test connectivity 
docker exec -it d52e6478f2d6 bash #worker id 
python 
from sqlalchemy import create_engine
engine = create_engine('postgresql://root:root@pgdatabase:5432/ny_taxi' )
engine.connect()