# Run example: docker run -it --net=host -p 8083:8083 -e MASTER=mesos://127.0.0.1:5050 nexus-webapp

FROM nexusjpl/spark-mesos-base

MAINTAINER Joseph Jacob "Joseph.Jacob@jpl.nasa.gov"

# Set environment variables.

ENV MASTER=local[1] \
    PORT_NUM=8083 \
    SPARK_LOCAL_IP=nexus-webapp

# Run NEXUS webapp.

EXPOSE ${PORT_NUM}

WORKDIR /tmp

CMD python -m webservice.webapp
