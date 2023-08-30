# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push -b https://github.com/anthonydahanne/java-buildpack.git\#java-cfenv-anthony

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from http://34.95.29.143:8000/ (built from https://github.com/pivotal-cf/java-cfenv/pull/222)

# Results

Log:
```
   2023-08-29T22:51:37.15-0400 [STG/0] OUT -----> Java Buildpack fc87fc3 | https://github.com/anthonydahanne/java-buildpack.git#fc87fc3
   2023-08-29T22:51:37.25-0400 [APP/PROC/WEB/0] OUT 2023-08-30T02:51:37.249Z  INFO 31 --- [onnection adder] ContainerTrustManagerFactory$PKIXFactory : Adding System Trust Manager
   2023-08-29T22:51:37.56-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/jammy/x86_64/jvmkill-1.17.0-RELEASE.so (0.2s)
   2023-08-29T22:51:38.41-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/jammy/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (0.8s)
   2023-08-29T22:51:40.53-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (2.1s)
   2023-08-29T22:51:40.53-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-08-29T22:51:40.75-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/jammy/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.2s)
   2023-08-29T22:51:42.17-0400 [STG/0] OUT Loaded Classes: 28526, Threads: 250
   2023-08-29T22:51:42.29-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-08-29T22:51:42.48-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-08-29T22:52:08.69-0400 [STG/0] OUT Exit status 0

```

App log:
```
   2023-08-29T23:01:59.45-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.2)
   2023-08-29T23:01:59.70-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:01:59.694Z  INFO 15 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 15 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-08-29T23:01:59.71-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:01:59.713Z  INFO 15 --- [           main] o.c.samples.music.Application            : The following 1 profile is active: "http2"
   2023-08-29T23:01:59.87-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:01:59.876Z  INFO 15 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-08-29T23:02:03.24-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:02:03.248Z  INFO 15 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-08-29T23:02:03.25-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:02:03.254Z  INFO 15 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-08-29T23:02:03.62-0400 [APP/PROC/WEB/0] OUT 2023-08-30T03:02:03.625Z  INFO 15 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 352 ms. Found 1 JPA repository interfaces.

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

Java-cfenv DID NOT add the `cloud` profile to the `http2` profile set in the manifest. The `mysql` profile was not activated (I disabled that in the app), but since `java-cfenv` detected the Mysql-service, it autoconfigured the mysql datasource by itself.

# SAR disabled impact

## SAR disabled  JBP_CONFIG_SPRING_AUTO_RECONFIGURATION: '{enabled: false}'
SAR was not downloaded, meaning Java buildpacks did not inject it (as seen in the logs)

## SAR not explicitly disabled
same