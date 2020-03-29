FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive
ENV HOME="/config"

SHELL ["/bin/bash", "-c"]

RUN \
 apt-get update && \
 apt-get install -y wget && \
 apt-get install -y --no-install-recommends apt-utils 2>&1 | grep -v "debconf: delaying package configuration, since apt-utils is not installed" && \
 wget -q "https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb" && \
 dpkg -i packages-microsoft-prod.deb && \
 apt-get update && \
 apt-get install -y \
   software-properties-common \
   apt-transport-https \
   aspnetcore-runtime-3.1 \
   unzip \
   acl \
   ffmpeg \
   libc6-dev \
   libgdiplus && \
mkdir -p /var/www/remotely && \
mkdir -p /var/www/remotely/db && \
wget -q https://github.com/Jay-Rad/Remotely/releases/latest/download/Remotely_Server_Linux-x64.zip && \
unzip -o Remotely_Server_Linux-x64.zip -d /var/www/remotely && \
wget https://raw.githubusercontent.com/fanuelsen/Docker/master/remotely/appsettings.Production.json -P /var/www/remotely/db && \
setfacl -R -m u:www-data:rwx /var/www/remotely && \
chown -R www-data:www-data /var/www/remotely

EXPOSE 5000

ENV ASPNETCORE_URLS="http://*:5000"
ENV ASPNETCORE_ENVIRONMENT="Production"

WORKDIR /var/www/remotely
CMD ["/usr/bin/dotnet", "Remotely_Server.dll"]
