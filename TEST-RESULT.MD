# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb3-with-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv
    cf bind-service ad-sb3-with-cfenv mysql-service

Meaning: with java-cfenv buildpack using `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-12T15:32:39.07-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-12T15:32:39.64-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.3s)
   2023-09-12T15:32:40.72-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (1.0s)
   2023-09-12T15:32:42.44-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.7s)
   2023-09-12T15:32:42.44-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-12T15:32:42.86-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.4s)
   2023-09-12T15:32:44.44-0400 [STG/0] OUT Loaded Classes: 28563, Threads: 250
   2023-09-12T15:32:44.60-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-09-12T15:32:44.90-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.2s)
   2023-09-12T15:33:04.66-0400 [STG/0] OUT Exit status 0

```

App log:
```
   2023-09-12T15:33:26.15-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::                (v3.1.3)
   2023-09-12T15:33:26.32-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:26.325Z  INFO 7 --- [           main] o.c.samples.music.Application            : Starting Application v1.0 using Java 17.0.8 with PID 7 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-12T15:33:26.33-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:26.331Z  INFO 7 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "http2", "cloud"
   2023-09-12T15:33:26.49-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:26.497Z  INFO 7 --- [           main] s.b.CfDataSourceEnvironmentPostProcessor : Setting spring.datasource properties from bound service [mysql-service]
   2023-09-12T15:33:28.88-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:28.875Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-12T15:33:28.88-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:28.883Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-12T15:33:29.38-0400 [APP/PROC/WEB/0] OUT 2023-09-12T19:33:29.385Z  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 477 ms. Found 1 JPA repository interfaces.

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
          "value": "jdbc:mysql://a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?user=4e14109a36574883ae6c797089daf490&password=hchl6qazx7f4vr1y&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "4e14109a36574883ae6c797089daf490"
        },
        "spring.datasource.password": {
          "value": "hchl6qazx7f4vr1y"
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