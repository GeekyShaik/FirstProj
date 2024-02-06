Continuing from the previous setup, let's add a few more test cases focusing on different response codes and scenarios that might be encountered with the `ObjectProfileController`. These tests will further ensure the robustness of the controller by covering a wider range of outcomes.

### Test for No Content Response (Assuming Such a Scenario Is Applicable)

This test assumes that there might be a scenario where a successful request does not return any content, which is not typical for the provided controller setup but is included for completeness.

```java
@Test
void getTemplateProfile_Returns204_WhenNoContentFound() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 2))
            .thenReturn(Collections.emptyList()); // Assuming an empty list triggers a 204 response

    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/2"))
            .andExpect(status().isNoContent());
}
```

### Test for Unauthorized Access

This test case assumes there's some form of security in place that might result in a 401 Unauthorized status if the requester is not properly authenticated. This scenario requires security configurations that are not part of the initial setup provided.

```java
@Test
@WithMockUser(roles = {"INVALID_ROLE"}) // Assuming Spring Security Test is set up
void getTemplateProfile_Returns401_WhenUnauthorized() throws Exception {
    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isUnauthorized());
}
```

### Test for Forbidden Access

Similar to the unauthorized test, this assumes a security context where the authenticated user does not have the necessary permissions to access the endpoint.

```java
@Test
@WithMockUser(roles = {"BASIC_USER"}) // Assuming the role does not have access
void getTemplateProfile_Returns403_Forbidden() throws Exception {
    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isForbidden());
}
```

### Test for Unsupported Media Type

This test checks the response when an unsupported media type is sent as part of the request. It's a way to ensure your controller properly handles content negotiation.

```java
@Test
void getTemplateProfile_Returns415_WhenUnsupportedMediaType() throws Exception {
    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1")
            .contentType(MediaType.APPLICATION_XML)) // Assuming JSON is the only supported type
            .andExpect(status().isUnsupportedMediaType());
}
```

### Test for Server Error Due to Malformed Request

This scenario simulates a server error triggered by a malformed request. It's more common to respond with a 400 Bad Request in such cases, but this is included to demonstrate handling unexpected conditions.

```java
@Test
void getTemplateProfile_Returns500_WhenMalformedData() throws Exception {
    // Simulating this scenario directly might require a specific setup or condition that causes an internal server error.
    // Typically, you would mock a service or component to throw an unexpected exception.
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 3))
            .thenThrow(new RuntimeException("Unexpected error"));

    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/3"))
            .andExpect(status().isInternalServerError());
}
```

### Important Notes:

- The 204 No Content and 415 Unsupported Media Type scenarios might not directly apply based on the controller code provided earlier. These are included for educational purposes to cover a broad range of HTTP responses.
- The 401 Unauthorized and 403 Forbidden tests assume that Spring Security is configured and integrated with your controller, which requires additional setup not covered in the initial code.
- The exact conditions to trigger each response will depend on your application's specific implementation details, including security configurations, content negotiation setup, and exception handling strategies.

These additional tests complete a more comprehensive suite aimed at covering a wide array of HTTP response scenarios, thereby ensuring your controller behaves correctly under various conditions.
