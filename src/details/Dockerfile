FROM ruby:2.7.1-slim

EXPOSE 9080

RUN mkdir /opt/microservices
WORKDIR /opt/microservices

COPY details.rb .

ARG service_version
ARG enable_external_book_service
ENV SERVICE_VERSION ${service_version:-v1}
ENV ENABLE_EXTERNAL_BOOK_SERVICE ${enable_external_book_service:-false}

CMD ["ruby", "details.rb", "9080"]