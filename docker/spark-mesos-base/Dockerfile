FROM nexusjpl/nexusbase

MAINTAINER Joseph Jacob "Joseph.Jacob@jpl.nasa.gov"

# Set environment variables.  For Mesos, I used MESOS_VER because MESOS_VERSION
# is expected to be a logical TRUE/FALSE flag that tells Mesos whether or not 
# to simply print the version number and exit.

ENV INSTALL_LOC=/usr/local \
    HADOOP_VERSION=2.7.3 \
    SPARK_VERSION=2.1.0 \
    MESOS_VER=1.2.0 \
    MESOS_PORT=5050 \
    MESOS_WORKDIR=/var/lib/mesos \
    MESOS_IP=127.0.0.1

ENV HADOOP_PACKAGE=hadoop-${HADOOP_VERSION}.tar.gz \
    SPARK_DIR=spark-${SPARK_VERSION} \
    SPARK_PACKAGE=spark-${SPARK_VERSION}-bin-without-hadoop \
    MESOS_MASTER=mesos://${MESOS_IP}:${MESOS_PORT} \
    MESOS_PACKAGE=mesos-${MESOS_VER}.tar.gz

ENV HADOOP_HOME=${INSTALL_LOC}/hadoop-${HADOOP_VERSION} \
    SPARK_HOME=${INSTALL_LOC}/${SPARK_DIR} \
    MESOS_HOME=${INSTALL_LOC}/mesos-${MESOS_VER}
    
WORKDIR ${INSTALL_LOC}

# Set up Hadoop

RUN wget --quiet http://archive.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/${HADOOP_PACKAGE} && \
        tar -xzf ${HADOOP_PACKAGE} -C ${INSTALL_LOC} && \
	rm -f ${HADOOP_PACKAGE}

# Set up Spark

RUN wget --quiet http://d3kbcqa49mib13.cloudfront.net/${SPARK_PACKAGE}.tgz && \
    tar -xzf ${SPARK_PACKAGE}.tgz && \
    chown -R root.root ${SPARK_PACKAGE} && \
    ln -s ${SPARK_PACKAGE} ${SPARK_DIR}

# RUN wget --quiet http://d3kbcqa49mib13.cloudfront.net/${SPARK_PACKAGE}.tgz && \
#     tar -xzf ${SPARK_PACKAGE}.tgz -C ${INSTALL_LOC} && \
#     cd ${INSTALL_LOC} && chown -R root.root ${SPARK_PACKAGE} && \
#     ln -s ${SPARK_PACKAGE} ${SPARK_DIR} && \
#     rm -f ${SPARK_PACKAGE}.tgz

# Set up Mesos

COPY install_mesos.sh .

RUN source ./install_mesos.sh && \
    mkdir ${MESOS_WORKDIR}

CMD ["/bin/bash"]