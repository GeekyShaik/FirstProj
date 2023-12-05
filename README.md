2023-12-05T05:30:57.572-06:00  WARN {} 18304 --- [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [v1#enterprise#metadata-services] appears to have started a thread named [BatchSpanProcessor_WorkerThread-1] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base@17.0.6/jdk.internal.misc.Unsafe.park(Native Method)
 java.base@17.0.6/java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:252)
 java.base@17.0.6/java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:1672)
 java.base@17.0.6/java.util.concurrent.ArrayBlockingQueue.poll(ArrayBlockingQueue.java:435)
 app//io.opentelemetry.sdk.trace.export.BatchSpanProcessor$Worker.run(BatchSpanProcessor.java:252)
 java.base@17.0.6/java.lang.Thread.run(Thread.java:833)
2023-12-05T05:30:57.592-06:00  INFO {} 18304 --- [  restartedMain] .s.b.a.l.ConditionEvaluationReportLogger : 

Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2023-12-05T05:30:57.623-06:00 ERROR {} 18304 --- [  restartedMain] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.NullPointerException: Cannot invoke "org.hibernate.metamodel.mapping.EmbeddableValuedModelPart.getNavigableRole()" because "mappingModelPart" is null
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
Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.NullPointerException: Cannot invoke "org.hibernate.metamodel.mapping.EmbeddableValuedModelPart.getNavigableRole()" because "mappingModelPart" is null
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:421)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:396)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:352)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1816)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1766)
	... 21 common frames omitted
Caused by: java.lang.NullPointerException: Cannot invoke "org.hibernate.metamodel.mapping.EmbeddableValuedModelPart.getNavigableRole()" because "mappingModelPart" is null
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.lambda$registerEmbeddableMappingType$1(MappingMetamodelImpl.java:229)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
	at org.hibernate.boot.internal.MetadataImpl.visitRegisteredComponents(MetadataImpl.java:570)
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.registerEmbeddableMappingType(MappingMetamodelImpl.java:225)
	at org.hibernate.metamodel.model.domain.internal.MappingMetamodelImpl.finishInitialization(MappingMetamodelImpl.java:210)
	at org.hibernate.internal.SessionFactoryImpl.initializeMappingModel(SessionFactoryImpl.java:319)
	at org.hibernate.internal.SessionFactoryImpl.<init>(SessionFactoryImpl.java:269)
	at org.hibernate.boot.internal.SessionFactoryBuilderImpl.build(SessionFactoryBuilderImpl.java:431)
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1455)
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:66)
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:376)
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:409)
	... 25 common frames omitted

Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is java.lang.NullPointerException: Cannot invoke "org.hibernate.metamodel.mapping.EmbeddableValuedModelPart.getNavigableRole()" because "mappingModelPart" is null

Caused by: java.lang.NullPointerException: Cannot invoke "org.hibernate.metamodel.mapping.EmbeddableValuedModelPart.getNavigableRole()" because "mappingModelPart" is null

