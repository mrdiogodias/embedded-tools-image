# syntax = edrevo/dockerfile-plus

FROM debian:bullseye-slim

INCLUDE+ Common/Compilers.dock
INCLUDE+ Common/StaticAnalysis.dock

WORKDIR "/home"
RUN rm -rf /tmp/*

CMD ["/bin/bash"]


