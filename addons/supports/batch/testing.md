---
title: "Testing"
type: "addon"
zones:
    - "Addons"
tags:
    - "batch"
    - "test"
    - "cli"
sections:
    - "AddonsSupports"
subsections:
    - "Spring batch"
menu:
    AddonsSupportsSpringBatch:
        weight: 40
---

To be able to do SeedStack integration testing, add the Seed integration test support to your project. Check the 
documentation [here](/docs/seed/manual/testing/integration).

# Basic testing

The following example checks that the batch returns with the exit code 0 and subsequently that injection works.

    import org.seedstack.seed.it.AbstractSeedIT;

    public class RunnerBatchIT extends AbstractSeedIT {
        @Inject
        MessageService messageService;
     
        @Test
        @WithCommandLine(
            command = "run-job", 
            args = {"--job", "mySimpleJob"}, 
            expectedExitCode = 0
        )
        public void testBatch() {
            assertThat(messageService).isNotNull();
        }
    }
    
We could easily use the service (or any injectable class) to check for the batch results. The `@WithCommandLine` annotation
simulates the running of a command from the operating system command line. All the arguments of the `run-job` command
can be used in the `args` attribute. Look [here](../running-jobs) for information about these arguments. 

{{% callout info %}}
Note that the test method is called **after** the job is completed. `@Before` annotated methods are executed after Kernel
startup (so you can use injection in them) but before job execution (so you can prepare a dataset if needed).
{{% /callout %}}