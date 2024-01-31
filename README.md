
Creating full test cases for all conditions would require more specific information about the behaviors and exceptions your service might throw, as well as the exact structure of your `TemplateProfileDto`. However, I'll provide you with a template for each of the test cases which you can then fill in or adjust according to the specifics of your application.

1. **Success Case**: This has been implemented by you already.

2. **Not Found Case**:
```java
@Test
public void getTemplateProfile_NotFound() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("InvalidName", 1))
        .thenReturn(Collections.emptyList());

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "InvalidName", 1))
        .andExpect(status().isNotFound());
}
```

3. **Invalid Name Case**:
```java
@Test
public void getTemplateProfile_InvalidName() throws Exception {
    // Assuming the service throws a custom NotFoundException for invalid names
    when(templateProfileService.getTemplatesByNameAndVersion("InvalidName!", 1))
        .thenThrow(new NotFoundException("Invalid template name"));

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "InvalidName!", 1))
        .andExpect(status().isBadRequest());
}
```

4. **Invalid Version Case**:
```java
@Test
public void getTemplateProfile_InvalidVersion() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", -1))
        .thenThrow(new IllegalArgumentException("Invalid template version"));

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", -1))
        .andExpect(status().isBadRequest());
}
```

5. **Service Exception Case**:
```java
@Test
public void getTemplateProfile_ServiceException() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
        .thenThrow(new RuntimeException("Internal server error"));

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", 1))
        .andExpect(status().isInternalServerError());
}
```

6. **Empty Profile List Case**:
```java
@Test
public void getTemplateProfile_EmptyList() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
        .thenReturn(Collections.emptyList());

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", 1))
        .andExpect(status().isOk())
        .andExpect(content().json("[]")); // Expecting an empty JSON array
}
```

7. **Null Profile Case**:
```java
@Test
public void getTemplateProfile_NullProfile() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
        .thenReturn(null);

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", 1))
        .andExpect(status().isInternalServerError()); // Assuming your controller handles null with a 500 error
}
```

8. **Invalid Parameters Case**:
```java
@Test
public void getTemplateProfile_InvalidParameters() throws Exception {
    // No need to mock the service here as the request should fail before reaching it

    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "", "")) // Empty strings as parameters
        .andExpect(status().isBadRequest());
}
```

9. **Security Case**:
```java
@Test
@WithMockUser(roles = "NON_AUTHORIZED_ROLE")
public void getTemplateProfile_SecurityCheck() throws Exception {
    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", 1))
        .andExpect(status().isForbidden());
}
```

10. **Content Type Case**:
```java
@Test
public void getTemplateProfile_WrongContentType() throws Exception {
    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
            "ValidName", 1)
            .accept(MediaType.TEXT_PLAIN)) // Requesting text/plain instead of application/json
        .andExpect(status().isUnsupportedMediaType());
}
```

For each of these test cases, you will need to replace placeholders like `"ValidName"`, `"InvalidName"`, the exception types, and the expected status codes with values and logic that are appropriate for your application. The `@WithMockUser` annotation assumes you're using Spring Security for role-based access control.

Make sure you have the `NotFoundException` class and any other custom exceptions defined in your codebase, or replace them with the appropriate exceptions provided by your application or the Spring framework.