#   Licensed to the Apache Software Foundation (ASF) under one or more
#   contributor license agreements.  See the NOTICE file distributed with
#   this work for additional information regarding copyright ownership.
#   The ASF licenses this file to You under the Apache License, Version 2.0
#   (the "License"); you may not use this file except in compliance with
#   the License.  You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

FROM eclipse-temurin:23-jre-alpine

LABEL org.opencontainers.image.authors="Stian Soiland-Reyes <stain@apache.org>"

ENV LANG C.UTF-8

RUN set -eux; \
	apk -U upgrade; \
	apk add bash curl ca-certificates findutils coreutils procps; \
	rm -rf /var/cache/apk/*



# Update below according to https://jena.apache.org/download/
# and checksum for apache-jena-4.x.x.tar.gz.sha512
ENV JENA_SHA512 9a66044573ca269c0a9a1191fe7a8e1925f2d77e2e0bdadddd68616a1bafed1d9819c0b2988dd03614ec778a3882ccb091d825976fc0837406916f9f001b701c
ENV JENA_VERSION 5.2.0
ENV ASF_MIRROR https://www.apache.org/dyn/mirrors/mirrors.cgi?action=download&filename=
ENV ASF_ARCHIVE https://archive.apache.org/dist/

LABEL org.opencontainers.image.url https://github.com/stain/jena-docker/tree/master/jena
LABEL org.opencontainers.image.source https://github.com/stain/jena-docker/
LABEL org.opencontainers.image.documentation https://jena.apache.org/documentation/tools/
LABEL org.opencontainers.image.title "Apache Jena"
LABEL org.opencontainers.image.description "Jena riot is a command line tool for parsing and validating multiple RDF formats"
LABEL org.opencontainers.image.version ${FUSEKI_VERSION}
LABEL org.opencontainers.image.licenses "(Apache-2.0 AND (GPL-2.0 WITH Classpath-exception-2.0) AND GPL-3.0)"
LABEL org.opencontainers.image.authors "Apache Jena by https://jena.apache.org/; this image by https://orcid.org/0000-0001-9842-9718"


WORKDIR /tmp
# sha512 checksum
RUN echo "$JENA_SHA512  jena.tar.gz" > jena.tar.gz.sha512
# Download/check/unpack/move in one go (to reduce image size)
RUN     (curl --location --silent --show-error --fail --retry-connrefused --retry 3 --output jena.tar.gz ${ASF_MIRROR}jena/binaries/apache-jena-$JENA_VERSION.tar.gz || \
	curl --fail --silent --show-error --retry-connrefused --retry 3 --output jena.tar.gz $ASF_ARCHIVE/jena/binaries/apache-jena-$JENA_VERSION.tar.gz) && \
	sha512sum -c jena.tar.gz.sha512 && \
	tar zxf jena.tar.gz && \
	mv apache-jena* /jena && \
	rm jena.tar.gz* && \
	cd /jena && rm -rf *javadoc* *src* bat

# Add to PATH
ENV PATH $PATH:/jena/bin
# Check it works
RUN riot  --version

# Default dir /rdf, can be used with
# --volume
RUN mkdir /rdf
WORKDIR /rdf
#VOLUME /rdf
CMD ["/jena/bin/riot"]
