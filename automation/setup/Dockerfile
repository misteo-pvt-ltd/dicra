FROM apache/airflow:2.5.1

USER root

# Install system dependencies with newer GDAL from Debian backports
RUN apt-get update && \
    apt-get install -y \
        build-essential \
        software-properties-common && \
    echo "deb http://deb.debian.org/debian bullseye-backports main" > /etc/apt/sources.list.d/backports.list && \
    apt-get update && \
    apt-get install -y \
        -t bullseye-backports \
        gdal-bin \
        libgdal-dev \
        python3-gdal

# Set GDAL environment variables
ENV CPLUS_INCLUDE_PATH=/usr/include/gdal
ENV C_INCLUDE_PATH=/usr/include/gdal

USER airflow

# Install Python GDAL bindings matching system version
RUN pip3 install --no-cache-dir "GDAL==$(gdal-config --version)"

# Copy and install other requirements
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt
