In the <wildfly-home>/standalone/configuration/<config-name>.xml add the following:

1) Under server/extensions add:

 <extension module="org.wildfly.extension.picketlink"/>
 
2) Under server/profile add (example configuration only!):

   <subsystem xmlns="urn:jboss:domain:picketlink-federation:2.0">
        <federation name="federation-simple-redirect-binding">
            <identity-provider name="idp-redirect.war" url="http://${jboss.bind.address:127.0.0.1}:8080/idp-redirect/" security-domain="idp" support-signatures="false" strict-post-binding="false">
                <trust>
                    <trust-domain name="${jboss.bind.address:127.0.0.1}"/>
                </trust>
            </identity-provider>
            <service-providers>
                <service-provider name="sp-redirect1.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-redirect1/" post-binding="false" support-signatures="false"/>
                <service-provider name="sp-redirect2.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-redirect2/" post-binding="false" support-signatures="false"/>
            </service-providers>
        </federation>
        <federation name="federation-redirect-with-signatures">
            <key-store file="/jbid_test_keystore.jks" password="store123" sign-key-alias="servercert" sign-key-password="test123">
                <keys>
                    <key name="servercert" host="${jboss.bind.address:localhost},127.0.0.1"/>
                </keys>
            </key-store>
            <identity-provider name="idp-redirect-sig.war" url="http://${jboss.bind.address:127.0.0.1}:8080/idp-redirect-sig/" security-domain="idp" support-signatures="true" strict-post-binding="false">
                <trust>
                    <trust-domain name="${jboss.bind.address:127.0.0.1}"/>
                </trust>
            </identity-provider>
            <service-providers>
                <service-provider name="sp-redirect-sig1.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-redirect-sig1/" post-binding="false" support-signatures="true"/>
                <service-provider name="sp-redirect-sig2.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-redirect-sig2/" post-binding="false" support-signatures="true"/>
            </service-providers>
        </federation>
        <federation name="federation-simple-post-binding">
            <identity-provider name="idp-post.war" url="http://${jboss.bind.address:127.0.0.1}:8080/idp-post/" security-domain="idp">
                <trust>
                    <trust-domain name="${jboss.bind.address:127.0.0.1}"/>
                </trust>
            </identity-provider>
            <service-providers>
                <service-provider name="sp-post1.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-post1/"/>
                <service-provider name="sp-post2.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-post2/"/>
            </service-providers>
        </federation>
        <federation name="federation-post-with-signatures">
            <key-store file="/jbid_test_keystore.jks" password="store123" sign-key-alias="servercert" sign-key-password="test123">
                <keys>
                    <key name="servercert" host="${jboss.bind.address:localhost},127.0.0.1"/>
                </keys>
            </key-store>
            <identity-provider name="idp-post-sig.war" url="http://${jboss.bind.address:127.0.0.1}:8080/idp-post-sig/" security-domain="idp" support-signatures="true">
                <trust>
                    <trust-domain name="${jboss.bind.address:127.0.0.1}"/>
                </trust>
            </identity-provider>
            <service-providers>
                <service-provider name="sp-post-sig1.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-post-sig1/" support-signatures="true"/>
                <service-provider name="sp-post-sig2.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-post-sig2/" support-signatures="true"/>
            </service-providers>
        </federation>
        <federation name="federation-with-metadata">
            <key-store file="/jbid_test_keystore.jks" password="store123" sign-key-alias="servercert" sign-key-password="test123">
                <keys>
                    <key name="servercert" host="${jboss.bind.address:localhost},127.0.0.1"/>
                </keys>
            </key-store>
            <identity-provider name="idp-metadata.war" url="http://${jboss.bind.address:127.0.0.1}:8080/idp-metadata/" security-domain="idp" support-signatures="true" support-metadata="true">
                <trust>
                    <trust-domain name="${jboss.bind.address:127.0.0.1}"/>
                </trust>
            </identity-provider>
            <service-providers>
                <service-provider name="sp-metadata1.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-metadata1/" support-signatures="true" support-metadata="true"/>
                <service-provider name="sp-metadata2.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sp-metadata2/" support-signatures="true" support-metadata="true"/>
            </service-providers>
        </federation>
        <federation name="federation-with-external-idp">
            <identity-provider name="idp-external" url="http://idp.external.com/" external="true"/>
            <service-providers>
                <service-provider name="sales-on-external.war" security-domain="sp" url="http://${jboss.bind.address:127.0.0.1}:8080/sales-on-external/" post-binding="false" support-signatures="false"/>
            </service-providers>
        </federation>
    </subsystem>
    
    <subsystem xmlns="urn:jboss:domain:picketlink-identity-management:2.0">

        <!-- A complete configuration using a file-based identity store. -->
        <partition-manager jndi-name="picketlink/FileCompletePartitionManager" name="file.complete.partition.manager">
            <identity-configuration name="file.config">
                <file-store relative-to="jboss.server.data.dir" working-dir="pl-idm-complete" always-create-files="true" async-write="true"
                            async-write-thread-pool="10">
                    <supported-types supports-all="true"/>
                </file-store>
            </identity-configuration>
        </partition-manager>

        <!-- A simple configuration using a file-based identity store. -->
        <partition-manager jndi-name="picketlink/FileSimplePartitionManager" name="file.simple.partition.manager">
            <identity-configuration name="file.config">
                <file-store>
                    <supported-types supports-all="true"/>
                </file-store>
            </identity-configuration>
        </partition-manager>

        <!-- A configuration using a JPA-based identity store. The store is configured using a existing datasource. -->
        <partition-manager jndi-name="picketlink/JPADSBasedPartitionManager" name="jpa.ds.based.partition.manager">
            <identity-configuration name="jpa.config">
                <jpa-store data-source="jboss/datasources/ExampleDS">
                    <supported-types supports-all="true"/>
                </jpa-store>
            </identity-configuration>
        </partition-manager>

        <!-- A configuration using a JPA-based identity store. The store is configured using a existing JPA EntityManagerFactory, obtained via JNDI. -->
        <partition-manager jndi-name="picketlink/JPAEMFBasedPartitionManager" name="jpa.emf.based.partition.manager">
            <identity-configuration name="jpa.config">
                <jpa-store entity-manager-factory="jboss/TestingIDMEMF">
                    <supported-types>
                        <supported-type name="Partition"  code="Partition"/>
                        <supported-type name="IdentityType"  code="IdentityType"/>
                        <supported-type name="Relationship"  code="Relationship"/>
                    </supported-types>
                </jpa-store>
          </identity-configuration>
        </partition-manager>

        <!-- A configuration using a JPA-based identity store. The store is configured using a existing JPA Persistence Unit from a static module. -->
        <partition-manager jndi-name="picketlink/JPAEMFModulePartitionManager" name="jpa.emf.modules.partition.manager">
            <identity-configuration name="jpa.config">
                <jpa-store entity-module="my.module" entity-module-unit-name="my-persistence-unit-name">
                    <supported-types supports-all="true"/>
                </jpa-store>
          </identity-configuration>
        </partition-manager>

        <!-- A configuration using a LDAP-based identity store. -->
        <partition-manager name="ldap.idm" jndi-name="picketlink/LDAPBasedPartitionManager">
            <identity-configuration name="ldap.store">
                <ldap-store url="ldap://${jboss.bind.address:127.0.0.1}:10389" bind-dn="uid=admin,ou=system" bind-credential="secret" base-dn-suffix="dc=jboss,dc=org">
                    <supported-types supports-all="false">
                        <supported-type name="IdentityType"  code="IdentityType"/>
                        <supported-type name="Relationship"  code="Relationship"/>
                    </supported-types>
                    <mappings>
                        <mapping name="Agent" code="Agent" base-dn-suffix="ou=Agent,dc=jboss,dc=org" object-classes="account">
                            <attribute name="loginName" ldap-name="uid" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="User" code="User" base-dn-suffix="ou=People,dc=jboss,dc=org" object-classes="inetOrgPerson, organizationalPerson">
                            <attribute name="loginName" ldap-name="uid" is-identifier="true" />
                            <attribute name="firstName" ldap-name="cn" />
                            <attribute name="lastName" ldap-name="sn" />
                            <attribute name="email" ldap-name="mail" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="Role" code="Role" base-dn-suffix="ou=Roles,dc=jboss,dc=org" object-classes="groupOfNames">
                            <attribute name="name" ldap-name="cn" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp"  read-only="true"/>
                        </mapping>
                        <mapping name="Group" code="Group" base-dn-suffix="ou=Groups,dc=jboss,dc=org" object-classes="groupOfNames">
                            <attribute name="name" ldap-name="cn" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="Grant" code="Grant" relates-to="Role">
                            <attribute name="assignee" ldap-name="member" />
                        </mapping>
                    </mappings>
                </ldap-store>
            </identity-configuration>
        </partition-manager>

        <!-- A configuration using a LDAP and JPA identity store. In this example we use JPA to store relationships and LDAP for users, roles and groups. -->
        <partition-manager name="multiple.store.idm" jndi-name="picketlink/MultipleStoreBasedPartitionManager">
            <identity-configuration name="multiple.store">
                <jpa-store data-source="jboss/datasources/ExampleDS" support-credential="false" support-attribute="false">
                    <supported-types supports-all="false">
                        <supported-type name="Relationship"  class-name="org.picketlink.idm.model.Relationship"/>
                    </supported-types>
                </jpa-store>
                <ldap-store url="ldap://${jboss.bind.address:127.0.0.1}:10389" bind-dn="uid=admin,ou=system" bind-credential="secret" base-dn-suffix="dc=jboss,dc=org" support-attribute="false" support-credential="true">
                    <supported-types supports-all="false">
                        <supported-type name="IdentityType"  code="IdentityType"/>
                    </supported-types>
                    <mappings>
                        <mapping name="Agent" code="Agent" base-dn-suffix="ou=Agent,dc=jboss,dc=org" object-classes="account">
                            <attribute name="loginName" ldap-name="uid" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="User" code="User" base-dn-suffix="ou=People,dc=jboss,dc=org" object-classes="inetOrgPerson, organizationalPerson">
                            <attribute name="loginName" ldap-name="uid" is-identifier="true" />
                            <attribute name="firstName" ldap-name="cn" read-only="false"/>
                            <attribute name="lastName" ldap-name="sn" read-only="false"/>
                            <attribute name="email" ldap-name="mail" read-only="false"/>
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="Role" code="Role" base-dn-suffix="ou=Roles,dc=jboss,dc=org" object-classes="groupOfNames">
                            <attribute name="name" ldap-name="cn" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                        <mapping name="Group" code="Group" base-dn-suffix="ou=Groups,dc=jboss,dc=org" object-classes="groupOfNames">
                            <attribute name="name" ldap-name="cn" is-identifier="true" />
                            <attribute name="createdDate" ldap-name="createTimeStamp" read-only="true"/>
                        </mapping>
                    </mappings>
                </ldap-store>
            </identity-configuration>
        </partition-manager>

        <!-- A configuration using two identity configurations. Each identity configuration has a JPA store. -->
        <partition-manager jndi-name="picketlink/MultipleConfigurationPartitionManager" name="multiple.config.partition.manager">
            <identity-configuration name="partition.jboss.config">
                <jpa-store data-source="jboss/datasources/JBossIdentityDS">
                    <supported-types supports-all="true"/>
                </jpa-store>
            </identity-configuration>
            <identity-configuration name="partition.redhat.config">
                <jpa-store data-source="jboss/datasources/RedHatIdentityDS">
                    <supported-types supports-all="true"/>
                </jpa-store>
            </identity-configuration>
        </partition-manager>

    </subsystem>
    
