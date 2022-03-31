FROM registry.access.redhat.com/ubi8/nodejs-16-minimal

USER 1001

WORKDIR /opt/app-root/src

RUN chown 1001 /opt/app-root/src \
    && chmod "g+rwX" /opt/app-root/src \
    && chown 1001:root /opt/app-root/src

COPY --chown=1001:root . /opt/app-root/src/

RUN 

USER 1001

RUN npm rebuild esbuild
RUN /opt/app-root/src/package.sh

EXPOSE 4200

CMD ["/opt/app-root/src/run.sh"]
