FROM ruby:2.5-stretch

RUN apt-get update
RUN apt-get install -y apt-utils
RUN apt-get install -y openssh-server openssh-client

RUN apt-get update && \
	apt-get install -y openjdk-8-jdk && \
	apt-get install -y ant && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* && \
	rm -rf /var/cache/oracle-jdk8-installer;

RUN apt-get update && \
	apt-get install -y ca-certificates-java && \
	apt-get clean && \
	update-ca-certificates -f && \
	rm -rf /var/lib/apt/lists/* && \
	rm -rf /var/cache/oracle-jdk8-installer;

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME

RUN mkdir -p /var/run/sshd
COPY ./sshd_config /etc/ssh/sshd_config
COPY ./ssh_config /etc/ssh/ssh_config
COPY ./id_rsa.pub /root/.ssh/authorized_keys
COPY ./id_rsa /root/.ssh/id_rsa
COPY ./ssh_host_rsa_key /etc/ssh/ssh_host_rsa_key
COPY ./ssh_host_rsa_key /etc/ssh/ssh_host_rsa_key.pub
COPY ./ssh_host_rsa_key.pub /root/.ssh/known_hosts

CMD /usr/sbin/sshd -D