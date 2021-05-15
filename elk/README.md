## To start the ELK run below command
```
docker-compose up
```


in your spring application you can configuree below settings for logging

### logback-spring.xml
~~~
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true">
    <include resource="org/springframework/boot/logging/logback/base.xml"/>

    <appender name="logstash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <param name="Encoding" value="UTF-8"/>
        <remoteHost>localhost</remoteHost>
		<port>5000</port>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder"/>
    </appender>

    <root level="INFO">
        <appender-ref ref="logstash"/>
    </root>
</configuration>
~~~





### Dependency required for json format
~~~
<dependency>
			<groupId>net.logstash.logback</groupId>
			<artifactId>logstash-logback-encoder</artifactId>
			<version>6.2</version>
		</dependency>
~~~

### logback.xml
~~~
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
	<appender name="kibanaLogger-console" class="ch.qos.logback.core.ConsoleAppender">
		<!-- <filter class="com.realspeed.common.logger.LogFilter" /> -->
		<encoder
			class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
			<providers>
				<provider
					class="net.logstash.logback.composite.loggingevent.ArgumentsJsonProvider" />
				<timestamp />
				<pattern>
					<pattern>
						{
						"logger":"%logger",
						"thread":"%thread",
						"level": "%level",
						"traceId": "%X{X-B3-TraceId:-}",
						"spanId": "%X{X-B3-SpanId:-}",
						"message":"%replace(%message){'null',''}",
						"logCode":"%X{logCode}",
						"priority":"%X{priority}",
						"appName":"%X{appName}",              	
						"logLevel":"%X{logLevel}",
						"logger":"%X{logger}",
						"httpStatus":"%X{httpStatus}",
						"serviceImpact":"%X{serviceImpact}",
						"exception": "%ex"
						}
					</pattern>
				</pattern>
			</providers>
		</encoder>
	</appender>
	
	<appender name="logstash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
		<filter class="com.realspeed.common.logger.LogFilter" />
		<param name="Encoding" value="UTF-8"/>
		<remoteHost>localhost</remoteHost>
		<port>5000</port>
		<encoder
				class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
			<providers>
				<provider
						class="net.logstash.logback.composite.loggingevent.ArgumentsJsonProvider" />
				<timestamp />
				<pattern>
					<pattern>
						{
						"logger":"%logger",
						"thread":"%thread",
						"level": "%level",
						"traceId": "%X{X-B3-TraceId:-}",
						"spanId": "%X{X-B3-SpanId:-}",
						"message":"%replace(%message){'null',''}",
						"logCode":"%X{logCode}",
						"priority":"%X{priority}",
						"appName":"%X{appName}",
						"logLevel":"%X{logLevel}",
						"logger":"%X{logger}",
						"httpStatus":"%X{httpStatus}",
						"serviceImpact":"%X{serviceImpact}",
						"exception": "%ex"
						}
					</pattern>
				</pattern>
			</providers>
		</encoder>
	</appender>

	<root level="INFO">
		<appender-ref ref="kibanaLogger-console" />
		<appender-ref ref="logstash" />
	</root>
</Configuration>

~~~
