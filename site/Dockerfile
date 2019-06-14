FROM ubuntu:latest

RUN apt-get clean 
RUN apt-get update
RUN apt-get install -y nginx
RUN apt-get install -y openssh-server
ADD nginx.conf /etc/nginx/nginx.conf

RUN mkdir -p /var/www/site
ADD index.html /var/www/site/index.html
RUN echo 'root:P@ssw0rd' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 80
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
CMD ["nginx", "-g", "daemon off;"]

