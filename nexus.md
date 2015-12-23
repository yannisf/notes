# Sonatype Nexus
Maven artifact repository manager
http://www.sonatype.com/nexus/solution-overview/nexus-repository

## docker
* https://hub.docker.com/r/sonatype/nexus/
```
sudo mount -t cifs -o username=admin,uid=200 //smbserver/nexus /var/nexus
docker run -d -p 8081:8081 --name nexus -v  /var/nexus:/sonatype-work sonatype/nexus
```
Nexus should be available at `http://localhost:8081`

