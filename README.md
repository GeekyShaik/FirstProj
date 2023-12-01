rg.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.reflect.InaccessibleObjectException: Unable to make field private final byte java.lang.String.coder accessible: module java.base does not "opens java.lang" to unnamed module @6c7b82f1
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1770)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:598)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:520)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:326)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:324)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:200)
	at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1156)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:931)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:608)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:733)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:435)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:311)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1305)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1294)
	at com.usaa.ect.V1MetadataServicesApplication.main(V1MetadataServicesApplication.java:14)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:568)
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49)
Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.reflect.InaccessibleObjectException: Unable to make field private final byte java.lang.String.coder accessible: module java.base does not "opens java.lang" to unnamed module @6c7b82f1
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:421)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:396)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:352)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1816)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1766)
	... 21 common frames omitted
Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make field private final byte java.lang.String.coder accessible: module java.base does not "opens java.lang" to unnamed module @6c7b82f1
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:354)
	at java.base/java.lang.reflect.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:297)
	at java.base/java.lang.reflect.Field.checkCanSetAccessible(Field.java:178)
	at java.base/java.lang.reflect.Field.setAccessible(Field.java:172)
	at org.hibernate.internal.util.ReflectHelper.ensureAccessibility(ReflectHelper.java:436)
	at org.hibernate.internal.util.ReflectHelper.findField(ReflectHelper.java:426)
	at org.hibernate.property.access.internal.PropertyAccessFieldImpl.<init>(PropertyAccessFieldImpl.java:34)
	at org.hibernate.property.access.internal.PropertyAccessStrategyFieldImpl.buildPropertyAccess(PropertyAccessStrategyFieldImpl.java:26)
	at org.hibernate.metamodel.internal.EmbeddableRepresentationStrategyPojo.buildPropertyAccess(EmbeddableRepresentationStrategyPojo.java:164)
	at org.hibernate.metamodel.internal.AbstractEmbeddableRepresentationStrategy.<init>(AbstractEmbeddableRepresentationStrategy.java:44)
	at org.hibernate.metamodel.internal.EmbeddableRepresentationStrategyPojo.<init>(EmbeddableRepresentationStrategyPojo.java:56)
	at org.hibernate.metamodel.internal.ManagedTypeRepresentationResolverStandard.resolveStrategy(ManagedTypeRepresentationResolverStandard.java:156)
	at org.hibernate.metamodel.mapping.internal.EmbeddableMappingTypeImpl.<init>(EmbeddableMappingTypeImpl.java:162)
	at org.hibernate.metamodel.mapping.internal.EmbeddableMappingTypeImpl.from(EmbeddableMappingTypeImpl.java:112)
Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.reflect.InaccessibleObjectException: Unable to make field private final byte java.lang.String.coder accessible: module java.base does not "opens java.lang" to unnamed module @6c7b82f1

	at org.hibernate.metamodel.mapping.internal.MappingModelCreationHelper.buildEncapsulatedCompositeIdentifierMapping(MappingModelCreationHelper.java:135)
	at org.hibernate.persister.entity.AbstractEntityPersister.generateIdentifierMapping(AbstractEntityPersister.java:5030)
	at org.hibernate.persister.entity.AbstractEntityPersister.lambda$prepareMappingModel$19(AbstractEntityPersister.java:4735)
	at org.hibernate.metamodel.mapping.internal.MappingModelCreationProcess.processSubPart(MappingModelCreationProcess.java:163)
	at org.hibernate.persister.entity.AbstractEntityPersister.prepareMappingModel(AbstractEntityPersister.java:4733)
	at org.hibernate.persister.entity.AbstractEntityPersister.prepareMappingModel(AbstractEntityPersister.java:4573)
	at org.hibernate.metamodel.mapping.internal.MappingModelCreationProcess.execute(MappingModelCreationProcess.java:84)
	at org.hibernate.metamodel.mapping.internal.MappingModelCreationProcess.process(MappingModelCreationProcess.java:42)
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.finishInitialization(MappingMetamodelImpl.java:201)
	at org.hibernate.internal.SessionFactoryImpl.initializeMappingModel(SessionFactoryImpl.java:319)
	at org.hibernate.internal.SessionFactoryImpl.<init>(SessionFactoryImpl.java:269)
	at org.hibernate.boot.internal.SessionFactoryBuilderImpl.build(SessionFactoryBuilderImpl.java:431)
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1455)
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:66)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:376)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:409)
	... 25 common frames omitted

Caused by: java.lang.reflect.InaccessibleObjectException: Unable to make field private final byte java.lang.String.coder accessible: module java.base does not "opens java.lang" to unnamed module @6c7b82f1


Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.6.1/userguide/command_line_interface.html#sec:command_line_warnings

