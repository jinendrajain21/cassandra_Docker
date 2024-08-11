# Use the official Cassandra image as the base
FROM cassandra:latest

# Install Python3 and pip (if needed for other purposes)
RUN apt-get update && apt-get install -y python3 python3-pip

# Set the working directory
WORKDIR /etc/cassandra

# Copy the template cassandra.yaml file into the container
COPY cassandra.yaml /etc/cassandra/cassandra.yaml.template

# Copy the values.yaml file into the container
COPY values.yaml /etc/cassandra/values.yaml

# Replace placeholders in cassandra.yaml.template using sed
RUN while IFS= read -r line; do \
      key=$(echo "$line" | cut -d':' -f1 | xargs); \
      value=$(echo "$line" | cut -d':' -f2- | xargs); \
      sed -i "s|<$key>|$value|g" cassandra.yaml.template; \
    done < values.yaml

# Rename the file to cassandra.yaml
RUN mv /etc/cassandra/cassandra.yaml.template /etc/cassandra/cassandra.yaml

# Expose the necessary ports
EXPOSE 7000 7001 7199 9042 9160

# Start Cassandra
CMD ["cassandra", "-f"]

