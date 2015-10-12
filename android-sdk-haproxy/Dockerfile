FROM misumirize/android-sdk

MAINTAINER Rize MISUMI <r@ayase-e.li>

RUN apt-get update && \
    apt-get install -y haproxy && \
    apt-get clean

ONBUILD COPY haproxy.cfg /etc/haproxy/haproxy.cfg

ENV PROJECT /project
RUN mkdir $PROJECT
WORKDIR $PROJECT
ONBUILD COPY . $PROJECT

RUN echo "sdk.dir=$ANDROID_HOME" > local.properties && \
    wget https://raw.githubusercontent.com/travis-ci/travis-cookbooks/master/community-cookbooks/android-sdk/files/default/android-wait-for-emulator && \
    chmod +x android-wait-for-emulator

CMD /etc/init.d/haproxy start && ./android-wait-for-emulator && ./gradlew connectedAndroidTest
