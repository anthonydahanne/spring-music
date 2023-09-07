# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb2-no-libs -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv


Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-07T15:08:35.39-0400 [STG/0] OUT -----> Java Buildpack 0023891 | https://github.com/cloudfoundry/java-buildpack.git#0023891
   2023-09-07T15:08:35.69-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.1s)
   2023-09-07T15:08:36.92-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 17.0.8_7 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre17.0.8%2B7-linux-amd64.tar.gz (1.2s)
   2023-09-07T15:08:38.64-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.7s)
   2023-09-07T15:08:38.64-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-07T15:08:38.80-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.1s)
   2023-09-07T15:08:39.93-0400 [STG/0] OUT Loaded Classes: 27545, Threads: 250
   2023-09-07T15:08:40.04-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.0s)
   2023-09-07T15:08:40.19-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   2023-09-07T15:08:40.34-0400 [STG/0] OUT -----> Downloading Spring Auto Reconfiguration 2.12.0_RELEASE from https://java-buildpack.cloudfoundry.org/auto-reconfiguration/auto-reconfiguration-2.12.0-RELEASE.jar (0.1s)
   2023-09-07T15:08:40.34-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  ATTENTION: The Spring Auto Reconfiguration and shaded Spring Cloud Connectors libraries are being installed. These projects have been deprecated, are no longer receiving updates and should not be used going forward.
   2023-09-07T15:08:40.34-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  If you are not using these libraries, set `JBP_CONFIG_SPRING_AUTO_RECONFIGURATION='{enabled: false}'` to disable their installation and clear this warning message. The buildpack will switch its default to disable by default after March 2023. Spring Auto Reconfiguration and its shaded Spring Cloud Connectors will be removed from the buildpack after March 2024.
   2023-09-07T15:08:40.34-0400 [STG/0] ERR [SpringAutoReconfiguration]      WARN  If you are using these libraries, please migrate to java-cfenv immediately. See https://via.vmw.com/EiBW for migration instructions. Once you upgrade this message will go away.
   2023-09-07T15:08:59.04-0400 [STG/0] OUT Exit status 0

```

# app logs

```
   2023-09-07T15:09:22.14-0400 [APP/PROC/WEB/0] OUT :: Spring Boot ::               (v2.7.14)
   2023-09-07T15:09:22.39-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:22.393  INFO 12 --- [           main] pertySourceApplicationContextInitializer : 'cloud' property source added
   2023-09-07T15:09:22.39-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:22.398  INFO 12 --- [           main] nfigurationApplicationContextInitializer : Reconfiguration enabled
   2023-09-07T15:09:22.42-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:22.421  INFO 12 --- [           main] o.c.samples.music.Application            : Starting Application using Java 17.0.8 on 4a333304-4ad6-4054-533f-f494 with PID 12 (/home/vcap/app/BOOT-INF/classes started by vcap in /home/vcap/app)
   2023-09-07T15:09:22.42-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:22.423  INFO 12 --- [           main] o.c.samples.music.Application            : The following 2 profiles are active: "cloud", "http2"
   2023-09-07T15:09:24.72-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:24.726  INFO 12 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode
   2023-09-07T15:09:24.72-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:24.729  INFO 12 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
   2023-09-07T15:09:24.99-0400 [APP/PROC/WEB/0] OUT 2023-09-07 19:09:24.996  INFO 12 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 247 ms. Found 1 JPA repository interfaces.
```

# actuator

```
 "activeProfiles": [
    "cloud",
    "http2"
  ],
  {
      "name": "cloud",
      "properties": {
        "cloud.services.mysql.connection.port": {
          "value": 3306
        },
        "cloud.application.host": {
          "value": "0.0.0.0"
        },
        "cloud.application.application_name": {
          "value": "ad-sb2-no-libs"
        },
        "cloud.application.organization_name": {
          "value": "system"
        },
        "cloud.application.instance_index": {
          "value": 0
        },
        "cloud.application.cf_api": {
          "value": "https://api.sys.pastelviolet.cf-app.com"
        },
        "cloud.services.mysql.connection.password": {
          "value": "zd8xizyoiu0dz5q8"
        },
        "cloud.services.mysql.connection.query": {
          "value": "reconnect=true"
        },
        "cloud.application.application_id": {
          "value": "6f57f22c-c54a-4d82-809c-9c5968a48325"
        },
        "cloud.application.application_uris": {
          "value": "Complex property type java.util.ArrayList"
        },
        "cloud.services.mysql.connection.jdbcurl": {
          "value": "jdbc:mysql://03734107-3b37-45d1-a830-8bf6c1737f6a.mysql.service.internal:3306/service_instance_db?user=043f8ee0bc144c88ad9d9dd5ab7d82b0&password=zd8xizyoiu0dz5q8&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "cloud.application.application_version": {
          "value": "080bc332-5514-437e-b59e-7c96ace59ecd"
        },
        "cloud.application.port": {
          "value": 8080
        },
        "cloud.application.process_id": {
          "value": "6f57f22c-c54a-4d82-809c-9c5968a48325"
        },
        "cloud.application.uris": {
          "value": "Complex property type java.util.ArrayList"
        },
        "cloud.application.name": {
          "value": "ad-sb2-no-libs"
        },
        "cloud.services.mysql.connection.username": {
          "value": "043f8ee0bc144c88ad9d9dd5ab7d82b0"
        },
        "cloud.application.app-id": {
          "value": "ad-sb2-no-libs"
        },
        "cloud.services.mysql.connection.scheme": {
          "value": "mysql"
        },
        "cloud.application.instance_id": {
          "value": "4a333304-4ad6-4054-533f-f494"
        },
        "cloud.application.version": {
          "value": "080bc332-5514-437e-b59e-7c96ace59ecd"
        },
        "cloud.application.organization_id": {
          "value": "1d67710b-2db7-4830-8457-9cca6c4e5fc9"
        },
        "cloud.application.limits": {
          "value": "Complex property type java.util.LinkedHashMap"
        },
        "cloud.application.process_type": {
          "value": "web"
        },
        "cloud.application.instance-id": {
          "value": "4a333304-4ad6-4054-533f-f494"
        },
        "cloud.services.mysql.connection.schemespecificpart": {
          "value": "//043f8ee0bc144c88ad9d9dd5ab7d82b0:zd8xizyoiu0dz5q8@03734107-3b37-45d1-a830-8bf6c1737f6a.mysql.service.internal:3306/service_instance_db?reconnect=true"
        },
        "cloud.application.space_name": {
          "value": "pivotal-services"
        },
        "cloud.services.mysql.connection.path": {
          "value": "service_instance_db"
        },
        "cloud.application.space_id": {
          "value": "9b74600d-5545-4d13-bd9e-eacea96cf09f"
        },
        "cloud.services.mysql.connection.host": {
          "value": "03734107-3b37-45d1-a830-8bf6c1737f6a.mysql.service.internal"
        },
        "cloud.services.mysql.id": {
          "value": "mysql"
        },
        "cloud.services.mysql.connection.uri": {
          "value": "mysql://043f8ee0bc144c88ad9d9dd5ab7d82b0:zd8xizyoiu0dz5q8@03734107-3b37-45d1-a830-8bf6c1737f6a.mysql.service.internal:3306/service_instance_db?reconnect=true"
        }
      }
    }
```

# Results interpretation:

SAR WAS injected; as expected. No traces of Java-cfenv
I've checked the jar did not contain java-cfenv nor SAR


