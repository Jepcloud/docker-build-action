FROM alpine:latest

LABEL maintainer="info@jepcloud.com"
LABEL description="Test Docker image for GitHub Action"

RUN apk add --no-cache curl

WORKDIR /app

COPY test/test-script.sh /app/

RUN chmod +x /app/test-script.sh

CMD ["/app/test-script.sh"]
