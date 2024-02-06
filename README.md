To create JUnit test cases for the `ObjectProfileController` based on the response codes and considering the structure of the controller, service, and global exception handling as discussed, we'll use Spring Boot's `MockMvc` to simulate HTTP requests and verify responses. These tests will cover scenarios including successful data retrieval, handling of database errors, and handling of general errors during template retrieval.

First, ensure your test class is annotated correctly to set up the web application context and mock the service layer:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import static org.mockito.BDDMockito.given;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    // Sample template data
    private TemplateProfileDto sampleTemplateProfileDto;

    @BeforeEach
    void setUp() {
        // Initialize your test data here
        sampleTemplateProfileDto = new TemplateProfileDto();
        // Set properties on sampleTemplateProfileDto as needed for testing
    }

    @Test
    void getTemplateProfile_Returns200_WithValidData() throws Exception {
        given(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
                .willReturn(List.of(sampleTemplateProfileDto));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$").isArray())
                .andExpect(jsonPath("$[0]").isNotEmpty()); // Customize based on TemplateProfileDto structure
    }

    @Test
    void getTemplateProfile_Returns404_WhenTemplateNotFound() throws Exception {
        given(templateProfileService.getTemplatesByNameAndVersion("NonExistent", 1))
                .willReturn(List.of());

        mockMvc.perform(get("/template-details/template-name/NonExistent/template-version/1"))
                .andExpect(status().isNotFound());
    }

    @Test
    void getTemplateProfile_Returns500_OnDatabaseError() throws Exception {
        given(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
                .willThrow(new TemplateDatabaseException("ValidName", 1, "Database access error", new RuntimeException()));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
                .andExpect(status().isInternalServerError())
                .andExpect(content().string("Database error for templateName: ValidName, templateVersion: 1 - Database access error"));
    }

    @Test
    void getTemplateProfile_Returns400_OnGeneralError() throws Exception {
        given(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
                .willThrow(new TemplateRetrievalException("ValidName", 1, "General error occurred while retrieving template data"));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
                .andExpect(status().isBadRequest())
                .andExpect(content().string("Error retrieving template with name: ValidName and version: 1 - General error occurred while retrieving template data"));
    }
}
```

### Notes:

- **Setup and Initialization**: In the `setUp()` method, initialize any objects or data you'll need for the tests. For example, create a `TemplateProfileDto` with sample data that a successful service call might return.
- **Mocking Service Calls**: Use `MockBean` to mock the `TemplateProfileService` and `given(...).willReturn(...)` or `willThrow(...)` to define how it should behave in each test scenario.
- **Verifying Responses**: Use `andExpect(...)` to verify the HTTP status codes, response content types, and bodies. The `jsonPath(...)` assertions can be adjusted based on the actual structure of your `TemplateProfileDto`.
- **Handling Exceptions**: For tests simulating service exceptions, the `willThrow(...)` method setups are based on the custom exceptions we defined earlier. These exceptions are mapped to HTTP responses in the `GlobalExceptionHandler`.

Ensure your project includes the necessary dependencies for Spring Boot Test, MockMvc, and Mockito if you haven't already. This setup provides a comprehensive approach to testing the various outcomes of your controller based on different service layer behaviors.
