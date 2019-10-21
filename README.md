# ShedLock Provider for CosmosDB

[ShedLock](https://github.com/lukas-krecan/ShedLock) is a Java library helpful to be sure that a scheduled task runs only once at the same time in a distributed environment.

The lock may be implemented using various providers like Redis, MongoDB, Hazelcast and so on.

Using the provider in this project, you can use CosmosDB.

To use it, you need only to import the dependency in your pom.xml:

```xml
<dependency>
	<groupId>net.javacrumbs.shedlock</groupId>
	<artifactId>shedlock-core</artifactId>
    <version>3.0.0</version>
</dependency>

<dependency>
    <groupId>com.github.jesty</groupId>
    <artifactId>shedlock-provider-cosmosdb</artifactId>
    <version>1.0.0</version>
</dependency>
```

and then instantiate the right provider, for example using Spring::

```java
import com.github.jesty.shedlock.provider.cosmosdb.CosmosDBLockProvider;

...

@Bean
public LockProvider lockProvider(CosmosContainer container) {
    return new MongoLockProvider(container, "lockGroup"); //lockGroup is the partition key
}
```

For more information about ShedLock refer to [original documentation](https://github.com/lukas-krecan/ShedLock).


The lock rely on a collection on your CosmosDB instance. In order to perform the lock you have to create a stored procedure ([checkLockAndAcquire.js](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/storedprocedures/checkLockAndAcquire.js)) in your CosmosDB instance.


## Build from source

Before build this project you need to checkout ShedLock and install _shedlock-test-support_ in your local Maven repository:

    > git clone https://github.com/lukas-krecan/ShedLock.git
    > cd shedlock-test-support
    > mvn clean install 

After you can build this project using:

    > mvn package

There is an [integration test](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/src/test/java/net/javacrumbs/shedlock/provider/cosmosdb/CosmosDbProviderIntegrationTest.java) that can be run using:

    > mvn verify

This test need a CosmosDB instance on Azure, or the [CosmosDB local emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator).
The instance parameters must be set in [config.properties](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/src/test/resources/config.properties).

If you would like to build or install in your local repository the project without running integration test, you can run:
    
    > mvn install -DskipITs

