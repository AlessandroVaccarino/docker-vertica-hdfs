FROM alessandrov87/docker-vertica:9.1.0-0
LABEL  maintainer="alessandrov87 (https://github.com/AlessandroVaccarino)"

ENV HADOOP_VERSION 2.9.0

# Replace bootstrap with new version
RUN rm /etc/bootstrap.sh
COPY bootstrap.sh /etc/
RUN chown root:root /etc/bootstrap.sh
RUN chmod 700 /etc/bootstrap.sh

# Create HDFS folder
RUN mkdir -p /srv/hdfs/datanode && \
    mkdir -p /srv/hdfs/namenode

# Install SSH Server
RUN apt-get update && \
    apt-get install openssh-server

# Intall Java
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y  software-properties-common && \
    add-apt-repository ppa:webupd8team/java -y && \
    apt-get update && \
    echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get install -y oracle-java8-installer && \
    apt-get clean

# Download and extract Hadoop
RUN wget http://it.apache.contactlab.it/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz && \
    tar -xzvf hadoop-${HADOOP_VERSION}.tar.gz && \
    mv hadoop-2.9.0 /usr/local/hadoop && \
    rm /hadoop-${HADOOP_VERSION}.tar.gz

# Replace Hadoop config
RUN rm /usr/local/hadoop/etc/hadoop/hadoop-env.sh && \
    rm /usr/local/hadoop/etc/hadoop/core-site.xml && \
    rm /usr/local/hadoop/etc/hadoop/hdfs-site.xml
COPY conf/hadoop-env.sh /usr/local/hadoop/etc/hadoop/
COPY conf/core-site.xml /usr/local/hadoop/etc/hadoop/
COPY conf/hdfs-site.xml /usr/local/hadoop/etc/hadoop/

# ssh without key
RUN ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
# Add localhost and 0.0.0.0 to known hosts
RUN ssh-keyscan -H 127.0.0.1 >> ~/.ssh/known_hosts
RUN ssh-keyscan -H 0.0.0.0 >> ~/.ssh/known_hosts

# format namenode
RUN /usr/local/hadoop/bin/hdfs namenode -format

CMD [ "sh", "-c", "service ssh start; bash"]

# Expose ports
# Vertica Ports
EXPOSE 5433
EXPOSE 5450
# Hadoop Ports
EXPOSE 9000
EXPOSE 50070
EXPOSE 50075
EXPOSE 50090
EXPOSE 50105
