# ---------- 构建阶段 ----------
FROM node:22-slim AS builder
WORKDIR /app

# 启用 npm 内置的 corepack，以便使用 pnpm
RUN corepack enable

# 先拷贝依赖清单单独安装,利用 Docker 层缓存
COPY package.json ./
RUN pnpm install

# 拷贝源码并构建 H5 产物(输出到 dist/build/h5)
COPY . .
RUN pnpm build:h5

# ---------- 运行阶段 ----------
FROM nginx:1.24-alpine
COPY --from=builder /app/dist/build/h5 /usr/share/nginx/html/h5
COPY nginx.conf /etc/nginx/conf.d
EXPOSE 80