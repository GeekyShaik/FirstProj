Certainly! Here are full test cases for response codes 400, 401, and 403 in your Spring Boot application:

1. **Response Code 400 - Bad Request**:
   
   ```java
   @Test
   public void getTemplateProfile_Returns400BadRequest() throws Exception {
       // Arrange: Prepare invalid request parameters
       String invalidTemplateName = "";  // An invalid template name

       // Act & Assert: Make a request with invalid parameters and expect a 400 status code
       mockMvc.perform(get("/template-details/template-name/" + invalidTemplateName + "/template-version/1")
           .accept(MediaType.APPLICATION_JSON))
           .andExpect(status().isBadRequest());
   }
   ```

2. **Response Code 401 - Unauthorized**:

   ```java
   @Test
   public void getTemplateProfile_Returns401Unauthorized() throws Exception {
       // Arrange: Mock a scenario where the user is unauthorized
       when(templateProfileService.getTemplatesByNameAndVersion("UnauthorizedTemplate", 1))
           .thenThrow(new UnauthorizedAccessException("Unauthorized"));

       // Act & Assert: Make a request and expect a 401 status code
       mockMvc.perform(get("/template-details/template-name/UnauthorizedTemplate/template-version/1")
           .accept(MediaType.APPLICATION_JSON))
           .andExpect(status().isUnauthorized());
   }
   ```

3. **Response Code 403 - Forbidden**:

   ```java
   @Test
   public void getTemplateProfile_Returns403Forbidden() throws Exception {
       // Arrange: Mock a scenario where the user is forbidden to access the resource
       when(templateProfileService.getTemplatesByNameAndVersion("ForbiddenTemplate", 1))
           .thenThrow(new ForbiddenAccessException("Forbidden"));

       // Act & Assert: Make a request and expect a 403 status code
       mockMvc.perform(get("/template-details/template-name/ForbiddenTemplate/template-version/1")
           .accept(MediaType.APPLICATION_JSON))
           .andExpect(status().isForbidden());
   }
   ```

These test cases cover scenarios where your application should return response codes 400 (Bad Request), 401 (Unauthorized), and 403 (Forbidden) based on different conditions or exceptions. Adjust the scenarios and exception types as needed to match your application's logic.
