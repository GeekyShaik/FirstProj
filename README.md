

> Task :bootBuildInfo
> Task :generateEffectiveLombokConfig UP-TO-DATE
> Task :compileJava UP-TO-DATE
> Task :processResources UP-TO-DATE
> Task :classes
> Task :generateTestEffectiveLombokConfig UP-TO-DATE
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
> Task :test
Picked up JAVA_TOOL_OPTIONS: -agentpath:"C:\WINDOWS\system32\Aternity\Java\JavaHookLoader.dll"="C:\ProgramData\Aternity\hooks"

OpenJDK 64-Bit Server VM warning: Sharing is only supported for boot loader classes because bootstrap classpath has been appended

09:29:17.505 [Test worker] INFO org.springframework.test.context.support.AnnotationConfigContextLoaderUtils -- Could not detect default configuration classes for test class [com.usaa.ect.TemplateProfileControllerTest]: TemplateProfileControllerTest does not declare any static, non-private, non-final, nested classes annotated with @Configuration.
09:29:17.904 [Test worker] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper -- Found @SpringBootConfiguration com.usaa.ect.V1MetadataServicesApplication for test class com.usaa.ect.TemplateProfileControllerTest

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.1.0)

2024-02-07T09:29:19.746-06:00  INFO {} 23928 --- [    Test worker] c.u.ect.TemplateProfileControllerTest    : Starting TemplateProfileControllerTest using Java 17.0.6 with PID 23928 (started by PL5G249 in C:\Users\PL5G249\IdeaProjects\v1-metadata-services)
2024-02-07T09:29:19.750-06:00  INFO {} 23928 --- [    Test worker] c.u.ect.TemplateProfileControllerTest    : The following 2 profiles are active: "dev", "dev-secrets"
2024-02-07T09:29:27.147-06:00  INFO {} 23928 --- [    Test worker] o.s.b.t.m.w.SpringBootMockServletContext : Initializing Spring TestDispatcherServlet ''
2024-02-07T09:29:27.148-06:00  INFO {} 23928 --- [    Test worker] o.s.t.web.servlet.TestDispatcherServlet  : Initializing Servlet ''
2024-02-07T09:29:27.152-06:00  INFO {} 23928 --- [    Test worker] o.s.t.web.servlet.TestDispatcherServlet  : Completed initialization in 4 ms
2024-02-07T09:29:27.631-06:00  INFO {} 23928 --- [    Test worker] c.u.ect.TemplateProfileControllerTest    : Started TemplateProfileControllerTest in 9.471 seconds (process running for 13.105)

Request processing failed: java.lang.RuntimeException: Unexpected error
jakarta.servlet.ServletException: Request processing failed: java.lang.RuntimeException: Unexpected error
	at app//org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1019)
	at app//org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:903)
	at app//jakarta.servlet.http.HttpServlet.service(HttpServlet.java:564)
	at app//org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:885)
	at app//org.springframework.test.web.servlet.TestDispatcherServlet.service(TestDispatcherServlet.java:72)
	at app//jakarta.servlet.http.HttpServlet.service(HttpServlet.java:658)
	at app//org.springframework.mock.web.MockFilterChain$ServletFilterProxy.doFilter(MockFilterChain.java:165)
	at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
	at app//org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)
	at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
	at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
	at app//org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)
	at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
	at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
	at app//org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)
	at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
	at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
	at app//org.springframework.test.web.servlet.MockMvc.perform(MockMvc.java:201)
	at app//com.usaa.ect.TemplateProfileControllerTest.getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException(TemplateProfileControllerTest.java:169)
	at java.base@17.0.6/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base@17.0.6/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
	at java.base@17.0.6/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base@17.0.6/java.lang.reflect.Method.invoke(Method.java:568)
	at app//org.junit.platform.commons.util.ReflectionUtils.invokeMethod(ReflectionUtils.java:727)
	at app//org.junit.jupiter.engine.execution.MethodInvocation.proceed(MethodInvocation.java:60)
	at app//org.junit.jupiter.engine.execution.InvocationInterceptorChain$ValidatingInvocation.proceed(InvocationInterceptorChain.java:131)
	at app//org.junit.jupiter.engine.extension.TimeoutExtension.intercept(TimeoutExtension.java:156)
	at app//org.junit.jupiter.engine.extension.TimeoutExtension.interceptTestableMethod(TimeoutExtension.java:147)
	at app//org.junit.jupiter.engine.extension.TimeoutExtension.interceptTestMethod(TimeoutExtension.java:86)
	at app//org.junit.jupiter.engine.execution.InterceptingExecutableInvoker$ReflectiveInterceptorCall.lambda$ofVoidMethod$0(InterceptingExecutableInvoker.java:103)
	at app//org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.lambda$invoke$0(InterceptingExecutableInvoker.java:93)
	at app//org.junit.jupiter.engine.execution.InvocationInterceptorChain$InterceptedInvocation.proceed(InvocationInterceptorChain.java:106)
	at app//org.junit.jupiter.engine.execution.InvocationInterceptorChain.proceed(InvocationInterceptorChain.java:64)
	at app//org.junit.jupiter.engine.execution.InvocationInterceptorChain.chainAndInvoke(InvocationInterceptorChain.java:45)
	at app//org.junit.jupiter.engine.execution.InvocationInterceptorChain.invoke(InvocationInterceptorChain.java:37)
	at app//org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.invoke(InterceptingExecutableInvoker.java:92)
	at app//org.junit.jupiter.engine.execution.InterceptingExecutableInvoker.invoke(InterceptingExecutableInvoker.java:86)
	at app//org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.lambda$invokeTestMethod$7(TestMethodTestDescriptor.java:217)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.invokeTestMethod(TestMethodTestDescriptor.java:213)
	at app//org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.execute(TestMethodTestDescriptor.java:138)
	at app//org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.execute(TestMethodTestDescriptor.java:68)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:151)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at app//org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at java.base@17.0.6/java.util.ArrayList.forEach(ArrayList.java:1511)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at app//org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at java.base@17.0.6/java.util.ArrayList.forEach(ArrayList.java:1511)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at app//org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.submit(SameThreadHierarchicalTestExecutorService.java:35)
	at app//org.junit.platform.engine.support.hierarchical.HierarchicalTestExecutor.execute(HierarchicalTestExecutor.java:57)
	at app//org.junit.platform.engine.support.hierarchical.HierarchicalTestEngine.execute(HierarchicalTestEngine.java:54)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:107)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:88)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.lambda$execute$0(EngineExecutionOrchestrator.java:54)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.withInterceptedStreams(EngineExecutionOrchestrator.java:67)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:52)
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:114)
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:86)
	at org.junit.platform.launcher.core.DefaultLauncherSession$DelegatingLauncher.execute(DefaultLauncherSession.java:86)
	at org.junit.platform.launcher.core.SessionPerRequestLauncher.execute(SessionPerRequestLauncher.java:53)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor$CollectAllTestClassesExecutor.processAllTestClasses(JUnitPlatformTestClassProcessor.java:99)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor$CollectAllTestClassesExecutor.access$000(JUnitPlatformTestClassProcessor.java:79)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor.stop(JUnitPlatformTestClassProcessor.java:75)
	at org.gradle.api.internal.tasks.testing.SuiteTestClassProcessor.stop(SuiteTestClassProcessor.java:62)
	at java.base@17.0.6/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base@17.0.6/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
	at java.base@17.0.6/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base@17.0.6/java.lang.reflect.Method.invoke(Method.java:568)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:36)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
	at org.gradle.internal.dispatch.ContextClassLoaderDispatch.dispatch(ContextClassLoaderDispatch.java:33)
	at org.gradle.internal.dispatch.ProxyDispatchAdapter$DispatchingInvocationHandler.invoke(ProxyDispatchAdapter.java:94)
	at jdk.proxy2/jdk.proxy2.$Proxy5.stop(Unknown Source)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker$3.run(TestWorker.java:193)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.executeAndMaintainThreadName(TestWorker.java:129)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.execute(TestWorker.java:100)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.execute(TestWorker.java:60)
	at org.gradle.process.internal.worker.child.ActionExecutionWorker.execute(ActionExecutionWorker.java:56)
	at org.gradle.process.internal.worker.child.SystemApplicationClassLoaderWorker.call(SystemApplicationClassLoaderWorker.java:113)
	at org.gradle.process.internal.worker.child.SystemApplicationClassLoaderWorker.call(SystemApplicationClassLoaderWorker.java:65)
	at app//worker.org.gradle.process.internal.worker.GradleWorkerMain.run(GradleWorkerMain.java:69)
	at app//worker.org.gradle.process.internal.worker.GradleWorkerMain.main(GradleWorkerMain.java:74)
Caused by: java.lang.RuntimeException: Unexpected error
	at com.usaa.ect.service.TemplateProfileService.getTemplatesByNameAndVersion(TemplateProfileService.java:86)
	at com.usaa.ect.controller.ObjectProfileController.getTemplateProfile(ObjectProfileController.java:91)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:568)
	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:207)
	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:152)
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:118)
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:884)
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:797)
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1081)
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:974)
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1011)
	... 103 more


TemplateProfileControllerTest > getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException() FAILED
    jakarta.servlet.ServletException: Request processing failed: java.lang.RuntimeException: Unexpected error
        at app//org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1019)
        at app//org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:903)
        at app//jakarta.servlet.http.HttpServlet.service(HttpServlet.java:564)
        at app//org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:885)
        at app//org.springframework.test.web.servlet.TestDispatcherServlet.service(TestDispatcherServlet.java:72)
        at app//jakarta.servlet.http.HttpServlet.service(HttpServlet.java:658)
        at app//org.springframework.mock.web.MockFilterChain$ServletFilterProxy.doFilter(MockFilterChain.java:165)
        at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
        at app//org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)
        at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
        at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
        at app//org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)
        at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
        at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
        at app//org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)
        at app//org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116)
        at app//org.springframework.mock.web.MockFilterChain.doFilter(MockFilterChain.java:132)
        at app//org.springframework.test.web.servlet.MockMvc.perform(MockMvc.java:201)
        at app//com.usaa.ect.TemplateProfileControllerTest.getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException(TemplateProfileControllerTest.java:169)
        Caused by:
        java.lang.RuntimeException: Unexpected error
            at com.usaa.ect.service.TemplateProfileService.getTemplatesByNameAndVersion(TemplateProfileService.java:86)
            at com.usaa.ect.controller.ObjectProfileController.getTemplateProfile(ObjectProfileController.java:91)
            at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77)
            at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.base/java.lang.reflect.Method.invoke(Method.java:568)
            at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:207)
            at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:152)
            at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:118)
            at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:884)
            at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:797)
            at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)
            at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1081)
            at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:974)
            at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1011)
            ... 18 more
1 test completed, 1 failed
> Task :test FAILED
FAILURE: Build failed with an exception.
* What went wrong:
Execution failed for task ':test'.
> There were failing tests. See the report at: file:///C:/Users/PL5G249/IdeaProjects/v1-metadata-services/build/reports/tests/test/index.html
* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.
* Get more help at https://help.gradle.org

Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.6.1/userguide/command_line_interface.html#sec:command_line_warnings
BUILD FAILED in 22s
7 actionable tasks: 3 executed, 4 up-to-date

