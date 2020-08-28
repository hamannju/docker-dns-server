FROM coredns/coredns:latest
LABEL maintainer="hamann.julian@spedition-hamann.de"
VOLUME ["/etc/coredns"]
VOLUME ["/etc/coredns/zones"]
ENTRYPOINT ["/coredns"]
CMD ["-conf","/etc/coredns/Corefile"]
EXPOSE 53 53/udp