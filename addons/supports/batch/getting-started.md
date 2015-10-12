---
title: "Getting started"
type: "addon"
zones:
    - "Addons"
tags:
    - "batch"
    - "example"
    - "tasklet"
    - "job"
sections:
    - "AddonsSupports"
subsections:
    - "Spring batch"
menu:
    AddonsSupportsSpringBatch:
        weight: 20
---

The goal of this chapter is to create your first batch in order to make clear Spring Batch classes and
interfaces usage as clear as possible with a simple job example. This one step job will just print “My Simple Job”.

# Add Maven dependencies

This example requires `business-core`:

 		<dependency>
            <groupId>org.seedstack.seed</groupId>
            <artifactId>seed-springbatch-support</artifactId>
        </dependency>
        <dependency>
           <groupId>org.seedstack.business</groupId>
           <artifactId>business-core</artifactId>
       </dependency>


# Create the application context

We need to set up a Spring Batch environment. Spring files must be in the `META-INF/spring` classpath location and end
with `-context.xml` to be automatically detected. The `application-context.xml` file:

    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:batch="http://www.springframework.org/schema/batch"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:seed="http://www.seedstack.org/xml-schemas/spring-support"
           xsi:schemaLocation="http://www.springframework.org/schema/batch
            http://www.springframework.org/schema/batch/spring-batch-2.2.xsd
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
            http://www.seedstack.org/xml-schemas/spring-support
            http://www.seedstack.org/xml-schemas/spring-support/spring-support-1.1.xsd">
     
        <bean id="jobRepository" class="org.springframework.batch.core.repository.support.MapJobRepositoryFactoryBean">
            <property name="transactionManager" ref="transactionManager" />
        </bean>
    
        <bean id="transactionManager" class="org.springframework.batch.support.transaction.ResourcelessTransactionManager" />
    
        <bean id="jobLauncher" class="org.springframework.batch.core.launch.support.SimpleJobLauncher">
            <property name="jobRepository" ref="jobRepository" />
        </bean>
    </beans>

Description of the beans:

- `JobRepository` : responsible for persistence of batch meta-data information.
- `JobLauncher` : responsible for launching the batch job.
- `TransactionManager` : As this example won’t be dealing with transactional data, we are using `ResourcelessTransactionManager`
 which is mainly used for testing purpose. **Don't use in production**.

# Create the service

We will create a service that will be injected directly in a Spring Batch tasklet. The service interface: 

    package org.myorg.myapp.domain.services;

    import org.seedstack.seed.business.api.Service;

    @Service
    public interface MessageService {
        public String getMessage();
    }

The service implementation:

    package org.myorg.myapp.infrastructure.services;

    import org.myorg.myapp.domain.services.MessageService;

    public class MessageServiceImpl implements MessageService {

        public String getMessage() {
            return "--- My Simple Job ----";
        }

    }

# Create the Tasklet

A tasklet is a Class containing custom logic to be ran as a part of a job. `PrintTasklet` is our custom tasklet which
implements `Tasklet` interface and overrides the `execute()` method which prints the message from `MessageService`.

    package org.seedstack.seed.batch.tasklet;
    
    import org.springframework.batch.core.StepContribution;
    import org.springframework.batch.core.scope.context.ChunkContext;
    import org.springframework.batch.core.step.tasklet.Tasklet;
    import org.springframework.batch.repeat.RepeatStatus;
    
    import org.seedstack.seed.service.MessageService;
    
    public class PrintTasklet implements Tasklet {
    
        private MessageService messageService;
        private Logger logger = LoggerFactory.getLogger(PrintTasklet.class);
    
        public RepeatStatus execute(StepContribution contribution,
                ChunkContext chunkContext) throws Exception {
            logger.info(messageService.getMessage());
            return RepeatStatus.FINISHED;
        }
    
        public MessageService getMessageService() {
            return messageService;
        }
    
        public void setMessageService(MessageService messageService) {
            this.messageService = messageService;
        }
        
    }

# Define the job Configuration

In this section we will configure the Spring Batch job context to use our Tasklet and inject the `MessageService` Service.
The `job-context.xml` file:

    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:batch="http://www.springframework.org/schema/batch"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:seed="http://www.seedstack.org/xml-schemas/spring-support"
           xsi:schemaLocation="http://www.springframework.org/schema/batch
            http://www.springframework.org/schema/batch/spring-batch-2.2.xsd
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
            http://www.seedstack.org/xml-schemas/spring-support
            http://www.seedstack.org/xml-schemas/spring-support/spring-support-1.1.xsd">
     
        <import resource="application-context.xml"/>
        
        <batch:job id="mySimpleJob">        
            <batch:step id="printStep" >
                <batch:tasklet>
                    <bean class="org.seedstack.seed.batch.tasklet.PrintTasklet">
                        <property name="messageService">
                            <seed:instance class="org.seedstack.seed.service.MessageService"/>
                        </property>                        
                    </bean>
                </batch:tasklet>
            </batch:step>        
        </batch:job>
    </beans>

The example above illustrates the basic structure of a job. A job (`<batch:joby>` tag) is made of steps (`<batch:step>` 
tag) with a Tasklet (`<batch:tasklet>` tag) and irelated beans to be injected. Steps are executed one by one following 
their declared order. For more information, please read the [Spring Batch documentation](http://docs.spring.io/spring-batch/reference/html/index.html).
