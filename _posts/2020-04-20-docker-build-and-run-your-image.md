**Docker. Build and run your image**

#### Dockerfile
```dockerfile
# Use the official image as a parent image.
FROM java:8-jre
# Copy the file from your host to your specified folder.
ADD ./build/libs/server-mock-0.0.1-SNAPSHOT.jar /app/
# Run the specified command within the container.
CMD ["java", "-Xmx200m", "-jar", "/app/social-mock-0.0.1-SNAPSHOT.jar"]
# Inform Docker that the container is listening on the specified port at runtime.
EXPOSE 8080
```

#### Build image
```bash
docker build --tag server-mock:1.0 .
```

#### Run your image as a container
```bash
docker run -p 8081:8080 --name server-mock server-mock:1.0
```
 - `-p 8081:8080` 8081 - app's port in docker container, 8080 - host's port
 - `--name server-mock` - container name
 
 now it will be available on http://localhost:8081