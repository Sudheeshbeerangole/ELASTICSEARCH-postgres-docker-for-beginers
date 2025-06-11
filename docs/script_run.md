## Running scripts to load data to ES
We will be reading data from Postgres and loading it to Elasticsearch using a script.


#### First make sure that Elasticsearch and Kibana is up and running.
Go to any browser, in search bar and enter 127.0.0.1:9200 to check elasticsearch service, enter 
127.0.0.1:5601 to check kibana service.

### Load schema to ES
In Kibana, go to Dev Tools page, paste the below command and run. 
```
PUT /_index_template/products_template
{
  "index_patterns": [
      "product_*"
    ],
  "template": {
    "mappings": {
      "dynamic": "strict",
      "properties": {
        "productId": {
          "type": "text"
        },
        "productCategory": {
          "type": "text"
        },
        "OrderStats": {
          "type": "object",
          "properties": {
            "orderCount": {
              "type": "integer"
            },
            "minPrice": {
              "type": "float"
            },
            "maxPrice": {
              "type": "float"
            }
          }
        }
      }
    }
  }
}
```

### Build script docker image
###### Note: Please make the following changes in the Dockerfile 
```
ENV POSTGRES_HOST='127.0.0.1'
ENV ES_HOST='127.0.0.1'
For the above lines, replace '127.0.0.1' with your local system ip
```
Build image
```
cd ELASTICSEARCH-postgres-docker-for-beginers
docker build -t "ws2_es_script" .
```

### Bring up script container.
```
docker run -it ws2_es_script:latest bash
```

### Run the script
```
python3 app.py
```


