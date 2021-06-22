FROM ubuntu:21.10


WORKDIR /root/playbooks

RUN apt-get update && apt install -y software-properties-common && apt-add-repository --yes --update ppa:ansible/ansible 
RUN apt-get install -y \ 
    openssh-server \
    vim \
    python \
    net-tools \
    telnet \
    ansible \
    git \
    python3-pip

RUN pip install --upgrade pip
RUN mkdir /var/run/sshd
RUN echo 'root:ansible' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
COPY requirements.yml /root/playbooks/requirements.yml
RUN ansible-galaxy install -r requirements.yml

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]