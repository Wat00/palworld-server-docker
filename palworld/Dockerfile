FROM cm2network/steamcmd AS steamcmd-mkdir
ARG SAVED_DIR
RUN mkdir -p $SAVED_DIR

FROM steamcmd-mkdir AS steamcmd
ENTRYPOINT ["./steamcmd.sh"]
CMD ["+login", "anonymous", "+app_update", "2394010", "validate", "+quit"]

FROM steamcmd-mkdir AS palserver
ARG PALSERVER_DIR
WORKDIR $PALSERVER_DIR
ENTRYPOINT ["./PalServer.sh"]
ARG RCON_PORT
EXPOSE $RCON_PORT