FROM bitnami/minideb:stretch
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-9" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages ca-certificates curl libbz2-1.0 libc6 libffi6 libgcc1 libncurses5 libnss-wrapper libreadline7 libsqlite3-0 libssl1.1 libtinfo5 locales procps unzip zlib1g
RUN . ./libcomponent.sh && component_unpack "python" "3.6.9-3" --checksum c4d4542cde9d0c0e5d0239491cdd91cacc68319679d9fab4088a994c9d40a7f6
RUN . ./libcomponent.sh && component_unpack "java" "1.8.232-0" --checksum f4e5ff1d4fb13baf9bb2a4f2e5ffb328cf9eeb26d0b91214196e4473c2874adc
RUN . ./libcomponent.sh && component_unpack "spark" "2.4.4-1" --checksum c1cd12976aea9700ffb19413f478946982bcd0e350b71729c914813dd0671440
RUN curl --silent -L https://github.com/tianon/gosu/releases/download/1.11/gosu-amd64 > /usr/local/bin/gosu && echo 0b843df6d86e270c5b0f5cbd3c326a04e18f4b7f9b8457fa497b0454c4b138d7 /usr/local/bin/gosu | sha256sum --check && chmod u+x /usr/local/bin/gosu && mkdir -p /opt/bitnami/licenses && curl --silent -L https://raw.githubusercontent.com/tianon/gosu/master/LICENSE > /opt/bitnami/licenses/gosu-1.11.txt
RUN /opt/bitnami/python/bin/python3 -m pip install pyspark pyarrow koalas azure-storage
RUN apt-get update
RUN apt-get install -y wget

COPY rootfs /
RUN /postunpack.sh
ENV BITNAMI_APP_NAME="spark" \
    BITNAMI_IMAGE_VERSION="2.4.4-debian-9-r68" \
    JAVA_HOME="/opt/bitnami/java" \
    NSS_WRAPPER_GROUP="/opt/bitnami/spark/tmp/nss_group" \
    NSS_WRAPPER_PASSWD="/opt/bitnami/spark/tmp/nss_passwd" \
    PATH="/opt/bitnami/python/bin:/opt/bitnami/java/bin:/opt/bitnami/spark/bin:/opt/bitnami/spark/sbin:$PATH" \
    SPARK_HOME="/opt/bitnami/spark"

RUN cd /opt/bitnami/python/lib/python3.6/site-packages/pyspark/jars
RUN wget -O /opt/bitnami/python/lib/python3.6/site-packages/pyspark/jars/azure-storage-8.4.0.jar http://central.maven.org/maven2/com/microsoft/azure/azure-storage/8.4.0/azure-storage-8.4.0.jar
RUN wget -O /opt/bitnami/python/lib/python3.6/site-packages/pyspark/jars/hadoop-azure-3.2.1.jar http://central.maven.org/maven2/org/apache/hadoop/hadoop-azure/3.2.1/hadoop-azure-3.2.1.jar
COPY data /opt/data



WORKDIR /opt/bitnami/spark
USER 1001
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "/run.sh" ]
