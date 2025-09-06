FROM docker.io/library/alpine:edge AS git
ARG TAG
ENV TAG=${TAG:-main}
RUN apk update && apk add --no-cache \
	git
WORKDIR /
RUN git -c advice.detachedHead=false clone --branch $TAG --depth=1 --recurse-submodules https://github.com/pfm-powerforme/sillytavern.git source
WORKDIR /source/


FROM docker.io/library/node:lts-alpine AS runtime
ARG APP_HOME=/home/node/app
COPY --from=git /source/ /home/node/app/
RUN apk update && apk add --no-cache \
    gcompat tini git git-lfs
WORKDIR ${APP_HOME}
ENV NODE_ENV=production
RUN \
  echo "*** Install npm packages ***" && \
  npm i --no-audit --no-fund --loglevel=error --no-progress --omit=dev && npm cache clean --force
RUN \
  rm -f "config.yaml" || true && \
  ln -s "./config/config.yaml" "config.yaml" || true && \
  mkdir "config" || true
RUN \
  echo "*** Run Webpack ***" && \
  node "./docker/build-lib.js"
RUN \
  echo "*** Cleanup ***" && \
  mv "./docker/docker-entrypoint.sh" "./" && \
  rm -rf "./docker" && \
  echo "*** Make docker-entrypoint.sh executable ***" && \
  chmod +x "./docker-entrypoint.sh" && \
  echo "*** Convert line endings to Unix format ***" && \
  dos2unix "./docker-entrypoint.sh"
RUN git config --global --add safe.directory "*"
EXPOSE 8000
ENTRYPOINT ["tini", "--", "./docker-entrypoint.sh"]
