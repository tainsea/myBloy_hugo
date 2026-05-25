---
date: '2026-05-25T22:03:07+08:00'
draft: false
title: 'SpringBoot项目application常用配置'
---

```
# ===================================================================
# Spring Boot application.yml 常用配置模板
# ===================================================================

server:
  port: 8080                        # 服务端口
  servlet:
    context-path: /api              # 全局接口前缀
  tomcat:
    uri-encoding: UTF-8
    threads:
      max: 200                      # 最大线程数
      min-spare: 10                 # 最小空闲线程数
    connection-timeout: 5000        # 连接超时（毫秒）

# -------------------------------------------------------------------
# Spring 核心配置
# -------------------------------------------------------------------
spring:
  application:
    name: my-spring-boot-app        # 应用名称（注册中心/链路追踪会用到）

  profiles:
    active: dev                     # 激活的环境配置（dev / test / prod）

  # ── 数据源（MySQL）──────────────────────────────────────────────
  datasource:
    url: jdbc:mysql://localhost:3306/mydb?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
    username: root
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.zaxxer.hikari.HikariDataSource
    hikari:
      minimum-idle: 5               # 最小空闲连接
      maximum-pool-size: 20         # 最大连接池大小
      idle-timeout: 600000          # 空闲连接超时（ms）
      max-lifetime: 1800000         # 连接最大存活时间（ms）
      connection-timeout: 30000     # 获取连接超时（ms）
      connection-test-query: SELECT 1

  # ── Redis ───────────────────────────────────────────────────────
  data:
    redis:
      host: 127.0.0.1
      port: 6379
      password:                     # 无密码留空
      database: 0
      timeout: 3000ms
      lettuce:
        pool:
          max-active: 8
          max-idle: 8
          min-idle: 0
          max-wait: -1ms

  # ── JPA / Hibernate ─────────────────────────────────────────────
  jpa:
    hibernate:
      ddl-auto: update              # none / validate / update / create / create-drop
    show-sql: true
    open-in-view: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
        format_sql: true

  # ── MyBatis-Plus（若使用 MP 替代 JPA）──────────────────────────
  # mybatis-plus 配置见下方独立块

  # ── 文件上传 ────────────────────────────────────────────────────
  servlet:
    multipart:
      enabled: true
      max-file-size: 50MB
      max-request-size: 100MB
      location: /tmp

  # ── 邮件发送 ────────────────────────────────────────────────────
  mail:
    host: smtp.example.com
    port: 465
    username: no-reply@example.com
    password: your_email_password
    protocol: smtp
    default-encoding: UTF-8
    properties:
      mail:
        smtp:
          auth: true
          ssl:
            enable: true

  # ── Jackson（JSON 序列化）───────────────────────────────────────
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: Asia/Shanghai
    default-property-inclusion: non_null   # 忽略 null 字段
    serialization:
      write-dates-as-timestamps: false

  # ── 国际化 ──────────────────────────────────────────────────────
  messages:
    basename: i18n/messages
    encoding: UTF-8

# -------------------------------------------------------------------
# MyBatis-Plus 配置
# -------------------------------------------------------------------
mybatis-plus:
  mapper-locations: classpath*:mapper/**/*.xml
  type-aliases-package: com.example.entity
  configuration:
    map-underscore-to-camel-case: true    # 驼峰映射
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: ASSIGN_ID                  # 主键策略：雪花ID
      logic-delete-field: deleted         # 逻辑删除字段名
      logic-delete-value: 1
      logic-not-delete-value: 0

# -------------------------------------------------------------------
# 日志配置
# -------------------------------------------------------------------
logging:
  level:
    root: INFO
    com.example: DEBUG                    # 指定包的日志级别
    org.springframework.web: DEBUG
    org.hibernate.SQL: DEBUG
  file:
    name: logs/app.log                    # 日志输出文件
  logback:
    rollingpolicy:
      max-file-size: 50MB                 # 单个日志文件最大大小
      max-history: 30                     # 保留天数
      total-size-cap: 2GB

# -------------------------------------------------------------------
# Actuator 监控端点
# -------------------------------------------------------------------
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,env  # 暴露的端点
      base-path: /actuator
  endpoint:
    health:
      show-details: always
  info:
    env:
      enabled: true

# -------------------------------------------------------------------
# SpringDoc / Swagger（OpenAPI 3）
# -------------------------------------------------------------------
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
    tags-sorter: alpha
    operations-sorter: alpha
  packages-to-scan: com.example.controller

# -------------------------------------------------------------------
# 自定义业务配置（示例）
# -------------------------------------------------------------------
app:
  jwt:
    secret: your-very-long-jwt-secret-key-here
    expiration: 86400000              # Token 有效期（ms），此处为 1 天
    refresh-expiration: 604800000     # 刷新 Token 有效期（ms），此处为 7 天
  oss:
    endpoint: https://oss-cn-hangzhou.aliyuncs.com
    access-key-id: your_access_key
    access-key-secret: your_secret
    bucket-name: my-bucket
  cors:
    allowed-origins: "*"
    allowed-methods: GET,POST,PUT,DELETE,OPTIONS

```

