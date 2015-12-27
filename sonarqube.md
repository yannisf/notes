# SonarQube

## docker
``docker run sonar``


## Default URL
http://localhost:9000

## Default administrator
admin:admin

## Analyze a maven project with code coverage
``mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true sonar:sonar``

## Links
* [Source code analysis](http://docs.sonarqube.org/display/SONAR/Analyzing+Source+Code)
* [Maven analysis](http://docs.sonarqube.org/display/SONAR/Analyzing+with+SonarQube+Scanner+for+Maven)
* [Maven analysis parameters](http://docs.sonarqube.org/display/SONAR/Analysis+Parameters)
* [Sonar examples (inc. ant)](https://github.com/SonarSource/sonar-examples)


