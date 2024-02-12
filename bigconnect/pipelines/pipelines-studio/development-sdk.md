# Development SDK

BigConnect Data Pipelines allows developers to create their own Origins, Processors, Destinations and Executors, if the ones provided out-of-the-box do not fit their specific use-case.

In order to generate the basic project structure you will need to run in terminal:

```
mvn archetype:generate -DarchetypeGroupId=io.bigconnect -DarchetypeArtifactId=dp-stage-lib-tutorial -DarchetypeVersion=3.7.2 -DinteractiveMode=true
```

This will generate a Java project template that can be used to build your custom extension. The basic project archetype comes also with example code for an Origin, a Destination and an Executor.

To add additional dependencies to your project, edit the `pom.xml` file and add your dependencies.

**Update processor labels**

The label is the text visible in the Data Pipelines interface. So, we will need a custom label.

```
@StageDef(
    version = 1,
    label = "Content Extract Processor",
    description = "",
    icon = "default.png",
    onlineHelpRefUrl = ""
)
```

**Add processor-specific code**

```java
package ro.example.stage.processor.sample;

import io.bigconnect.pipeline.api.Field;
import io.bigconnect.pipeline.api.FileRef;
import io.bigconnect.pipeline.api.base.OnRecordErrorException;
import org.apache.tika.exception.TikaException;
import org.apache.tika.parser.AutoDetectParser;
import org.apache.tika.parser.ParseContext;
import org.apache.tika.parser.Parser;
import org.apache.tika.sax.BodyContentHandler;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.ContentHandler;
import org.xml.sax.SAXException;
import ro.example.stage.lib.sample.Errors;

import io.bigconnect.pipeline.api.Record;
import io.bigconnect.pipeline.api.StageException;
import io.bigconnect.pipeline.api.base.SingleLaneRecordProcessor;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;

import org.apache.tika.metadata.Metadata;

public abstract class SampleProcessor extends SingleLaneRecordProcessor {
    private static final Logger LOG = LoggerFactory.getLogger(SampleProcessor.class);

    /**
     * Gives access to the UI configuration of the stage provided by the {@link SampleDProcessor} class.
     */
    public abstract String getConfig();

    /**
     * {@inheritDoc}
     */
    @Override
    protected List<ConfigIssue> init() {
        // Validate configuration values and open any required resources.
        List<ConfigIssue> issues = super.init();

        if (getConfig().equals("invalidValue")) {
            issues.add(
                    getContext().createConfigIssue(
                            Groups.SAMPLE.name(), "config", Errors.SAMPLE_00, "Here's what's wrong..."
                    )
            );
        }

        // If issues is not empty, the UI will inform the user of each configuration issue in the list.
        return issues;
    }

    /**
     * {@inheritDoc}
     */
    @Override
    public void destroy() {
        // Clean up any open resources.
        super.destroy();
    }

    /**
     * {@inheritDoc}
     */
    @Override
    protected void process(Record record, SingleLaneBatchMaker batchMaker) throws StageException {

        FileRef fileRef = record.get("/fileRef").getValueAsFileRef();


        try {

            InputStream inputStream = fileRef.createInputStream(getContext(), InputStream.class);
            String content = extractContentUsingParser(inputStream);

            Record newRecord = getContext().createRecord(record);
            HashMap<String, Field> root = new HashMap<>();
            root.put("fileInfo", record.get("/fileInfo"));
            root.put("fileRef", record.get("/fileRef"));

            root.put("content", Field.create(content));

            newRecord.set("/", Field.create(root));
            batchMaker.addRecord(newRecord);

        } catch (IOException e) {
            LOG.error(e.getMessage(), e);
            throw new OnRecordErrorException(record, Errors.SAMPLE_01, e);

        } catch (Exception e) {
            LOG.error(e.getMessage(), e);
            throw new OnRecordErrorException(record, Errors.SAMPLE_00, e);
        }
    }

    public static String extractContentUsingParser(InputStream stream)
            throws IOException, TikaException, SAXException {

        Parser parser = new AutoDetectParser();
        ContentHandler handler = new BodyContentHandler(-1);
        Metadata metadata = new Metadata();
        ParseContext context = new ParseContext();

        parser.parse(stream, handler, metadata, context);
        return handler.toString();
    }
}
```

**Build the project**

```
mvn clean package -DskipTests
```

After the build, in the project target folder you will find two artifacts: a tar.gz archive and a jar.

**Install the processor**

Install the processor inside your Data Pipelines installation folder, under the `user-libs` folder.
