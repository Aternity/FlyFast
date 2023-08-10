# FlyFast

This cookbook contains the full source code for FlyFast, utilizing both the  [FlyFast-WebUI](https://github.com/Aternity/FlyFast-WebUI) and [FlyFast-FlightSearch](https://github.com/Aternity/FlyFast-FlightSearch) to demonstrates how the [ALLUVIO Aternity DEM](https://www.riverbed.com/products/digital-experience-management) solution provides observability of a full-stack application using [OpenTelemetry](https://opentelemetry.io/) auto-instrumentation.

To instrument the FlyFast demo app, the OpenTelemetry agent will be containerized with the app and injected in the app at startup. The agent will be configured to export the tracing to the ALLUVIO Aternity APM SaaS backend via the [ALLUVIO Aternity OpenTelemetry Collector](https://hub.docker.com/r/aternity/apm-collector) that will run in an another container.

![diagram](/images/diagram.png)

## Prerequisites

1. an account for ALLUVIO Aternity APM (SaaS)
2. optional - an account for ALLUVIO Aternity UJI
3. a Docker host, for example [Docker Desktop](https://www.docker.com/products/docker-desktop)
4. git installed, see [Git](https://git-scm.com/)

## Step by Step

### 1. Get a local copy
    
```shell
git clone --recurse-submodules https://github.com/Aternity/FlyFast.git --depth 1
```

### 2. Get your CustomerID & SaaS Analysis Server Host details for APM

Open ALLUVIO Aternity APM (for example [https://apm.myaccount.aternity.com](https://apm.myaccount.aternity.com)) and navigate to Agents > Install Agents

1. Find your **CustomerID**, for example *12341234-12341234-13241234*
2. Grab **SaaS Analysis Server Host**, for example *agents.apm.myaccount.aternity.com*

Those information are required to activate the container of the [Aternity APM Collector](https://hub.docker.com/r/aternity/apm-collector)

### 3. Get the UJI Tag Prefix (Optional)

1. Open [Aternity UJI](https://portals.bluetriangle.com) and navigate to Settings & Administration > Sites
2. Find the site configured for FlyFast and get the **UJI Tag Prefix**, for example *my-UJI-Tag-Prefix-FlyFast*

### 4. Start the containers

Start the containers using the [docker-compose.yml](docker-compose.yml), for example with Bash:

```bash
cd FlyFast

# Configure the ALLUVIO Aternity APM OpenTelemetry Collector
# Replace the value with the information collected at step 2.
export ATERNITY_CUSTOMER_ID="12341234-12341234-13241234"
export ATERNITY_SAAS_SERVER_HOST="agents.apm.myaccount.aternity.com"

# Optional - Configure the ALLUVIO Aternity UJI tag
# Replace "my-UJI-Tag-Prefix-FlyFast" with your UJI Tag Prefix collected at step 3.
export ALLUVIO_UJI_TAG='<script id=\"ALLUVIO-Aternity-UJI\" src=\"https:\/\/my-UJI-Tag-Prefix-FlyFast\.btttag\.com\/btt\.js\"><\/script>'

# Build the containers
docker compose build --no-cache

# Start the containers
docker compose up
```

or with PowerShell:

```PowerShell
cd FlyFast

# Configure the ALLUVIO Aternity APM OpenTelemetry Collector
# Replace the value with your information collected at step 2.
$env:ATERNITY_CUSTOMER_ID="12341234-12341234-13241234"
$env:ATERNITY_SAAS_SERVER_HOST="agents.apm.myaccount.aternity.com"

# Optional - Configure the ALLUVIO Aternity UJI tag
# Replace "my-UJI-Tag-Prefix-FlyFast" with your UJI Tag Prefix collected at step 3.
$env:ALLUVIO_UJI_TAG='<script id=\"ALLUVIO-Aternity-UJI\" src=\"https:\/\/my-UJI-Tag-Prefix-FlyFast\.btttag\.com\/btt\.js\"><\/script>'

# Build the containers from scratch
docker-compose build --no-cache

# Start the containers
docker-compose up
```

### 5. Navigate Through The Application And Monitor

The web application should now be available on [http://localhost](http://localhost).

Open the url in your browser and navigate through the application a few times to generate some traffic.

### 6. ALLUVIO Aternity APM webconsole

Go to the APM webconsole to monitor the instance and observe every transaction.

![Alluvio Aternity APM OpenTelemetry Traces](/images/transaction.png)

View details of a specific transaction as a waterfall chart:

![Alluvio Aternity APM OpenTelemetry Transaction-Detail](/images/transaction-detail.png)

## Notes

### Stop The App and All The Containers

Press `CTRL + C` in the shell where it is running.

Or in a shell, go to the folder where you keep the [docker-compose.yml](docker-compose.yml) and run:

```shell
docker-compose stop
```

### Updating Based On Future Changes

Stay up to date with the latest changes.

```shell
git submodule update --remote
```

### Clone FlyFast and update submodules

```shell
git clone https://github.com/Aternity/FlyFast.git
cd FlyFast
git submodule init
git submodule update
```

## License
Copyright (c) 2022 Riverbed Technology, Inc.

The contents provided here are licensed under the terms and conditions of the MIT License accompanying the software ("License"). The scripts are distributed "AS IS" as set forth in the License. The script also include certain third party code. All such third party code is also distributed "AS IS" and is licensed by the respective copyright holders under the applicable terms and conditions (including, without limitation, warranty and liability disclaimers) identified in the license notices accompanying the software.
