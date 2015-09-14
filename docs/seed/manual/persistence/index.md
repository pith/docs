---
title: "Overview"
type: "manual"
zones:
    - "Seed"
sections:
    - "SeedPersistence"
tags:
    - "persistence"
menu:
    SeedPersistence:
        weight: 10
---

Seed provides various persistence supports. Multiple supports can be used simultaneously by adding all the corresponding 
Maven dependencies.

# JDBC

Allows you to configure, inject and use [JDBC](https://jcp.org/en/jsr/detail?id=221) connections. Add it to your application 
with the following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-jdbc</artifactId>
    </dependency>
    
Main features:
  
* Transactions
* Connection pooling with either [HikariCP](http://brettwooldridge.github.io/HikariCP/), [Commons DBCP](http://commons.apache.org/proper/commons-dbcp/),
[C3P0](http://www.mchange.com/projects/c3p0/) or a custom provider.
* JNDI

Learn about JDBC support [here](jdbc).

# JPA

Allows you to configure, inject and use [JPA](https://jcp.org/en/jsr/detail?id=338) units. Add it to your application 
with the following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-jpa</artifactId>
    </dependency>
    
Main features:
    
* Supports JPA 1.0, 2.0 and 2.1
* Transactions
* Automatic or `persistence.xml`-based configuration
* Ability to use a JDBC support datasource
* JNDI
           
Learn about JPA support [here](jpa).

# MongoDB

Allows you to configure, inject and use [MongoDB](https://www.mongodb.org/) clients. Add it to your application with the 
following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-mongodb</artifactId>
    </dependency>
    
Main features:

* Synchronous clients
* Asynchronous clients     

Learn about MongoDB support [here](mongodb).

# Redis

Allows you to configure, inject and use [Jedis](https://github.com/xetorthio/jedis) clients. Add it to your application 
with the following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-redis</artifactId>
    </dependency>

Main features:

* Synchronous clients
* Asynchronous clients
* Transactions
* Pipelining

Learn about Redis support [here](redis).

# Neo4J

Allows you to configure, inject and use [Neo4J](http://neo4j.com/) graph databases. Add it to your application with the 
following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-neo4j</artifactId>
    </dependency>

Main features:

* Transactions
* Embedded databases

Learn about Neo4J support [here](neo4j).

# ElasticSearch

Allows you to configure, inject and use [ElasticSearch](https://www.elastic.co) clients. Add it to your application with 
the following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-elasticsearch</artifactId>
    </dependency>

Main features:

* Embedded indexes
* Remote indexes

Learn about ElasticSearch support [here](elasticsearch).

# Solr

Allows you to configure, inject and use [Solr](http://lucene.apache.org/solr/) clients. Add it to your application with 
the following Maven dependency:

    <dependency>
        <groupId>org.seedstack.seed</groupId>
        <artifactId>seed-persistence-support-solr</artifactId>
    </dependency>

Main features:

* Transactions
* Single instance, load balancing or Solr Cloud

Learn about Solr support [here](solr).
