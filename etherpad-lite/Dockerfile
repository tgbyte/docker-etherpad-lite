FROM node:10
MAINTAINER Tony Motakis <tvelocity@gmail.com>

ENV ETHERPAD_VERSION develop
ENV NODE_ENV production

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    curl unzip mysql-client node-pg postgresql-client && \
    rm -r /var/lib/apt/lists/*

WORKDIR /opt/

RUN curl -SL \
    https://github.com/tgbyte/etherpad-lite/archive/${ETHERPAD_VERSION}.zip \
    > etherpad.zip && unzip etherpad && rm etherpad.zip && \
    mv etherpad-lite-${ETHERPAD_VERSION} etherpad-lite

WORKDIR etherpad-lite

RUN bin/installDeps.sh && rm settings.json
COPY entrypoint.sh /entrypoint.sh

RUN sed -i 's/^node/exec\ node/' bin/run.sh

# OpenShift runs containers as non-root
RUN chmod g+rwX,o+rwX -R .

VOLUME /opt/etherpad-lite/var
RUN ln -s var/settings.json settings.json

EXPOSE 9001
ENTRYPOINT ["/entrypoint.sh"]
CMD ["bin/run.sh", "--root"]
