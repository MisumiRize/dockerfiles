FROM misumirize/android-sdk

RUN apt-get update && \
    apt-get install -y haproxy && \
    apt-get clean && \
    rm /etc/haproxy/haproxy.cfg

ONBUILD COPY haproxy.cfg /etc/haproxy/haproxy.cfg

ENV PROJECT /project
RUN mkdir $PROJECT
WORKDIR $PROJECT
ONBUILD COPY . $PROJECT

ADD https://raw.githubusercontent.com/travis-ci/travis-cookbooks/master/community-cookbooks/android-sdk/files/default/android-wait-for-emulator /usr/local/bin/android-wait-for-emulator
COPY android-send-menu-key /usr/local/bin/android-send-menu-key
RUN echo "sdk.dir=$ANDROID_HOME" > local.properties && \
    chmod +x /usr/local/bin/android-wait-for-emulator && \
    chmod +x /usr/local/bin/android-send-menu-key

ENTRYPOINT /etc/init.d/haproxy start && android-wait-for-emulator && android-send-menu-key && ./gradlew
