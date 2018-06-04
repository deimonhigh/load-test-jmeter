FROM java:8

ENV JMETER_PATH=/jmeter
ENV JMETER_HOME=/jmeter/apache-jmeter-3.3
ENV PLUGINS_PATH=$JMETER_HOME/plugins

# a selection of usefull plugins from jmeter-plugins.org
ENV LIST_PLUGINS="jpgc-casutg,jpgc-csl,jpgc-dbmon,jpgc-dummy,jpgc-functions,jpgc-jmxmon,jpgc-perfmon,jpgc-graphs-basic,jpgc-graphs-additional,jpgc-graphs-dist,jpgc-graphs-vs,jpgc-cmd,jpgc-filterresults,jpgc-ggl,jpgc-mergeresults,jpgc-pde,jpgc-synthesis,jpgc-sts,jpgc-standard"

RUN mkdir $JMETER_PATH \
    && cd $JMETER_PATH/ \
    && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-3.3.tgz \
    && tar -xzf apache-jmeter-3.3.tgz \
    && rm apache-jmeter-3.3.tgz

RUN cd $JMETER_HOME/lib/ext \
    && wget -O jmeter-plugins-manager.jar "https://jmeter-plugins.org/get/"

RUN cd $JMETER_HOME/lib \
    && wget -O cmdrunner-2.0.jar "http://search.maven.org/remotecontent?filepath=kg/apc/cmdrunner/2.0/cmdrunner-2.0.jar"

RUN java -cp $JMETER_HOME/lib/ext/jmeter-plugins-manager.jar org.jmeterplugins.repository.PluginManagerCMDInstaller \
    && cd $JMETER_HOME/bin \
    && chmod u+x PluginsManagerCMD.sh

# Run the PluginsManager to download and install files (jars and scripts files)
RUN cd $JMETER_HOME/bin && \
    JVM_ARGS="-Dhttps.proxyHost=$ARG_https_proxyHost -Dhttps.proxyPort=$ARG_https_proxyPort" ./PluginsManagerCMD.sh install $LIST_PLUGINS && \
    JVM_ARGS="-Dhttps.proxyHost=$ARG_https_proxyHost -Dhttps.proxyPort=$ARG_https_proxyPort" ./PluginsManagerCMD.sh status

RUN cd $JMETER_HOME/bin \
    && echo "jmeter.save.saveservice.output_format=csv\njmeter.save.saveservice.data_type=true\njmeter.save.saveservice.label=true\njmeter.save.saveservice.response_code=true\njmeter.save.saveservice.response_data.on_error=true\njmeter.save.saveservice.response_message=true\njmeter.save.saveservice.successful=true\njmeter.save.saveservice.thread_name=true\njmeter.save.saveservice.time=true\njmeter.save.saveservice.subresults=true\njmeter.save.saveservice.assertions=true\njmeter.save.saveservice.bytes=true\njmeter.save.saveservice.latency=true\njmeter.save.saveservice.hostname=true\njmeter.save.saveservice.thread_counts=true\njmeter.save.saveservice.sample_count=true\njmeter.save.saveservice.assertion_results_failure_messages=true\njmeter.save.saveservice.timestamp_format=HH:mm:ss\njmeter.save.saveservice.default_delimiter=;\njmeter.save.saveservice.print_field_names=true\n" > user.properties

# Add Jmeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH
