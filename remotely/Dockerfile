FROM alpine:latest AS build-env

WORKDIR /build

RUN \
    apk --update --no-cache add openssl wget unzip && \
    wget -q https://github.com/lucent-sea/Remotely/releases/latest/download/Remotely_Server_Linux-x64.zip && \
    unzip -o Remotely_Server_Linux-x64.zip && \
    rm Remotely_Server_Linux-x64.zip && \
    rm -r wwwroot/Downloads/*  
    
RUN chmod u+x,o+x .

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
ENV APP_USER=remotely
ENV APP_DIR="/$APP_USER"
WORKDIR $APP_DIR

RUN adduser \
  --disabled-password \
  --home /$APP_USER \
  --gecos '' $APP_USER && \
  apt-get update && \
  apt-get upgrade && \
  apt-get clean

COPY --from=build-env --chown=$APP_USER:$APP_USER /build .

ENV ASPNETCORE_ENVIRONMENT="Production"
ENV ASPNETCORE_URLS http://*:5000
EXPOSE 5000/tcp

USER $APP_USER
VOLUME $APP_DIR
CMD tail -f /dev/null
ENTRYPOINT ["dotnet", "Remotely_Server.dll"]