# ---------- 构建阶段 ----------
# 从仓库根上下文中构建多模块工程中的 yami-shop-api 模块及其依赖(-am)
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /build
COPY . .
RUN mvn -B -DskipTests -pl yami-shop-api -am clean package

# ---------- 运行阶段 ----------
FROM eclipse-temurin:17-jre
WORKDIR /opt/projects/mall4j
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
COPY --from=builder /build/yami-shop-api/target/yami-shop-api-0.0.1-SNAPSHOT.jar ./
EXPOSE 8086
CMD ["java", "-Xms1024m", "-Xmx1024m", "-Xmn256m", "-Xss256k", "-XX:SurvivorRatio=8", "-Dspring.profiles.active=docker", "-jar", "yami-shop-api-0.0.1-SNAPSHOT.jar"]