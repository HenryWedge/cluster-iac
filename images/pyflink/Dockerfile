FROM flink:2.2.0-java21

USER root

RUN apt-get update -y && \
    apt-get -y install software-properties-common && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get install -y python3.10 python3.10-dev build-essential && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt /opt/flink/requirements.txt
RUN ln -s /usr/bin/python3.10 /usr/bin/python

RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3.10 get-pip.py && \
    rm get-pip.py

RUN python -m pip install --break-system-packages --upgrade setuptools

RUN pip install --no-cache-dir --break-system-packages --ignore-installed -r /opt/flink/requirements.txt

USER flink

WORKDIR /opt/flink/lib
RUN wget https://repo.maven.apache.org/maven2/org/apache/flink/flink-connector-kafka/4.0.1-2.0/flink-connector-kafka-4.0.1-2.0.jar
RUN wget https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-kafka/4.0.1-2.0/flink-sql-connector-kafka-4.0.1-2.0.jar
RUN wget https://repo.maven.apache.org/maven2/org/apache/kafka/kafka-clients/4.0.2/kafka-clients-4.0.2.jar

WORKDIR /opt/flink/usrlib
COPY image-classification-pipeline.py .