FROM apache/airflow:2.9.0-python3.8
USER root
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       openjdk-17-jre-headless \
       libkrb5-dev \
       gcc \
       python3-dev \
    && apt-get autoremove -yqq --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
USER airflow
RUN pip install --no-cache-dir "apache-airflow==2.9.0" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.9.0/constraints-3.8.txt" \
    apache-airflow-providers-apache-spark \
    apache-airflow-providers-apache-hdfs
