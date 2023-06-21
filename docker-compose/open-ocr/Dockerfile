FROM ubuntu:20.04

ENV GOPATH /opt/go
# tzdata is interactive, make it non-interactive
ENV DEBIAN_FRONTEND=noninteractive

# Get dependencies
#RUN apt-get clean && rm -rf /var/lib/apt/lists/*
#RUN sed -i -e 's/http:\/\/archive/mirror:\/\/mirrors/' -e 's/\/ubuntu\//\/mirrors.txt/' /etc/apt/sources.list
RUN apt-get update
#add tesseract repo for v4, see https://ubuntuhandbook.org/index.php/2021/12/install-tesseract-ocr-5-ubuntu/
#RUN add-apt-repository -y ppa:alex-p/tesseract-ocr

RUN apt-get install -y \
  git \
  golang \
  gcc \
  wget \
  unzip

# Get tesseract-ocr packages
RUN apt-get install -y \
  libleptonica-dev \
  #libtesseract3 \
  libtesseract-dev \
  tesseract-ocr

# Get language data.
RUN apt-get install -y \
  tesseract-ocr-deu \
  tesseract-ocr-fra \
  tesseract-ocr-nld \
  tesseract-ocr-eng

# In theory, should be able to set export TESSDATA_PREFIX=/usr/share/tesseract-ocr/,
# but when I tried I still got error: Error opening data file /usr/local/share/tessdata/eng.traineddata
# Workaround: just copy the data to where it expects
#RUN mkdir -p /usr/local/share/tessdata/ && \
#    cp -R /usr/share/tesseract-ocr/tessdata/* /usr/local/share/tessdata/

RUN mkdir -p $GOPATH

# go get open-ocr
RUN go get -u -v -t github.com/bartengine27/open-ocr/

# build open-ocr-httpd binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/bartengine27/open-ocr/cli-httpd && go build -v -o open-ocr-httpd && cp open-ocr-httpd /usr/bin

# build open-ocr-worker binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/bartengine27/open-ocr/cli-worker && go build -v -o open-ocr-worker && cp open-ocr-worker /usr/bin
