# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb3-no-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv
    cf bind-service ad-sb3-no-cfenv  mysql-service
    

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-12T14:31:35.69-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-12T14:31:35.97-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.1s)
   2023-09-12T14:31:36.90-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (0.9s)
   2023-09-12T14:31:38.72-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.8s)
   2023-09-12T14:31:38.72-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-12T14:31:39.56-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.8s)
   2023-09-12T14:31:40.80-0400 [STG/0] OUT Loaded Classes: 28514, Threads: 250
   2023-09-12T14:31:40.89-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.0s)
   2023-09-12T14:31:41.05-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-09-12T14:31:41.16-0400 [STG/0] OUT -----> Downloading Java Cf Env 3.1.0 from https://java-buildpack.cloudfoundry.org/java-cfenv/java-cfenv-3.1.0.jar (0.1s)
   2023-09-12T14:32:00.86-0400 [STG/0] OUT Exit status 0


```

App log:
```
   2023-09-12T14:32:22.94-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.2)
   2023-09-12T14:32:23.14-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:23.123Z  INFO 13 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 13 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-12T14:32:23.14-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:23.143Z  INFO 13 --- [           main] o.c.samples.music.Application            : The following 1 profile is active: "http2"
   2023-09-12T14:32:23.26-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:23.263Z  INFO 13 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-09-12T14:32:25.66-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:25.669Z  INFO 13 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-12T14:32:25.67-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:25.672Z  INFO 13 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-12T14:32:25.95-0400 [APP/PROC/WEB/0] OUT 2023-09-12T18:32:25.957Z  INFO 13 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 272 ms. Found 1 JPA repository interfaces.

```

Actuator (/actuator/env):

```json
{
  "activeProfiles": [
    "http2"
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
          "value": "jdbc:mysql://a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?user=f06b62641ba1438a9a53925785b215a3&password=zsi2zokbwfyw9iy6&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "f06b62641ba1438a9a53925785b215a3"
        },
        "spring.datasource.password": {
          "value": "zsi2zokbwfyw9iy6"
        },
        "spring.datasource.driver-class-name": {
          "value": "com.mysql.cj.jdbc.Driver"
        }
      }
    },
```

# Results interpretation:

Java-cfenv did not add a `cloud` profile. The `mysql` profile was not activated (because the app code that set it was commented out), but since `java-cfenv` detected the Mysql-service, it autoconfigured the mysql datasource by itself.

# SAR disabled, JBP_CONFIG_SPRING_AUTO_RECONFIGURATION: '{enabled: false}'

SAR was not downloaded even though we did not disable it, meaning Java buildpacks did not inject it (as seen in the logs)

# SAR not explicitly disabled

same