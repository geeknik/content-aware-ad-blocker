FROM ubuntu

RUN ln -fs /usr/share/zoneinfo/America/Chicago /etc/localtime

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
  apt-get install -yq expect \
    python3-pip python3-dev pandoc \
    libfreetype6-dev pkg-config \
    r-base-core libzmq3-dev libcurl4-openssl-dev \
    texlive-latex-base texlive-latex-extra texlive-fonts-recommended && \
  rm -rf /var/cache/apt && \
  dpkg-reconfigure --frontend noninteractive tzdata

#RUN pip3 install --upgrade pip

WORKDIR /usr/src/app
COPY requirements.txt /usr/src/app/
RUN pip3 install --no-cache-dir -r requirements.txt

COPY Lasagne-master.zip /usr/src/app/
RUN pip3 install --user --upgrade --no-deps Lasagne-master.zip

RUN pip3 install --upgrade --no-cache-dir https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.11.0rc0-cp27-none-linux_x86_64.whl

COPY rsetup.r /usr/src/app/
RUN cat /usr/src/app/rsetup.r | R --vanilla

EXPOSE 9123

CMD ["/bin/sh", "-c", "jupyter notebook --debug --notebook-dir=/usr/src/app/models --no-browser --port 9123 --ip=0.0.0.0"]
