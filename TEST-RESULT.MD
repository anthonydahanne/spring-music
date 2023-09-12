# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb2-no-libs -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv
    cf bind-service ad-sb2-no-libs mysql-service


Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-12T14:11:25.36-0400 [STG/0] OUT -----> Java Buildpack bcede67 | https://github.com/cloudfoundry/java-buildpack.git#bcede67
   2023-09-12T14:11:25.66-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.1s)
   2023-09-12T14:11:27.19-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 1.8.0_382 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u382%2B6-linux-amd64.tar.gz (1.5s)
   2023-09-12T14:11:28.53-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.3s)
   2023-09-12T14:11:28.53-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-12T14:11:28.75-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.2s)
   2023-09-12T14:11:30.70-0400 [STG/0] OUT Loaded Classes: 21179, Threads: 250
   2023-09-12T14:11:30.82-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-09-12T14:11:30.99-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-09-12T14:11:31.16-0400 [STG/0] OUT -----> Downloading Spring Auto Reconfiguration 2.12.0_RELEASE from https://java-buildpack.cloudfoundry.org/auto-reconfiguration/auto-reconfiguration-2.12.0-RELEASE.jar (0.1s)
   2023-09-12T14:11:31.16-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  ATTENTION: The Spring Auto Reconfiguration and shaded Spring Cloud Connectors libraries are being installed. These projects have been deprecated, are no longer receiving updates and should not be used going forward.
   2023-09-12T14:11:31.16-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  If you are not using these libraries, set `JBP_CONFIG_SPRING_AUTO_RECONFIGURATION='{enabled: false}'` to disable their installation and clear this warning message. The buildpack will switch its default to disable by default after March 2023. Spring Auto Reconfiguration and its shaded Spring Cloud Connectors will be removed from the buildpack after March 2024.
   2023-09-12T14:11:31.16-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  If you are using these libraries, please migrate to java-cfenv immediately. See https://via.vmw.com/EiBW for migration instructions. Once you upgrade this message will go away.
   2023-09-12T14:11:46.63-0400 [STG/0] OUT Exit status 0

```

# app logs

```
   2023-09-12T14:12:06.98-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::               (v2.7.14)
   2023-09-12T14:12:07.39-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:07.380  INFO 7 --- [           main] pertySourceApplicationContextInitializer : 'cloud' property source added
   2023-09-12T14:12:07.40-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:07.402  INFO 7 --- [           main] nfigurationApplicationContextInitializer : Reconfiguration enabled
   2023-09-12T14:12:07.42-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:07.427  INFO 7 --- [           main] o.c.samples.music.Application            : Starting Application using Java 1.8.0_382 on 12c26465-3ecb-49bf-58e1-949c with PID 7 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-12T14:12:07.42-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:07.429  INFO 7 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "cloud", "http2"
   2023-09-12T14:12:09.78-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:09.788  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-12T14:12:09.79-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:09.792  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-12T14:12:10.13-0400 [APP/PROC/WEB/0] OUT 2023-09-12 18:12:10.135  INFO 7 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 321 ms. Found 1 JPA repository interfaces.

```

# actuator

```
{
  "activeProfiles": [
    "cloud",
    "http2"
  ],
{
      "name": "cloud",
      "properties": {
        "cloud.services.mysql.connection.schemespecificpart": {
          "value": "//9a3b2f06ef49489ea330feb80e8a3b4e:pzoxqiq0739q0bx2@a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?reconnect=true"
        },
        "cloud.application.space_id": {
          "value": "251dbb17-06f5-43fa-b37b-d22b8039995e"
        },
        "cloud.services.mysql-service.connection.scheme": {
          "value": "mysql"
        },
        "cloud.application.cf_api": {
          "value": "https://api.sys.maximumbluegreen.cf-app.com"
        },
        "cloud.services.mysql.connection.uri": {
          "value": "mysql://9a3b2f06ef49489ea330feb80e8a3b4e:pzoxqiq0739q0bx2@a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?reconnect=true"
        },
        "cloud.services.mysql.connection.scheme": {
          "value": "mysql"
        },
        "cloud.application.host": {
          "value": "0.0.0.0"
        },
        "cloud.services.mysql-service.connection.password": {
          "value": "pzoxqiq0739q0bx2"
        },
        "cloud.application.application_name": {
          "value": "ad-sb2-no-libs"
        },
        "cloud.application.organization_id": {
          "value": "660ab9fb-0dcf-43cc-955f-003bfb6c333c"
        },
        "cloud.services.mysql-service.connection.username": {
          "value": "9a3b2f06ef49489ea330feb80e8a3b4e"
        },
        "cloud.application.limits": {
          "value": "Complex property type java.util.LinkedHashMap"
        },
        "cloud.services.mysql-service.connection.schemespecificpart": {
          "value": "//9a3b2f06ef49489ea330feb80e8a3b4e:pzoxqiq0739q0bx2@a790f7f6-cc72-4b7a-8fd8-7d96787aa92e.mysql.service.internal:3306/service_instance_db?reconnect=true"
        },
        "cloud.application.process_id": {
          "value": "1dcf6882-0906-4c6b-b428-af0355c4e478"
        },
        "cloud.services.mysql.connection.port": {
          "value": 3306
        },

```

# Results interpretation:

SAR WAS injected; as expected. No traces of Java-cfenv
I've checked the jar did not contain java-cfenv nor SAR

If `JBP_CONFIG_SPRING_AUTO_RECONFIGURATION: '{enabled: false}'` , nothing gets injected
