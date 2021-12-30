FROM debian

LABEL app="MyGiropops"

ENV LINDO="Andre"

RUN apt update -y && apt install -y stress && apt-get clean

CMD "stress --vm 1 --vm-bytes 64M --cpu 0.5"
