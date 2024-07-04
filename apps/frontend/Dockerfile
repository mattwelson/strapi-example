# Docker file to be built from the root of the turbo repo like: docker build -t nextapp:latest -f apps/frontend/Dockerfile .
# reference: https://turbo.build/repo/docs/guides/tools/docker
FROM node:20-slim AS base
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable
#RUN apk add --no-cache libc6-compat

WORKDIR /repo

# Copy root package.json and lockfile
COPY package.json ./
COPY pnpm-workspace.yaml ./
COPY pnpm-lock.yaml ./

# Copy the frontend package.json
COPY apps/frontend/package.json ./apps/frontend/package.json

# deps
FROM base AS prod-deps
WORKDIR /repo
RUN pnpm install --prod --frozen-lockfile

# build
FROM base AS builder
WORKDIR /repo
COPY --from=prod-deps /repo/node_modules ./node_modules
COPY --from=prod-deps /repo/apps/frontend/node_modules ./apps/frontend/node_modules
COPY . .
RUN pnpm turbo run build --filter frontend

# runner
FROM base as runner
WORKDIR /repo

COPY --from=builder /repo/apps/frontend/public ./public

COPY --from=builder /repo/apps/frontend/.next/standalone ./

EXPOSE 3000
ENV PORT 3000
CMD ["node", "apps/frontend/server.js"]
