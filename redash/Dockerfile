# Base Redash image
FROM redash/redash:latest

# Switch to the root user to ensure we have the necessary permissions
USER root

# Ensure Python 3 and pip 3 are available
RUN apt-get update && apt-get install -y python3 python3-pip iputils-ping

# Install pymongo using pip3 (no need for kafka-python)
RUN pip3 install pymongo

# Run database migrations to create necessary tables
CMD ["bash", "-c", "python manage.py database create_tables && /app/bin/docker-entrypoint server"]

# Expose port 5000 for Redash
EXPOSE 5000

# Set the user back to default if necessary (usually 'redash')
USER redash

