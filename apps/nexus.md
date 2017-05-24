# Sonatype Nexus
[Maven artifact repository manager](http://www.sonatype.com/nexus/solution-overview/nexus-repository)

## docker
* https://hub.docker.com/r/sonatype/nexus/
```
sudo mount -t cifs -o username=admin,uid=200 //smbserver/nexus /var/nexus
docker run -d -p 8081:8081 --name nexus -v  /var/nexus:/sonatype-work sonatype/nexus
```

Nexus should be available at `http://localhost:8081`

From there on just:
```
docker start nexus
docker stop nexus
```

## settings.xml
```
<settings>
  <mirrors>
    <mirror>
      <!--This sends everything else to /public -->
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/content/groups/public</url>
    </mirror>
  </mirrors>
  <profiles>
    <profile>
      <id>nexus</id>
      <!--Enable snapshots for the built in central repo to direct -->
      <!--all requests to nexus via the mirror -->
      <repositories>
        <repository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
     <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
  <activeProfiles>
    <!--make the profile active all the time -->
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
</settings>
```

## Deploy third party artifacts on nexus
```
mvn deploy:deploy-file \
    -Dfile=artifact.jar \
    -DgroupId=com.google.code \
    -DartifactId=kaptcha \
    -Dversion=2.3 \
    -Dpackaging=jar \
    -Durl=http://localhost:8081/content/repositories/thirdparty \
    -DrepositoryId=thirdparty
```
**Note**: In your ``settings.xml`` it is expected to have configured the repository like so:
 ```
<servers>
    <server>
        <id>thirdparty</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
</servers>
 ...

== Nexus 3

=== Inbound SSL - Configuring to Serve Content via HTTPS
# Create a Java keystore file at $install-dir/etc/ssl/keystore.jks which contains the Jetty SSL certificate to use. Add the password into ${jetty.etc}/jetty-https.xml.
----
$ keytool -keystore keystore.jks -alias jetty -genkey -keyalg RSA
----
# Edit $data-dir/etc/nexus.properties. Add a property on a new line application-port-ssl=8443. Change 8443 to be your preferred port on which to expose the HTTPS connector.
# Edit $data-dir/etc/nexus.properties. Change the nexus-args property comma delimited value to include ${jetty.etc}/jetty-https.xml. Save the file.
# Restart Nexus. Verify HTTPS connections can be established.
# Update the Base URL to use https in your repository manager configuration using the Base URL capability.

=== Proxying docker hub

