
> Task :bootRun
Picked up JAVA_TOOL_OPTIONS: -agentpath:"C:\WINDOWS\system32\Aternity\Java\JavaHookLoader.dll"="C:\ProgramData\Aternity\hooks"

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.1.0)

2023-12-05T07:26:22.203-06:00  INFO {} 21032 --- [  restartedMain] c.u.ect.V1MetadataServicesApplication    : Starting V1MetadataServicesApplication using Java 17.0.6 with PID 21032 (C:\Users\PL5G249\IdeaProjects\v1-metadata-services\build\classes\java\main started by PL5G249 in C:\Users\PL5G249\IdeaProjects\v1-metadata-services)
2023-12-05T07:26:22.207-06:00  INFO {} 21032 --- [  restartedMain] c.u.ect.V1MetadataServicesApplication    : The following 2 profiles are active: "dev", "dev-secrets"
2023-12-05T07:26:22.272-06:00  INFO {} 21032 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2023-12-05T07:26:22.272-06:00  INFO {} 21032 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2023-12-05T07:26:24.874-06:00  INFO {} 21032 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2023-12-05T07:26:25.134-06:00  INFO {} 21032 --- [  restartedMain] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 248 ms. Found 1 JPA repository interfaces.
2023-12-05T07:26:25.955-06:00  INFO {} 21032 --- [  restartedMain] trationDelegate$BeanPostProcessorChecker : Bean 'v1MetadataServicesApplication' of type [com.usaa.ect.V1MetadataServicesApplication$$SpringCGLIB$$0] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-12-05T07:26:25.959-06:00  INFO {} 21032 --- [  restartedMain] trationDelegate$BeanPostProcessorChecker : Bean 'grantedAuthorityDefaults' of type [org.springframework.security.config.core.GrantedAuthorityDefaults] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-12-05T07:26:25.961-06:00  INFO {} 21032 --- [  restartedMain] trationDelegate$BeanPostProcessorChecker : Bean 'io.opentelemetry.instrumentation.spring.autoconfigure.httpclients.resttemplate.RestTemplateAutoConfiguration' of type [io.opentelemetry.instrumentation.spring.autoconfigure.httpclients.resttemplate.RestTemplateAutoConfiguration$$SpringCGLIB$$0] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-12-05T07:26:25.966-06:00  INFO {} 21032 --- [  restartedMain] trationDelegate$BeanPostProcessorChecker : Bean 'io.opentelemetry.instrumentation.spring.autoconfigure.httpclients.webclient.WebClientAutoConfiguration' of type [io.opentelemetry.instrumentation.spring.autoconfigure.httpclients.webclient.WebClientAutoConfiguration$$SpringCGLIB$$0] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-12-05T07:26:26.668-06:00  INFO {} 21032 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2023-12-05T07:26:26.683-06:00  INFO {} 21032 --- [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2023-12-05T07:26:26.684-06:00  INFO {} 21032 --- [  restartedMain] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.15]
2023-12-05T07:26:26.750-06:00  INFO {} 21032 --- [  restartedMain] C.[.[.[/v1/enterprise/metadata-services] : Initializing Spring embedded WebApplicationContext
2023-12-05T07:26:26.751-06:00  INFO {} 21032 --- [  restartedMain] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 4477 ms
2023-12-05T07:26:27.592-06:00  INFO {} 21032 --- [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2023-12-05T07:26:27.986-06:00  INFO {} 21032 --- [  restartedMain] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection conn0: url=jdbc:h2:mem:devdb user=SA
2023-12-05T07:26:27.989-06:00  INFO {} 21032 --- [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2023-12-05T07:26:28.002-06:00  INFO {} 21032 --- [  restartedMain] o.s.b.a.h2.H2ConsoleAutoConfiguration    : H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:devdb'
2023-12-05T07:26:28.180-06:00 DEBUG {} 21032 --- [  restartedMain] c.u.s.s.web.UsaaForwardedHeaderFilter    : Filter 'forwardedHeaderFilter' configured for use
2023-12-05T07:26:28.289-06:00  INFO {} 21032 --- [  restartedMain] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
2023-12-05T07:26:28.406-06:00  INFO {} 21032 --- [  restartedMain] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 6.2.2.Final
2023-12-05T07:26:28.409-06:00  INFO {} 21032 --- [  restartedMain] org.hibernate.cfg.Environment            : HHH000406: Using bytecode reflection optimizer
2023-12-05T07:26:28.658-06:00  INFO {} 21032 --- [  restartedMain] o.h.b.i.BytecodeProviderInitiator        : HHH000021: Bytecode provider name : bytebuddy
2023-12-05T07:26:28.893-06:00  INFO {} 21032 --- [  restartedMain] o.s.o.j.p.SpringPersistenceUnitInfo      : No LoadTimeWeaver setup: ignoring JPA class transformer
2023-12-05T07:26:28.957-06:00  INFO {} 21032 --- [  restartedMain] org.hibernate.orm.dialect                : HHH035001: Using dialect: org.hibernate.dialect.H2Dialect, version: 2.2.220
2023-12-05T07:26:29.313-06:00  INFO {} 21032 --- [  restartedMain] o.h.b.i.BytecodeProviderInitiator        : HHH000021: Bytecode provider name : bytebuddy
2023-12-05T07:26:30.394-06:00  INFO {} 21032 --- [  restartedMain] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
2023-12-05T07:26:30.399-06:00  INFO {} 21032 --- [  restartedMain] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2023-12-05T07:26:30.457-06:00 DEBUG {} 21032 --- [  restartedMain] c.u.s.s.p.envoy.UsaaEnvoyValidator       : Trusted hosts: [0:0:0:0:0:0:0:1, 127.0.0.1, ::1, localhost]
Load UsaaSwaggerProperties
2023-12-05T07:26:30.912-06:00  INFO {} 21032 --- [  restartedMain] o.s.d.j.r.query.QueryEnhancerFactory     : Hibernate is in classpath; If applicable, HQL parser will be used.
2023-12-05T07:26:31.179-06:00  WARN {} 21032 --- [  restartedMain] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning
2023-12-05T07:26:31.526-06:00  INFO {} 21032 --- [  restartedMain] o.s.s.web.DefaultSecurityFilterChain     : Will secure any request with [org.springframework.security.web.session.DisableEncodeUrlFilter@764f43bd, org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@259840b1, org.springframework.security.web.context.SecurityContextHolderFilter@73e7d503, org.springframework.security.web.header.HeaderWriterFilter@2dbfbc03, org.springframework.security.web.authentication.logout.LogoutFilter@c23aac2, com.usaa.spring.security.web.UsaaAuthenticationFilter@343e958d, org.springframework.security.web.savedrequest.RequestCacheAwareFilter@418c8f97, org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@74c46bf3, org.springframework.security.web.authentication.AnonymousAuthenticationFilter@48248530, org.springframework.security.web.session.SessionManagementFilter@30444e7d, org.springframework.security.web.access.ExceptionTranslationFilter@6137d514, org.springframework.security.web.access.intercept.AuthorizationFilter@3444ae69]
2023-12-05T07:26:32.089-06:00  WARN {} 21032 --- [  restartedMain] ocalVariableTableParameterNameDiscoverer : Using deprecated '-debug' fallback for parameter name resolution. Compile the affected code with '-parameters' instead or avoid its introspection: io.opentelemetry.instrumentation.spring.autoconfigure.aspects.TraceAspectAutoConfiguration
2023-12-05T07:26:33.046-06:00  INFO {} 21032 --- [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2023-12-05T07:26:33.058-06:00  INFO {} 21032 --- [  restartedMain] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 3 endpoint(s) beneath base path '/actuator'
2023-12-05T07:26:33.210-06:00  INFO {} 21032 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path '/v1/enterprise/metadata-services'
2023-12-05T07:26:33.237-06:00  INFO {} 21032 --- [  restartedMain] c.u.ect.V1MetadataServicesApplication    : Started V1MetadataServicesApplication in 11.974 seconds (process running for 12.917)
