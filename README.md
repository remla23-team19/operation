# Operation 🏃

[![Latest Commit](https://img.shields.io/github/last-commit/remla23-team19/operation.svg)](https://github.com/remla23-team19/operation/commits/main) ![Docker Compose](https://img.shields.io/badge/repository-Docker%20Compose-blue)

All deployment files for the complete project.

⚠️ Make sure to change the `docker-compose.yml` file to the correct image tags if you want to use a specific version of the [app](https://github.com/remla23-team19/app) or [model-service](https://github.com/remla23-team19/model-service).

## Instructions

1. Clone this repository:

```
git clone https://github.com/remla23-team19/operation.git
```

2. Build the Docker containers using the latest version:

```
docker-compose build --no-cache
```

> Note: also perform this if you manually change version tags in the `docker-compose.yml` file. Otherwise, Docker will use the cached version.

3. Start the Docker containers:

```
docker-compose up
```

1. Open the web browser and go to:

```
http://localhost:9999
```

👉 Check sentiment of text (using our [app](https://github.com/remla23-team19/app)) based on the [model-service](https://github.com/remla23-team19/model-service).

Once you're finished, you can stop the process (`CTRL + C`) and the containers with:

```
docker-compose down
```
