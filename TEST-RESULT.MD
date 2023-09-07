# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb2-with-cfenv -b https://github.com/cloudfoundry/java-buildpack.git\#java-cfenv

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from maven central

# Results

Log:
```
   2023-09-07T16:26:53.44-0400 [STG/0] OUT -----> Java Buildpack 0023891 | https://github.com/cloudfoundry/java-buildpack.git#0023891
   2023-09-07T16:26:53.93-0400 [STG/0] OUT -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.3s)
   2023-09-07T16:26:54.81-0400 [STG/0] OUT -----> Downloading Open Jdk JRE 1.8.0_382 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u382%2B6-linux-amd64.tar.gz (0.8s)
   2023-09-07T16:26:56.04-0400 [STG/0] OUT Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.2s)
   2023-09-07T16:26:56.04-0400 [STG/0] OUT JVM DNS caching disabled in lieu of BOSH DNS caching
   2023-09-07T16:26:56.32-0400 [STG/0] OUT -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (0.2s)
   2023-09-07T16:26:58.01-0400 [STG/0] OUT Loaded Classes: 21192, Threads: 250
   2023-09-07T16:26:58.15-0400 [STG/0] OUT -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.1s)
   2023-09-07T16:26:58.42-0400 [STG/0] OUT -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.2s)
   2023-09-07T16:27:13.60-0400 [STG/0] OUT Exit status 0


```

# app logs

```

```

# actuator

```
 "activeProfiles": [
    "http2"
    "mysql",
  ],
      {
      "name": "cfenvjdbc",
      "properties": {
        "spring.datasource.url": {
          "value": "jdbc:mysql://03734107-3b37-45d1-a830-8bf6c1737f6a.mysql.service.internal:3306/service_instance_db?user=b8ea1f696e16454a84e38c34921aeffe&password=jkbfdyh6gzmtke91&sslMode=VERIFY_IDENTITY&useSSL=true&requireSSL=true&enabledTLSProtocols=TLSv1.2&serverSslCert=/etc/ssl/certs/ca-certificates.crt"
        },
        "spring.datasource.username": {
          "value": "b8ea1f696e16454a84e38c34921aeffe"
        },
        "spring.datasource.password": {
          "value": "jkbfdyh6gzmtke91"
        },
        "spring.datasource.driver-class-name": {
          "value": "com.mysql.cj.jdbc.Driver"
        }
      }
    },
```

# Results interpretation:

SAR WAS NOT, NOR java-cfenv, since it was already in the classpath

This is the expected "old" behaviour


