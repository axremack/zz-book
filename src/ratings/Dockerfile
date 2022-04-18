# Compléter ici
FROM node:12.8.1-slim

#Port
EXPOSE 9080

#Create app directory  
RUN mkdir -p /opt/microservices
WORKDIR /opt/microservices

#Install app dependencies
COPY package.json .
COPY ratings.js .
RUN npm install
RUN apt-get update \
&& apt-get install curl --no-install-recommends -y \
&& rm -rf /var/lib/apt/lists/*

#Variable d'environnement, valeur par défaut v1, éditable au docker build
ARG service_version
ENV SERVICE_VERSION ${service_version:-v1}

CMD ["node", "ratings.js", "9080"]