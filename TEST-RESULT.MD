# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push -b https://github.com/anthonydahanne/java-buildpack.git\#java-cfenv-anthony

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from http://34.95.29.143:8000/ (built from https://github.com/pivotal-cf/java-cfenv/pull/222)

# Results

Log:
```
   2023-08-29T23:18:27.37-0400 [STG/0] OUT -----> Java Buildpack fc87fc3 | https://github.com/anthonydahanne/java-buildpack.git#fc87fc3
   2023-08-29T23:18:27.78-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/jammy/x86_64/jvmkill-1.17.0-RELEASE.so (0.2s)
   2023-08-29T23:18:28.85-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/jammy/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (1.0s)
   2023-08-29T23:18:30.85-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (2.0s)
   2023-08-29T23:18:30.85-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-08-29T23:18:31.18-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/jammy/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.3s)
   2023-08-29T23:18:32.34-0400 [STG/0] OUT Loaded Classes: 28514, Threads: 250
   2023-08-29T23:18:32.47-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-08-29T23:18:32.65-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-08-29T23:18:32.82-0400 [STG/0] OUT -----> Downloading Java Cf Env 3.1.0 from http://34.95.29.143:8000/java-cfenv-all-3.1.0.jar (0.1s)
   2023-08-29T23:18:56.31-0400 [STG/0] OUT Exit status 0

```

App log:
```
  2023-08-29T23:19:19.64-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.2)
   2023-08-29T23:19:19.76-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:19.762Z  INFO 23 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 23 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-08-29T23:19:19.76-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:19.768Z  INFO 23 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "cloud", "http2"
   2023-08-29T23:19:19.89-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:19.889Z  INFO 23 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-08-29T23:19:21.92-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:21.923Z  INFO 23 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-08-29T23:19:21.93-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:21.929Z  INFO 23 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-08-29T23:19:22.19-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:19:22.192Z  INFO 23 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 246 ms. Found 1 JPA repository interfaces.

```

Actuator (/actuator/env):

```json
{
  "activeProfiles": [
    "cloud",
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
          "value": "jdbc:mysql://9791a9f3-4b85-4159-957a-857ec777a7cd.mysql.service.internal:3306/service_instance_db?user=9f545ab7432148589329381c2aca7a03&password=vxakuwgo6tzckhf4&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "9f545ab7432148589329381c2aca7a03"
        },
        "spring.datasource.password": {
          "value": "vxakuwgo6tzckhf4"
        },
        "spring.datasource.driver-class-name": {
          "value": "com.mysql.cj.jdbc.Driver"
        }
      }
    },
```

# Results interpretation:

Java-cfenv added the `cloud` profile to the `http2` profile set in the manifest. The `mysql` profile was not activated (because the app code that set it was commented out), but since `java-cfenv` detected the Mysql-service, it autoconfigured the mysql datasource by itself.

# SAR disabled, JBP_CONFIG_SPRING_AUTO_RECONFIGURATION: '{enabled: false}'

SAR was not downloaded even though we did not disable it, meaning Java buildpacks did not inject it (as seen in the logs)

# SAR not explicitly disabled

same