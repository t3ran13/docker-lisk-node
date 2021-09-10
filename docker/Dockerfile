FROM ubuntu:20.04

RUN apt-get update && apt-get install -y \
    curl \
    jq

#create lisk user/group and home folder in ubuntu
RUN addgroup --gid 1000 lisk \
    && useradd --uid 1000 --gid lisk --create-home --shell /bin/bash lisk

# Switch to lisk user
USER lisk

#install lisk core
RUN cd /home/lisk \
    && curl -o lisk-core.tar.gz https://downloads.lisk.io/lisk/mainnet/3.0.0/lisk-core-v3.0.0-linux-x64.tar.gz \
    && tar -xf ./lisk-core.tar.gz \
    && rm lisk-core.tar.gz \
    && mkdir -p /home/lisk/.lisk
ENV PATH="$PATH:/home/lisk/lisk-core/bin"

#setting up node config
RUN cp /home/lisk/lisk-core/config/mainnet/config.json /home/lisk/lisk-core/config/mainnet/custom-config.json
#FIXME
ENV CONSOLE_LOG_LEVEL=${CONSOLE_LOG_LEVEL:-info}
RUN content=$(jq ".logger.consoleLogLevel|=\"$CONSOLE_LOG_LEVEL\"" /home/lisk/lisk-core/config/mainnet/custom-config.json) && echo ${content} > /home/lisk/lisk-core/config/mainnet/custom-config.json
#FIXME
ENV FILE_LOG_LEVELE=${FILE_LOG_LEVELE:-error}
RUN content=$(jq ".logger.fileLogLevel|=\"$FILE_LOG_LEVELE\"" /home/lisk/lisk-core/config/mainnet/custom-config.json) && echo ${content} > /home/lisk/lisk-core/config/mainnet/custom-config.json



#RUN chown -R lisk:lisk /home/lisk

WORKDIR  /home/lisk

#VOLUME /home/lisk/.lisk
VOLUME /home/lisk/.lisk /home/lisk/config

EXPOSE 8000 8001

# Start node
CMD ["lisk-core", "start", "--network=mainnet", "--api-ipc", "--config=/home/lisk/lisk-core/config/mainnet/custom-config.json"]