# FlyFast

This repository contains the full source code for the Demo Application.

## Starting and Stopping the Application Through Docker
1. Make sure in [package.json](/FlyFast-WebUI/package.json), the proxy contains the container name of the backend, FlightSearch.
    - In this case, this could be done by renaming `"proxy": "http://localhost:8080"` to `"proxy": "http://Search:8080"`
2. Build our docker with no cache
```
docker-compose build --no-cache
```
3. Start the service 
```
docker-compose up
```
4. Stop the service
```
docker-compose stop
```