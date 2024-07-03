FROM oven/bun:1.1 as main
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}
WORKDIR /opt/
COPY ./package.json ./bun.lockb ./
ENV PATH /opt/node_modules/.bin:$PATH
RUN bun install
WORKDIR /opt/app
COPY ./ .
RUN bun run build
EXPOSE 3000
# TODO: Add a bun run start version of this for "production" builds
CMD ["bun", "run", "develop"]
