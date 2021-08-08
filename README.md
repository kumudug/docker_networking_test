# Docker Networking / Cross Container Communication Test

* To build an image based on this code and start a container
   - Using nodemon to restart when code changes (during dev)
   - To refresh code changes use a bind mount to load code into the image
   - Start in detached mode
   - Remove container when stopped
   - Use a file to set environment variables
   - To build the container
      - `docker build . -t docker-networking:initial --build-arg DEFAULT_PORT=8080`
   - To create the image (overriding the default port just to test)
      - `docker run -p 3000:8081 --env-file ./.env -d --name network-app --rm -v $(pwd):/app:ro docker-networking:initial`
   - To test you can go to either of the below URLs
      - `http://localhost:3000/people`
      - `http://localhost:3000/movies`
   - To test code refresh is working
      - `docker logs network-app -f`
      - Change code and check whether logs indicate the server is restarting.

## Starting a MongoDB container based on the docker maintained mongo image

* [dockerhub-image](https://hub.docker.com/_/mongo?tab=description&page=1&ordering=last_updated)
   - `docker run -d --name mongodb mongo:5.0.2`
* Connecting using hacky way
   - Inspect the docker container created
      - `docker container inspect mongodb`
   - Find the container IP address
      ```
      "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "b012106e56b924be3f01ef6168d7ffdba5f472301c13b9baac6f2c15ef4b9c69",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "27017/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/b012106e56b9",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "934f044019f760bee07b726d82ca70ab4b2aaf9d9e0c1f991384c6d3b5f77c4f",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",
      ```
   - Use the ip to connect to the mongodb db in container
   - Test 
      - Post following json to : `http://localhost:3000/favorites` (RAW json in body)
         ```
         {
            "name": "Leia Organa",
            "type": "character",
            "url": "https://swapi.dev/api/people/5/"
         }
         ```
      - Go to or `http://localhost:3000/favorites` to test 