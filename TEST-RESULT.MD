# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb3-no-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv
    

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-11T23:01:21.51-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-11T23:01:21.97-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/jammy/x86_64/jvmkill-1.17.0-RELEASE.so (0.3s)
   2023-09-11T23:01:23.10-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/jammy/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (1.1s)
   2023-09-11T23:01:25.03-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.9s)
   2023-09-11T23:01:25.04-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-11T23:01:26.29-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/jammy/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (1.2s)
   2023-09-11T23:01:27.44-0400 [STG/0] OUT Loaded Classes: 28514, Threads: 250
   2023-09-11T23:01:27.59-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-09-11T23:01:27.87-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.2s)
   2023-09-11T23:01:28.06-0400 [STG/0] OUT -----> Downloading Java Cf Env 3.1.0 from https://java-buildpack.cloudfoundry.org/java-cfenv/java-cfenv-3.1.0.jar (0.1s)
   2023-09-11T23:01:50.53-0400 [STG/0] OUT Exit status 0


```

App log:
```
   2023-09-11T23:02:10.80-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.2)
   2023-09-11T23:02:10.91-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:10.907Z  INFO 21 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 21 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-11T23:02:10.91-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:10.912Z  INFO 21 --- [           main] o.c.samples.music.Application            : The following 1 profile is active: "http2"
   2023-09-11T23:02:10.99-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:10.995Z  INFO 21 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-09-11T23:02:13.04-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:13.045Z  INFO 21 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-11T23:02:13.04-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:13.048Z  INFO 21 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-11T23:02:13.28-0400 [APP/PROC/WEB/0] OUT 2023-09-12T03:02:13.284Z  INFO 21 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 224 ms. Found 1 JPA repository interfaces.

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
          "value": "jdbc:mysql://9791a9f3-4b85-4159-957a-857ec777a7cd.mysql.service.internal:3306/service_instance_db?user=9e4c277aa6f948ba90afa059fe5ad29b&password=k9028saggqglq2t4&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "9e4c277aa6f948ba90afa059fe5ad29b"
        },
        "spring.datasource.password": {
          "value": "k9028saggqglq2t4"
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