# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb2-with-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-12T14:24:12.85-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-12T14:24:13.13-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.1s)
   2023-09-12T14:24:15.16-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 1.8.0_382 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u382%2B6-linux-amd64.tar.gz (2.0s)
   2023-09-12T14:24:16.34-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.1s)
   2023-09-12T14:24:16.34-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-12T14:24:16.55-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.2s)
   2023-09-12T14:24:18.29-0400 [STG/0] OUT Loaded Classes: 21192, Threads: 250
   2023-09-12T14:24:19.14-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.8s)
   2023-09-12T14:24:19.29-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-09-12T14:24:34.16-0400 [STG/0] OUT Exit status 0

```

# app logs

```
   2023-09-12T14:24:55.32-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::               (v2.7.14)
   2023-09-12T14:24:55.50-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:55.496  INFO 25 --- [           main] .m.c.SpringApplicationContextInitializer : Found services mysql-service
   2023-09-12T14:24:55.50-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:55.500  INFO 25 --- [           main] .m.c.SpringApplicationContextInitializer : Setting service profile mysql
   2023-09-12T14:24:55.51-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:55.516  INFO 25 --- [           main] o.c.samples.music.Application            : Starting Application using Java 1.8.0_382 on d3187f76-b166-404f-433f-09d4 with PID 25 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-12T14:24:55.51-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:55.518  INFO 25 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "http2", "mysql"
   2023-09-12T14:24:55.67-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:55.671  INFO 25 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-09-12T14:24:57.97-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:57.973  INFO 25 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-12T14:24:57.97-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:24:57.978  INFO 25 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.

```

# actuator

```
{
  "activeProfiles": [
    "http2",
    "mysql"
  ],
  "propertySources": [
    {
      "name": "server.ports",
      "properties": {
        "local.server.port": {
          "value": 8080
        }
      }
    },
    {
      "name": "springMusicAutoConfig",
      "properties": {
        "spring.autoconfigure.exclude": {
          "value": "org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration"
        }
      }
    },
    {
      "name": "cfenvjdbc",
      "properties": {
        "spring.datasource.url": {
          "value": "jdbc:mysql://a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?user=464cf12c65e94b45a32c8b4e0020c909&password=plnth914ot8ks5da&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "464cf12c65e94b45a32c8b4e0020c909"
        },
        "spring.datasource.password": {
          "value": "plnth914ot8ks5da"
        },
        "spring.datasource.driver-class-name": {
          "value": "com.mysql.cj.jdbc.Driver"
        }
      }
    },
```

# Results interpretation:

SAR WAS NOT injected, NOR java-cfenv, since it was already in the classpath

This is the expected "old" behaviour


