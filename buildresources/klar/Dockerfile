FROM golang:1.10-alpine3.7
RUN wget https://github.com/optiopay/klar/releases/download/v2.0.2/klar-2.0.2-linux-amd64 -O /klar
RUN chmod +x /klar
ENTRYPOINT ["/klar"]
