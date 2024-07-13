---
title: Maven 配置多个远程仓库
date: 2024-06-28 08:37:05
tags:
    - "Env"
    - "Maven"
    - "Java"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/maven.jpg"
excerpt: "Maven 私有远程仓库与中央远程仓库集成"
---
配置Maven Settings.xml文件

## mirrorOf 将 * 改为 central
```xml
    <!-- 原来的 http://repo1.maven.org  -->
    <mirror>
      <id>mavenRepository3</id>
      <mirrorOf>central</mirrorOf>
      <name>mavenRepository3</name>
      <url>https://repo.maven.apache.org/maven2/</url>
    </mirror>
```

## 调整 profiles activeProfiles

```xml
  <profiles>
    <profile>
      <id>private-repository</id> 
      <repositories>
        <repository>
          <id>kaadas</id> 
          <url>http://private-repository/repository/maven-snapshots/</url> 
          <releases>
            <enabled>false</enabled>
          </releases> 
          <snapshots>
            <enabled>true</enabled> 
            <!-- <updatePolicy>always</updatePolicy> -->
          </snapshots>
        </repository>
      </repositories>
    </profile>
    <profile>
      <id>maven-central</id> 
      <repositories>
        <repository>
          <id>maven-central</id> 
          <url>https://repo.maven.apache.org/maven2/</url> 
          <releases>
            <enabled>true</enabled>
          </releases> 
          <snapshots>
            <enabled>false</enabled> 
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>private-repository</activeProfile>
    <activeProfile>maven-central</activeProfile>
  </activeProfiles>
```