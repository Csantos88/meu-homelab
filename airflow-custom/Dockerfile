# 1. Usamos a versão 2.9.0 para empatar com seu banco de dados atual
FROM apache/airflow:2.9.0-python3.8

# 2. Mudamos para o usuário root para instalar dependências do sistema operacional
USER root

# 3. Instalamos o Java 17 E as dependências de compilação C/Kerberos para o HDFS
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       openjdk-17-jre-headless \
       libkrb5-dev \
       gcc \
       python3-dev \
    && apt-get autoremove -yqq --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Definimos a variável de ambiente do Java apontando para a versão 17
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

# 4. Voltamos para o usuário airflow para instalar pacotes Python
USER airflow

# 5. Instalamos os provedores usando as Constraints oficiais para não quebrar dependências
RUN pip install --no-cache-dir "apache-airflow==2.9.0" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.9.0/constraints-3.8.txt" \
    apache-airflow-providers-apache-spark \
    apache-airflow-providers-apache-hdfs
