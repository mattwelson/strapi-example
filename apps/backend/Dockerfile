# Docker file to be built from the root of the turbo repo like: docker build -t nextapp:latest -f apps/frontend/Dockerfile .
# reference: https://turbo.build/repo/docs/guides/tools/docker
FROM node:20-slim AS base
# Installing libvips-dev for sharp Compatability
RUN apt-get update && apt-get install libvips-dev -y
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable

WORKDIR /repo

# Copy root package.json and lockfile
COPY package.json ./
COPY pnpm-workspace.yaml ./
COPY pnpm-lock.yaml ./

# Copy the backend package.json
COPY apps/backend/package.json ./apps/backend/package.json

# deps
FROM base AS prod-deps
WORKDIR /repo
RUN pnpm install --prod --frozen-lockfile

# build
FROM base AS builder
WORKDIR /repo
COPY --from=prod-deps /repo/node_modules ./node_modules
COPY --from=prod-deps /repo/apps/backend/node_modules ./apps/backend/node_modules
COPY . .
RUN pnpm turbo run build --filter backend

# runner
FROM base as runner
WORKDIR /repo

COPY --from=builder /repo ./

WORKDIR /repo/apps/backend

EXPOSE 1337
CMD ["pnpm", "start"]
