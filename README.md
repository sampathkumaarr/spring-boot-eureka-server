# Spring Boot Eureka Server
    This is a sample Spring Boot Eureka Server project.
    In this project I have integrated with "Log4j2" and "Sleuth" frameworks for better logging.<br />

# Self-preservation mode:<br />
#Renews: Total number of heartbeats the server received from clients<br />
#Renews threshold: A switch which controls the "self-preservation mode" of Eureka. If "Renews" is below "Renews threshold", the "self-preservation mode" is on.<br />
#self-preservation mode:<br />
    When the Eureka server comes up, it tries to get all of the instance registry information from a neighboring node. If there is a problem getting the information from a 
    node,   the server tries all of the peers before it gives up. If the server is able to successfully get all of the instances, it sets the renewal threshold that it should
    be receiving based on that information. If any time, the renewals falls below the percent configured for that value (below 85% within 15 mins), the server stops expiring
    instances to protect the current instance registry information.<br />

In Netflix, the above safeguard is called as self-preservation mode and is primarily used as a protection in scenarios where there is a network partition between a group of clients and the Eureka Server. In these scenarios, the server tries to protect the information it already has. There may be scenarios in case of a mass outage that this may cause the clients to get the instances that do not exist anymore. The clients must make sure they are resilient to eureka server returning an instance that is non-existent or un-responsive. The best protection in these scenarios is to timeout quickly and try other servers.<br />

#Below are the detailed properties used in the "application.properties"<br />
########################################################################<br />
org.springframework.boot.logging.LogLevel=INFO<br />
########################################################################<br />
server.port=61002<br />
spring.application.name=spring-boot-eureka-server<br />
server.servlet.context-path=/spring-boot-eureka-server<br />
#eureka.instance.instanceId=<br />
#eureka.instance.preferIpAddress=<br />
eureka.instance.appname=${spring.application.name}
eureka.instance.hostname=localhost<br />
eureka.datacenter=site1<br />
eureka.environment=test<br />
#Eureka Self preservation helps to survive the application in terms of Network blips. But we<br />
#are setting it to "false" assuming we won't get into Network blips.<br />
eureka.server.enableSelfPreservation=false<br />
#If we enable self preservation mode, then we need to set the renewalPercentThreshold properties.<br />
#eureka.server.renewalPercentThreshold=3<br />
#eureka.server.renewalThresholdUpdateIntervalMs=120000<br />
########################################################################<br />
eureka.server.evictionInterwalTimerInMs=60000<br />
eureka.server.waitTimeInMsWhenSyncEmpty=120000<br />
# How to build Eureka Clusters?<br />
#Use the below property while deploying Eureka Server clusters with more than 1 Eureka Server.<br />
#eureka.client.serviceUrl.defaultZone=http://10.11.12.12:61002/spring-boot-eureka-server/eureka,http://10.11.12.12:61022/spring-boot-eureka-server/eureka<br />
eureka.client.registerWithEureka=false<br />
eureka.client.fetchRegistry=false<br />
#set above properties to "true" when using Eureka Server clusters with more than 1 Eureka Server.<br />
########################################################################<br />
logging.config=./src/main/resources/eureka-server-log4j2.xml<br />
########################################################################<br />
#eureka.client.availability-zones.zone1='peer1,peer2'<br />
#eureka.client.availability-zones.zone2='peer1,peer2'<br />
#eureka.client.serviceUrl.peer1=http://peer1:8761/eureka-server/eureka<br />
#eureka.client.serviceUrl.peer2=http://peer1:8761/eureka-server/eureka<br />
########################################################################<br />
