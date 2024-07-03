FROM oven/bun:1.1 as main
# Installing libvips-dev for sharp Compatability
RUN apt-get update && apt-get install libvips-dev -y
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}
WORKDIR /opt/
COPY ./package.json ./bun.lockb ./
ENV PATH /opt/node_modules/.bin:$PATH
RUN bun install
WORKDIR /opt/app
COPY ./ .
# to run build 
RUN bun run build 
EXPOSE 1337
CMD ["bun", "run", "develop"]
