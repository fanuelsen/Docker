FROM ubuntu:18.04
MAINTAINER Per-Ole Fanuelsen

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
wget -q https://github.com/lucent-sea/Remotely/releases/latest/download/Remotely_Server_Linux-x64.zip && \
unzip -o Remotely_Server_Linux-x64.zip -d /var/www/remotely && \
rm -rf /var/www/remotely/wwwroot/lib/jquery \
  /var/www/remotely/wwwroot/lib/jquery-validation \
  /var/www/remotely/wwwroot/lib/jquery-validation-unobtrusive \
  rm Remotely_Server_Linux-x64.zip \
  rm packages-microsoft-prod.deb \
  rm -rf /var/lib/apt/lists/* && \
cp -ar /var/www/remotely/wwwroot/Identity/lib/jquery /var/www/remotely/wwwroot/lib/jquery && \
cp -ar /var/www/remotely/wwwroot/Identity/lib/jquery-validation /var/www/remotely/wwwroot/lib/jquery-validation && \
cp -ar /var/www/remotely/wwwroot/Identity/lib/jquery-validation-unobtrusive /var/www/remotely/wwwroot/lib/jquery-validation-unobtrusive &&\
setfacl -R -m u:www-data:rwx /var/www/remotely && \
chown -R www-data:www-data /var/www/remotely

EXPOSE 5000

ENV ASPNETCORE_URLS="http://*:5000"
ENV ASPNETCORE_ENVIRONMENT="Production"

WORKDIR /var/www/remotely
CMD ["/usr/bin/dotnet", "Remotely_Server.dll"]