# FlyFast

This cookbook contains the full source code for FlyFast, utilizing both the  [FlyFast-WebUI](https://github.com/Aternity/FlyFast-WebUI) and [FlyFast-FlightSearch](https://github.com/Aternity/FlyFast-FlightSearch) to demonstrates how the [Alluvio Aternity DEM](https://www.riverbed.com/products/digital-experience-management) solution provides observability of a full-stack application using [OpenTelemetry](https://opentelemetry.io/) auto-instrumentation.

To instrument the FlyFast demo app, the OpenTelemetry agent will be containerized with the app and injected in the app at startup. The agent will be configured to export the tracing to the Alluvio Aternity APM SaaS backend via the [Alluvio Aternity OpenTelemetry Collector](https://hub.docker.com/r/aternity/apm-collector) that will run in an another container.

![diagram](/images/diagram.png)

## Prerequisites

1. [Git](https://git-scm.com/) (Optional)
2. an Aternity APM account (SaaS)
3. an Aternity UJI account (Optional)
4. a Docker host, for example [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Step by Step
### 1. Clone and Update Submodules
    
```
git clone --recurse-submodules https://github.com/Aternity/FlyFast.git --depth 1
```

Or

```
git clone https://github.com/Aternity/FlyFast.git
cd FlyFast
git submodule init
git submodule update
```

### 2. Get your CustomerID & SaaS Analysis Server Host details from the Aternity APM webconsole

Navigate to Aternity APM (for example [https://apm.myaccount.aternity.com](https://apm.myaccount.aternity.com)) > Agents > Install Agents:

1. Find your **CustomerID**, for example *12341234-12341234-13241234*
2. Grab **SaaS Analysis Server Host**, for example *agents.apm.myaccount.aternity.com*

Those information are required to activate the Aternity OpenTelemetry Collector container, passing via the environment variable `SERVER_URL`. 

### 3. Get your page tag from the Aternity UJI's sites page (Optional)

Navigate to Aternity UJI (for example [https://portals.bluetriangle.com](https://portals.bluetriangle.com)) > Sites > Select Site > JS Source:

1. Find your **JS Source**, for example *<script id=\"ALLUVIO-Aternity-UJI\" src=\"https:\/\/{{my-UJI-site-id}}\.btttag\.com\/btt\.js\"><\/script>*

Additional Information can be found [here](https://help.aternity.com/bundle/release_news_apm_agent_console_apm/page/console/topics/admin_config_uji.html).

### 4. Start the containers

Start the containers using the [docker-compose.yml](docker-compose.yml), for example with Bash:

```bash
cd FlyFast

# Configure the environment variables for the Aternity OpenTelemetry Collector
export ATERNITY_SAAS_SERVER_HOST="agents.apm.myaccount.aternity.com"
export ATERNITY_CUSTOMER_ID="12341234-12341234-13241234"

# Configure the environment variables for Aternity UJI (Optional)
# Replace {{my-UJI-tagid}} with the UJI site id 
export ALLUVIO_UJI_TAG='<script id=\"ALLUVIO-Aternity-UJI\" src=\"https:\/\/{{my-UJI-site-id}}\.btttag\.com\/btt\.js\"><\/script>'

# Build our docker with no cache
docker-compose build --no-cache

# Start the service
docker-compose up
```

or with PowerShell:

```PowerShell
cd FlyFast

# Configure the environement variable for the Aternity OpenTelemetry Collector
$env:ATERNITY_SAAS_SERVER_HOST="agents.apm.myaccount.aternity.com"
$env:ATERNITY_CUSTOMER_ID="12341234-12341234-13241234"

# Configure the environment variables for Aternity UJI (Optional)
# Replace {{my-UJI-tagid}} with the UJI site id 
$env:ALLUVIO_UJI_TAG='<script id=\"ALLUVIO-Aternity-UJI\" src=\"https:\/\/{{my-UJI-site-id}}\.btttag\.com\/btt\.js\"><\/script>'

# Build our docker with no cache
docker-compose build --no-cache

# Start the service
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
```
docker-compose stop
```

### Updating Based On Future Changes
Stay up to date with the latest changes.
```
git submodule update --remote
```

## License
Copyright (c) 2022 Riverbed Technology, Inc.

The contents provided here are licensed under the terms and conditions of the MIT License accompanying the software ("License"). The scripts are distributed "AS IS" as set forth in the License. The script also include certain third party code. All such third party code is also distributed "AS IS" and is licensed by the respective copyright holders under the applicable terms and conditions (including, without limitation, warranty and liability disclaimers) identified in the license notices accompanying the software.
