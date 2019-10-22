# ShedLock Provider for CosmosDB

[ShedLock](https://github.com/lukas-krecan/ShedLock) is a Java library that can be used to be sure that a scheduled task runs only once at the same time in a distributed environment.

The lock may be implemented using various providers like Redis, MongoDB, Hazelcast and so on.

Using the provider in this project, you can rely on Cosmos DB.

To use it, you just need to:

- import the dependency in your pom.xml:
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

- instantiate the right provider, e.g. using Spring with annotation-based configuration:

```java
import com.github.jesty.shedlock.provider.cosmosdb.CosmosDBLockProvider;

...

@Bean
public LockProvider lockProvider(CosmosContainer container) {
    return new CosmosDBLockProvider(container, "lockGroup"); //lockGroup is the partition key
}
```

For more information about ShedLock refer to the [original documentation](https://github.com/lukas-krecan/ShedLock).

The lock relies on a Cosmos DB's collection. In order to perform the lock you have to create a stored procedure ([checkLockAndAcquire.js](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/storedprocedures/checkLockAndAcquire.js)) in your Cosmos DB instance.

## Build from source

Before building this project you need to checkout ShedLock and install _shedlock-test-support_ in your local Maven repository:

    > git clone https://github.com/lukas-krecan/ShedLock.git
    > cd shedlock-test-support
    > mvn clean install 

Finally, you can build this project by running:

    > mvn package

There is an [integration test](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/src/test/java/net/javacrumbs/shedlock/provider/cosmosdb/CosmosDbProviderIntegrationTest.java) that can be run using:

    > mvn verify

This test need a Cosmos DB instance on Azure, or a [Cosmos DB local emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator) instance running.
The instance parameters must be set in [config.properties](https://github.com/jesty/ShedLock/tree/master/providers/cosmosdb/shedlock-provider-cosmosdb/src/test/resources/config.properties).

If you would like to build or install the project in your local repository without running integration tests, you can run:
    
    > mvn install -DskipITs

