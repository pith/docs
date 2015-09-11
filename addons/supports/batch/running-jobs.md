---
title: "Running jobs"
type: "addon"
zones:
    - "Addons"
tags:
    - "batch"
    - "cli"
    - "shade"
    - "maven"
sections:
    - "AddonsSupports"
subsections:
    - "Spring batch"
menu:
    AddonsSupportsSpringBatch:
        weight: 30
---

The `org.seedstack.seed.cli.SeedRunner` class contains a `main()` method. Its role is to launch a named command directly
from the operating system command line. SeedStack Spring Batch support provides the `run-job` command to start a Spring
Batch job: 
 
    java [java-options] org.seedstack.seed.cli.SeedRunner run-job [run-job-options]
    
This will start the Spring Batch job named `job`.    
 
# Options
        
A number of options are available for customizing the `run-job` command behavior:

<table class="table table-striped">
<tbody>
<tr>
<th>Short option</th>
<th>Long option</th>
<th>Description</th>
</tr>
<tr>
<td>-j</td>
<td>--job jobName</td>
<td>Specify the job to launch by name (<code>job</code> by default).</td>
</tr>
<tr>
<td>-l</td>
<td>--jobLauncher jobLauncherName</td>
<td>Specify the job launcher by name (<code>jobLauncher</code> by default).</td>
</tr>
<tr>
<td>-P</td>
<td>--jobParameters paramName=paramValue</td>
<td>Specify a job parameter. Can be used multiple times.</td>
</tr>
</tbody>
</table>

# Example

Consider the following command:

```sh
java -cp "..." -Dsysprop=sysvalue org.seedstack.seed.cli.SeedRunner run-job --job myJob --jobParameter param1=value1 --jobParameter param2=value2
```

This will execute a Spring Batch job with the following characteristics:

* The classpath will be defined by the -cp option of the JVM,
* A system property `sysprop` with the value `sysvalue` will be set,
* The job named `myJob` will be executed,
* Two parameters will be passed to the job: `param1` with value `value1` and `param2` with value `value2`.

# Executable Über-JAR

To run the batch from a unique JAR, you can build the project with the Apache Maven Shade Plugin. This plugin will package 
the artifact as an über JAR with all necessary dependencies. For more information please refer to the plugin 
[documentation](http://maven.apache.org/plugins/maven-shade-plugin/examples/executable-jar.html). You can find a typical
configuration of the plugin below:

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>...</version>
                <configuration>
                    <transformers>
                        <transformer
                            implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                            <resource>META-INF/spring.handlers</resource>
                        </transformer>
    
                        <transformer
                            implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                            <resource>META-INF/spring.schemas</resource>
                        </transformer>
    
                        <transformer
                            implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <manifestEntries>
                                <Main-Class>org.seedstack.seed.cli.SeedRunner</Main-Class>
                            </manifestEntries>
                        </transformer>
    
                        <transformer
                            implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    </transformers>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>


