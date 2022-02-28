# FlyFast

This repository contains the full source code for the Demo Application.

## Getting Started
### First Method
1. Clone and Update Submodules
```
git clone --recurse-submodules https://github.com/Aternity/FlyFast.git
```
### Second Method
1. Clone/download this repository.
```
git clone https://github.com/Aternity/FlyFast.git
```
2. Initialize submodules
```
git submodule init
```
3. Update submodules
```
git submodule update
```

## Updating Based On Future Changes
Stay up to date with the latest changes.
```
git submodule update --remote
```

## Starting and Stopping the Application Through Docker
1. Build our docker with no cache
```
docker-compose build --no-cache
```
2. Start the service 
```
docker-compose up
```
3. Stop the service
```
docker-compose stop
```