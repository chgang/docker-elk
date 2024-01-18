ARG ELASTIC_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/elasticsearch/elasticsearch:7.9.3

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu
RUN set -x \
    && yum update -y \
    && yum install -y iputils nc vim libcurl telnet iputils-ping net-tools unzip openssl \
    && yum clean all
ADD ./elasticsearch-analysis-ik-7.9.3.zip /usr/share/elasticsearch/plugins/ik/
ADD ./elasticsearch-analysis-pinyin-7.9.3.zip /usr/share/elasticsearch/plugins/pinyin/

RUN unzip /usr/share/elasticsearch/plugins/ik/elasticsearch-analysis-ik-7.9.3.zip -d /usr/share/elasticsearch/plugins/ik/ \
    && unzip /usr/share/elasticsearch/plugins/pinyin/elasticsearch-analysis-pinyin-7.9.3.zip -d /usr/share/elasticsearch/plugins/pinyin/
ADD ./config/java.policy /usr/share/elasticsearch/jdk/conf/security