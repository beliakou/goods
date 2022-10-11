# Gradle

## Gradle

#### Overwrite property from `gradle.properties`
`If the environment variable name looks like ORG_GRADLE_PROJECT_prop=somevalue, then Gradle will set a prop property on your project object, with the value of somevalue.`

#### Publish to `.m2`
`./gradlew publishToMavenLocal`

#### PUblish to `.m2` with version:
`./gradlew publishToMavenLocal -Prelease.version=3.18.5-snapshot`

#### Add gradle wrapper
`gradle wrapper`

#### Skip tests
`gradlew build -x test`

#### List dependencies
`./gradlew :subproject:dependencies`

#### Boot Run with debug
`SPRING_PROFILES_ACTIVE=dev ./gradlew bootRun --debug-jvm`

#### Run in debug mode
`--no-daemon -Dorg.gradle.debug=true`

#### Run single test
`gradle test --tests org.gradle.SomeTest.someSpecificFeature`  
`gradle test --tests *SomeTest.someSpecificFeature`

#### Test output
`gradle test -i`


## Maven

#### maven - remove dependency from .m2
`mvn dependency:purge-local-repository -DmanualInclude="com.ihg.enterprise:gdc-cache-client,com.ihg.enterprise:gdc-producer"`

#### maven - describe lifecycle
`mvn help:describe -Dcmd=install -pl grs-hotel-gateway-common`

#### maven - create sources jar
`mvn org.apache.maven.plugins:maven-source-plugin:3.0.1:jar -pl grs-hotel-gateway-common install`  
or
`mvn source:jar install`

#### maven - download dependency to .m2
`mvn -DgroupId=commons-io -DartifactId=commons-io -Dversion=1.4 dependency:get`

## Localstack

#### SNS - create topic
`aws --endpoint-url=http://localhost:4566 sns create-topic --name my_topic`

#### SNS - list topics
`aws --endpoint-url=http://localhost:4566 sns list-topics`

#### SQS - create queue
`aws --endpoint-url=http://localhost:4566 sqs create-queue --queue-name my_queue`

#### SQS - list queues
`aws --endpoint-url=http://localhost:4566 sqs list-queues`

#### SNS - subscribe
`aws --endpoint-url=http://localhost:4566 sns subscribe --topic-arn arn:aws:sns:us-west-2:000000000000:my_topic --protocol sqs --notification-endpoint arn:aws:sqs:us-west-2:000000000000:my_queue`

#### SQS - receive message
`aws --endpoint-url=http://localhost:4566 sqs receive-message --queue-url=http://localhost:4576/queue/celery.fifo`

#### SNS - set `RawMessageDelivery`
`aws --endpoint-url=http://localhost:4566 sns set-subscription-attributes --subscription-arn arn:aws:sns:us-west-2:000000000000:my_topic:abb7b9f0-461e-4fed-acf6-a5b79b9077a2 --attribute-name RawMessageDelivery --attribute-value true`

#### SNS - list subscribtions
`aws --endpoint-url=http://localhost:4566 sns list-subscriptions`

#### Send message to queue
`aws --endpoint-url=http://localhost:4566 sns publish --topic-arn arn:aws:sns:us-west-2:000000000000:my_topic --message "{\"uuid\":\"99d8799d-3f1a-4886-8239-3aaca9a7105b\", \"urn\":\"urn:MediaServerGracefulTerminationEvent:json\", \"payload\":\"{\\\"uuid\\\":\\\"fd15418f-e733-44cd-99cb-2c65c07fbd5f\\\"}\"}"`

#### Purge queue
`aws sqs purge-queue --endpoint-url http://localhost:4566 --queue-url "http://localhost:4576/000000000000/celery"`


## Docker

#### check Docker memory status
`docker ps -q | xargs docker stats --no-stream`  
`watch bash -c "docker ps -q | xargs docker stats --no-stream"`

#### check Docker disk usage
`sudo docker system df --verbose`

## Git

#### Set origin
`git branch -u origin/US93056`

#### Rebase few commits
`git rebase --onto master server client`  
This basically says, “Take the client branch, figure out the patches since it diverged from the server branch, and replay these patches in the client branch as if it was based directly off the master branch instead.” It’s a bit complex, but the result is pretty cool.  
Or  
`git rebase --onto origin/master client~1 client`  
take last commit from client and rebase it on master, skip everything else in client branch

#### list root of repository
`git ls-tree @ --name-only`

#### remove file from stage
` git reset @ -- readme.txt`

#### global gitignore file
check if it exists: 
`git config --get core.excludesfile`  
create one: 
`git config --global core.excludesfile '~/.git/gitignore'`

#### pretty history with commiter name
`git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit`

#### remove file from staging
`git reset HEAD -- <file>`

#### find all my TODOs
`git grep -l TODO | xargs -n1 git blame -f | grep 'My Name' | grep TODO`

#### List tags
`git show-ref --tags`

#### Show history for file
`git log --full-history -- src/main/java/com/test/Class.java`


## Shell/misc

#### nc - listen on port
`nc -l -p 9091`
  
#### expose port to internet
`ssh -R 80:127.0.0.1:3000 ssh.localhost.run`

#### curl - request from file
`curl -X POST -H 'Content-type:application/xml' -d @request.txt http://google.com > response.txt`

#### static files server
`python3 -mhttp.server 8080`

#### JQ
`curl 192.168.8.108:9001/processing-service/actuator/env | jq -C '.propertySources[] | select(.name=="systemProperties") | .properties."emailing.hostnames.USER"'`
