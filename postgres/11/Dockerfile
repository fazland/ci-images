FROM postgres:11

RUN apt-get update && \
    apt-get install -y curl ca-certificates && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    apt-get install --no-install-recommends -y postgis postgresql-11-postgis-2.5 postgresql-11-postgis-2.5-scripts && \
    rm -rf /var/lib/apt/lists/*
