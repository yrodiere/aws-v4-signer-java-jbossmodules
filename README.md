AWS v4 Signer for Java JBoss Module
===================================

Packaging of [AWS v4 Signer for Java](https://github.com/lucasweb78/aws-v4-signer-java) as
a [JBoss Module](https://docs.jboss.org/author/display/MODULES/Home).

Historically the Hibernate Search project has been releasing such modules,
for convenience of Hibernate Search users running applications on WildFly or JBoss EAP.

We now moved these artifacts to a dedicated bundle so that other projects using the AWS v4 Signer
don't have to download the Hibernate Search specific modules,
and to attempt to have a single consistent distribution of such modules.

This should also make it easier to release a new bundle of these modules
as soon as a new AWS v4 Signer version is released,
without necessarily waiting for Hibernate Search to have adopted the new version.

## Version conventions

We will use an `X.Y.Z.qualifier` pattern as recommended by
[JBoss Project Versioning](https://developer.jboss.org/wiki/JBossProjectVersioning),
wherein the `X.Y.Z` section will match the version of the AWS v4 Signer version included in the modules,
possibly using a `0` for the last figure when this is missing in the AWS v4 Signer version scheme.

We will add an additional qualifier, lexicographically increasing with further releases, to distinguish
different releases of this package when still containing the same version of AWS v4 Signer library.
This might be useful to address problems in the package structure, or any other reason to have
to release a new version of these modules containing the same AWS v4 Signer version as a previously
released copy of these modules.

An example version could be `1.3.0.wildfly02` to contain AWS v4 Signer version `1.3`.

## Usage

Extract the produced module zip in the `/modules` directory of your WildFly distribution.

The modules are distributed as Maven artifacts, so when using Maven you could automate this step
for example using the `maven-dependency-plugin`.

	<plugin>
	    <artifactId>maven-dependency-plugin</artifactId>
	    <executions>
		<execution>
		    <id>unpack</id>
		    <phase>pre-integration-test</phase>
		    <goals>
		        <goal>unpack</goal>
		    </goals>
		    <configuration>
		        <artifactItems>
		            <artifactItem>
		                <groupId>org.wildfly</groupId>
		                <artifactId>wildfly-dist</artifactId>
		                <version>${wildfly.version}</version>
		                <type>zip</type>
		                <overWrite>true</overWrite>
		                <outputDirectory>${project.build.directory}/node1</outputDirectory>
		            </artifactItem>
		            <artifactItem>
		                <groupId>org.hibernate.aws-v4-signer-java-jbossmodules</groupId>
		                <artifactId>aws-v4-signer-java-jbossmodules</artifactId>
		                <version>${aws-v4-signer-java-jbossmodules.version}</version>
		                <type>zip</type>
		                <classifier>dist</classifier>
		                <overWrite>true</overWrite>
		                <outputDirectory>${project.build.directory}/node1/wildfly-${wildfly.version}/modules</outputDirectory>
		            </artifactItem>
		        </artifactItems>
		    </configuration>
		</execution>
	    </executions>
	</plugin>

This will make them available as an opt-in dependency to any application deployed on WildFly.
To enable the dependency there are various options, documented in
[Class Loading in WildFly](https://docs.jboss.org/author/display/WFLY/Class+Loading+in+WildFly).


