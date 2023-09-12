# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb3-with-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv
    cf bind-service ad-sb3-with-cfenv mysql-service

Meaning: with java-cfenv buildpack using `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-11T23:07:44.17-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-11T23:07:45.01-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/jammy/x86_64/jvmkill-1.17.0-RELEASE.so (0.2s)
   2023-09-11T23:07:45.69-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/jammy/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (0.6s)
   2023-09-11T23:07:47.46-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.7s)
   2023-09-11T23:07:47.46-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-11T23:07:47.64-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/jammy/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.1s)
   2023-09-11T23:07:48.75-0400 [STG/0] OUT Loaded Classes: 28514, Threads: 250
   2023-09-11T23:07:49.23-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.4s)
   2023-09-11T23:07:50.00-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.7s)
   2023-09-11T23:07:50.12-0400 [STG/0] OUT -----> Downloading Java Cf Env 3.1.0 from https://java-buildpack.cloudfoundry.org/java-cfenv/java-cfenv-3.1.0.jar (0.1s)
   2023-09-11T23:08:12.17-0400 [STG/0] OUT Exit status 0


```

App log:
```
   2023-09-11T23:25:34.67-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.3)
   2023-09-11T23:25:34.94-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:34.919Z  INFO 7 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 7 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-11T23:25:34.95-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:34.949Z  INFO 7 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "http2", "cloud"
   2023-09-11T23:25:35.13-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:35.130Z  INFO 7 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-09-11T23:25:38.07-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:38.071Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-11T23:25:38.09-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:38.094Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-11T23:25:38.60-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:25:38.600Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 467 ms. Found 1 JPA repository interfaces.

```

Actuator (/actuator/env):

```json
{
  "activeProfiles": [
    "http2",
    "cloud"
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
          "value": "jdbc:mysql://9791a9f3-4b85-4159-957a-857ec777a7cd.mysql.service.internal:3306/service_instance_db?user=c57b6ad659ad4944a74d351eb7524642&password=rk0eis5wirl5b3lo&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "c57b6ad659ad4944a74d351eb7524642"
        },
        "spring.datasource.password": {
          "value": "rk0eis5wirl5b3lo"
        },
        "spring.datasource.driver-class-name": {
          "value": "com.mysql.cj.jdbc.Driver"
        }
      }
    },

```

# Results interpretation:

Java-cfenv DID NOT add any profile; the `cloud` profile came from the manifest env. variables. The `mysql` profile was not activated (I disabled that in the app), but since `java-cfenv` detected the Mysql-service, it autoconfigured the mysql datasource by itself.

# SAR disabled impact

## SAR disabled  JBP_CONFIG_SPRING_AUTO_RECONFIGURATION: '{enabled: false}'
SAR was not downloaded, meaning Java buildpacks did not inject it (as seen in the logs)

## SAR not explicitly disabled
same