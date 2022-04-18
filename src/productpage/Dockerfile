FROM python:3.7.7-slim

WORKDIR /opt/microservices
ADD . /opt/microservices

ARG flood_factor
ENV FLOOD_FACTOR ${flood_factor:-0}

RUN ["pip", "install", "--no-cache-dir", "-r", "requirements.txt"]
  
CMD ["python", "productpage.py", "9080"]
