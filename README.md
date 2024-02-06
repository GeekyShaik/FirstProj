Below are implementations for some of the suggested test cases based on the `TemplateProfileControllerTest` class structure provided earlier. These implementations assume you are familiar with Mockito for mocking dependencies and Spring's `MockMvc` for making requests to your controller in a test environment.

### Test Case 1: Successful Retrieval of Template Profile

```java
@Test
public void getTemplateProfile_Returns200_WithValidData() throws Exception {
    TemplateProfileDto dto = createTemplateProfileDto(); // Assuming this method sets up a valid DTO as shown earlier
    List<TemplateProfileDto> mockResponse = Collections.singletonList(dto);

    when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
            .thenReturn(mockResponse);

    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].templateName", is("ECM_Delivery_NewBrnd")))
            .andExpect(jsonPath("$[0].templateVersion", is(1)));
}
```

### Test Case 2: Template Not Found

```java
@Test
public void getTemplateProfile_Returns404_WhenTemplateNotFound() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("NonExistentTemplate", 1))
            .thenReturn(Collections.emptyList());

    mockMvc.perform(get("/template-details/template-name/NonExistentTemplate/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isNotFound());
}
```

### Test Case 3: Invalid Template Name

This case might be more relevant if you have specific validation on your path variables that you can test directly in the controller. Spring automatically returns a 400 Bad Request for path variables that don't match the expected format, so you might not need to explicitly write a test for this unless you have custom validation logic.

### Test Case 4: Invalid Template Version

Similar to Test Case 3, testing for invalid path variable formats is typically handled by Spring MVC directly.

### Test Case 5: Database Error

```java
@Test
public void getTemplateProfile_Returns500_OnDatabaseError() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
            .thenThrow(new RuntimeException("Database error"));

    mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isInternalServerError());
}
```

### Test Case 6: Empty Response

```java
@Test
public void getTemplateProfile_Returns404_WhenNoDataFound() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion("EmptyResponse", 1))
            .thenReturn(Collections.emptyList());

    mockMvc.perform(get("/template-details/template-name/EmptyResponse/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isNotFound());
}
```

### Test Case 7-10:

The implementation of these test cases would follow a similar pattern to the ones above, adjusting the mocked service behavior and the `MockMvc` request and expectations accordingly. 

For verifying logging (Test Case 9), you might need to use a library like `Logback` with an appender that captures log entries to assertions can be made against them. However, testing logging behavior is often considered beyond the scope of unit testing and more relevant to integration testing.

For handling unexpected exceptions (Test Case 10), the setup would be similar to Test Case 5, where you mock the service to throw an unexpected exception and then assert that the controller returns a 500 Internal Server Error response.

Remember, these tests are intended to guide you through testing various scenarios your controller might encounter. Depending on the specifics of your implementation and requirements, you may need to adjust these examples accordingly.
