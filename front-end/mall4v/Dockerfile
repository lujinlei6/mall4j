# ---------- 构建阶段 ----------
FROM node:22-slim AS builder
WORKDIR /app

# 启用 npm 内置的 corepack，以便使用 pnpm
RUN corepack enable

# 先拷贝依赖清单单独安装,利用 Docker 层缓存
COPY package.json .npmrc ./
RUN pnpm install --config.dangerouslyAllowAllBuilds=true

# 拷贝源码并构建生产产物(输出到 dist/)
COPY . .
RUN pnpm build

# ---------- 运行阶段 ----------
FROM nginx:1.24-alpine
RUN rm -f /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html/dist
COPY nginx.conf /etc/nginx/conf.d
EXPOSE 80