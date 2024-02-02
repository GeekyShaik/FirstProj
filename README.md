Given the structure of your `ObjectProfileController` and its endpoints, we can outline additional test cases for different response codes as defined in the controller. These tests will simulate various scenarios, including successful requests, client errors (e.g., bad requests), unauthorized access, forbidden access, not found responses, and server errors. Let's create a test case for each of these scenarios based on the endpoint for retrieving template profiles:

### 1. Test Case for 200 OK Response
You've already provided a template for testing a successful response. This test case ensures the endpoint returns a 200 OK status when a valid request is made.

### 2. Test Case for 400 Bad Request Response
This test case would simulate a scenario where the request parameters do not meet the validation requirements (e.g., template name length is not within the specified range).

```java
@Test
public void getTemplateProfile_WithInvalidParameters_Returns400() throws Exception {
    mockMvc.perform(get(TEMPLATE_NAME_PATH + "InvalidTemplateName" + TEMPLATE_VERSION_PATH)
                    .headers(localTestHeaders))
            .andExpect(status().isBadRequest())
            .andDo(MockMvcResultHandlers.print());
}
```

### 3. Test Case for 401 Unauthorized Response
Simulate a request without proper authentication. Given your setup, you might need to mock the security context to simulate this scenario since the actual authentication process is outside the scope of `@WebMvcTest`.

```java
@Test
public void getTemplateProfile_WithoutAuthentication_Returns401() throws Exception {
    // Assuming you have a way to simulate an unauthenticated request in your setup
}
```

### 4. Test Case for 403 Forbidden Response
This would involve a request where the user is authenticated but does not have the required role. This requires mocking the security context to simulate a user with different roles.

```java
@Test
public void getTemplateProfile_WithUnauthorizedRole_Returns403() throws Exception {
    // Assuming you have a way to simulate an authenticated request with incorrect roles
}
```

### 5. Test Case for 404 Not Found Response
Simulate a scenario where the requested template profile does not exist. This requires mocking the `templateProfileService` to return an empty list or `null`.

```java
@Test
public void getTemplateProfile_WithNonexistentTemplateName_Returns404() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyString())).thenReturn(Collections.emptyList());

    mockMvc.perform(get(TEMPLATE_NAME_PATH + "NonexistentTemplateName" + TEMPLATE_VERSION_PATH)
                    .headers(localTestHeaders))
            .andExpect(status().isNotFound())
            .andDo(MockMvcResultHandlers.print());
}
```

### 6. Test Case for 500 Internal Server Error Response
To simulate a server error, you can mock the `templateProfileService` to throw an exception when the controller calls it.

```java
@Test
public void getTemplateProfile_ServiceThrowsException_Returns500() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyString())).thenThrow(new RuntimeException("Internal server error"));

    mockMvc.perform(get(TEMPLATE_NAME_PATH + "ValidTemplateName" + TEMPLATE_VERSION_PATH)
                    .headers(localTestHeaders))
            .andExpect(status().isInternalServerError())
            .andDo(MockMvcResultHandlers.print());
}
```

These test cases cover various scenarios based on the response codes provided in your controller. Adjustments may be needed based on the actual implementation details of your authentication and authorization mechanism, and how your service layer (`templateProfileService`) is implemented and interacts with the controller.
