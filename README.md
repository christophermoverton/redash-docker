
# Redash + MongoDB + Redis + PostgreSQL Docker Setup

This repository contains the Docker setup for running Redash with MongoDB, Redis, and PostgreSQL. This setup includes the Redash web server, background worker, Redis for job queues, and PostgreSQL as the main database. **This app is designed to be used alongside the following repositories**:

- [docker_kafka_weatheralert](https://github.com/christophermoverton/docker_kafka_weatheralert.git): Provides the Kafka network and weather alert producer service.
- [weather-alert-consumer](https://github.com/christophermoverton/weather-alert-consumer.git): Consumes weather alerts from Kafka and stores them in MongoDB for Redash to query.

## Prerequisites

Before you start, make sure you have the following installed:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Related Repositories

This Redash setup works in conjunction with the following applications:

1. **[docker_kafka_weatheralert](https://github.com/christophermoverton/docker_kafka_weatheralert.git)**: This repository sets up Kafka and produces weather alert data.
2. **[weather-alert-consumer](https://github.com/christophermoverton/weather-alert-consumer.git)**: This repository consumes weather alert data from Kafka and stores it in MongoDB. Redash will query MongoDB to visualize this data.

Ensure that these repositories are set up and running in your environment to provide data for Redash.

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-repo/redash-docker-setup.git
cd redash-docker-setup
```

### 2. Configuration

You can modify the `docker-compose.yml` to adjust settings for your environment. The `docker-compose.yml` defines the following services:

- **Redash**: The main Redash web server.
- **Redash Worker**: Background worker for processing queries and scheduled tasks.
- **PostgreSQL**: The primary database for Redash, storing queries, dashboards, users, etc.
- **Redis**: Used by Redash for job queuing.
- **MongoDB**: An external MongoDB instance connected to Redash for querying data.

### 3. Environment Variables

Make sure to update the following environment variables in the `docker-compose.yml` file based on your environment:

- **REDASH_DATABASE_URL**: PostgreSQL connection string for Redash.
- **REDASH_REDIS_URL**: Redis connection string.
- **REDASH_MONGODB_URL**: MongoDB connection string.

Example MongoDB URL:

```yaml
REDASH_MONGODB_URL: "mongodb://monitorUser:monitorPassword@172.20.0.4:27017/admin"
```

### 4. Build and Start the Services

Run the following command to build and start the services in detached mode:

```bash
docker-compose up --build -d
```

### 5. Access Redash

Once the containers are up and running, you can access Redash in your web browser:

```
http://localhost:5000
```

### 6. Stopping the Services

To stop all the services, run:

```bash
docker-compose down
```

## Services Overview

### Redash
- URL: `http://localhost:5000`
- Redash is an open-source data visualization and dashboarding tool. It allows you to query multiple data sources, create visualizations, and build interactive dashboards.

### Redis
- Redis is used by Redash to manage job queues, including query execution and scheduled tasks.

### PostgreSQL
- PostgreSQL serves as the primary database for storing Redash metadata, including queries, dashboards, and users.

### MongoDB
- MongoDB is configured as an external data source. Redash queries MongoDB to retrieve data for visualizations and dashboards.

### Kafka Integration (via Related Repositories)

- **docker_kafka_weatheralert**: Produces weather alert data via Kafka.
- **weather-alert-consumer**: Consumes Kafka messages and stores them in MongoDB, allowing Redash to query the alert data.

## Customization

- You can increase the number of worker processes in the `docker-compose.yml` by modifying the `WORKERS_COUNT` environment variable under the `redash_worker` service.

## Troubleshooting

### Common Issues:

1. **Cannot connect to Redis**: Ensure the `REDASH_REDIS_URL` is correctly set to `redis://redis:6379/0` in the `docker-compose.yml`.

2. **Query taking too long**: Check that the Redash worker is running and properly connected to Redis. You can inspect the worker logs with:
   ```bash
   docker logs redash-docker-redash_worker-1
   ```

3. **Job Queue Stuck**: If jobs are stuck in the queue, try restarting the worker:
   ```bash
   docker-compose restart redash_worker
   ```

## Contributing

Feel free to submit issues or pull requests to contribute to this project.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
