# Manual Tests conditions

Tests run with:

    ./gradlew clean build
    cf push ad-sb2-no-libs -b https://github.com/anthonydahanne/java-buildpack.git\#java-cfenv-anthony-no-fix

Meaning: with java-cfenv buildpack fetching `java-cfenv-3.1.0.jar` from http://34.95.29.143:8000/ (built from https://github.com/pivotal-cf/java-cfenv/pull/222)

# Results

Log:
```
   -----> Java Buildpack d3eb0ad | https://github.com/anthonydahanne/java-buildpack.git#d3eb0ad
   -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (0.1s)
   -----> Downloading Open Jdk JRE 1.8.0_382 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u382%2B6-linux-amd64.tar.gz (0.6s)
   Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.2s)
   JVM DNS caching disabled in lieu of BOSH DNS caching
   -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (1.3s)
   Loaded Classes: 21179, Threads: 250
   -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (0.0s)
   -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (0.1s)
   Exit status 0


```
# Results interpretation:

SAR was not auto injected; which is not inline with maintaining a SB2 experience with auto SAR injection
I've checked the jar did not contain java-cfenv nor SAR


Then to make sure, I've redeployed with: `cf push ad-sb2-no-libs  -b java_buildpack_offline` and I got the expected:

```
   -----> Java Buildpack v4.61.0 (offline) | https://github.com/cloudfoundry/java-buildpack#a9c22335
   -----> Downloading Jvmkill Agent 1.17.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.17.0-RELEASE.so (found in cache)
   -----> Downloading Open Jdk JRE 1.8.0_382 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/bellsoft-jre8u382%2B6-linux-amd64.tar.gz (found in cache)
   Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (1.2s)
   JVM DNS caching disabled in lieu of BOSH DNS caching
   -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0-RELEASE.tar.gz (found in cache)
   Loaded Classes: 21179, Threads: 250
   -----> Downloading Client Certificate Mapper 1.11.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.11.0-RELEASE.jar (found in cache)
   -----> Downloading Container Security Provider 1.20.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.20.0-RELEASE.jar (found in cache)
   -----> Downloading Maria Db JDBC 2.7.9 from https://java-buildpack.cloudfoundry.org/mariadb-jdbc/mariadb-jdbc-2.7.9.jar (found in cache)
   -----> Downloading Spring Auto Reconfiguration 2.12.0_RELEASE from https://java-buildpack.cloudfoundry.org/auto-reconfiguration/auto-reconfiguration-2.12.0-RELEASE.jar (found in cache)
   [SpringAutoReconfiguration]      WARN  ATTENTION: The Spring Auto Reconfiguration and shaded Spring Cloud Connectors libraries are being installed. These projects have been deprecated, are no longer receiving updates and should not be used going forward.
   [SpringAutoReconfiguration]      WARN  If you are not using these libraries, set `JBP_CONFIG_SPRING_AUTO_RECONFIGURATION='{enabled: false}'` to disable their installation and clear this warning message. The buildpack will switch its default to disable by default after March 2023. Spring Auto Reconfiguration and its shaded Spring Cloud Connectors will be removed from the buildpack after March 2024.
   [SpringAutoReconfiguration]      WARN  If you are using these libraries, please migrate to java-cfenv immediately. See https://via.vmw.com/EiBW for migration instructions. Once you upgrade this message will go away.
   Exit status 0

```

