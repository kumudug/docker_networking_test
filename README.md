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
