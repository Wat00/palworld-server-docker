FROM cm2network/steamcmd AS steamcmd-mkdir
ARG PALSERVER_DIR
RUN mkdir -p $PALSERVER_DIR

FROM steamcmd-mkdir AS steamcmd
ENTRYPOINT ["./steamcmd.sh"]
CMD ["+login", "anonymous", "+app_update", "2394010", "validate", "+quit"]

FROM steamcmd-mkdir AS palserver
WORKDIR $PALSERVER_DIR
ENTRYPOINT ["./PalServer.sh"]
ARG RCON_PORT
EXPOSE $RCON_PORT