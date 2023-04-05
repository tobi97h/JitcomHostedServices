# Project

This project contains the central docker compose definition for all services related to the company once named Jitcom, currently its just named Claes & Huebner GbR. 

# Structure / Init

We define an external network called jitcom_net, starting it with the command `docker network create jitcom_net`. This only needs to be done once on the root server.

After this any other services can be deployed directly into this network, in their own compose files / startup scripts. The webserver that is in this main repository manages all the neccessary ssl certificates and routes requests to the other containers living in this network.

See the frontend of our [suggest app](https://github.com/Suggest-App/SuggestApp/blob/master/docker-compose_prod.yaml) for example. Here we deploy a nginx instance serving the frontend into our `jitcom_net`. 

Take note of the nginx configuration for this main nginx server (located in `vnginx/nginx/site-configs/default`):

```
...
    server_name suggest-app.com;

    location / {
        set $endpoint http://sg-frontend:80;
        proxy_pass $endpoint$request_uri;    
    }
...
```

Here any requests going to the domain suggest-app.com directly get routed ntto the sg-froend container defined [here again](https://github.com/Suggest-App/SuggestApp/blob/master/docker-compose_prod.yaml).

