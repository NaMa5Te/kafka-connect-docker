FROM confluentinc/cp-kafka-connect:3.2.0

WORKDIR /target

# Set environment variables
ENV KAFKA_VERSION=3.4.0
ENV KAFKA_URL=https://downloads.apache.org/kafka/${KAFKA_VERSION}/kafka-${KAFKA_VERSION}-src.tgz

# Create a directory for Kafka
RUN mkdir kafka

# Download Kafka and extract it to the Kafka directory
RUN wget -q -O - ${KAFKA_URL} | tar -xzf - --strip-components=1 -C kafka

# Download connector and extract it to the Kafka Connector directory
COPY my-connector kafka/connectors/my-connector

ENV CON_PATH=/hs-connector/kafka/connectors/kafka-redis-sink
RUN sed -i 's/^plugin\.path=$/plugin.path=\/hs-connector\/kafka\/connectors\/my-connector' /target/kafka/config/connect-standalone.properties

#RUN sed -i "s/plugin.path=.*/plugin.path=$CON_PATH/g" /target/kafka/config/connect-standalone.properties

VOLUME /target/config
VOLUME /target/offsets

# Copy your shell script to the container
COPY generateproperties.sh generateproperties.sh

# Set execute permission on your script
RUN chmod +x generateproperties.sh

CMD bash -c "/target/generateproperties.sh" && sh kafka/bin/connect-standalone.sh kafka/config/connect-standalone.properties /target/config/connector.properties
