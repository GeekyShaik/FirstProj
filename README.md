
Certainly! Based on your controller, a successful test case would involve mocking the service layer to return a list of `TemplateProfileDto` objects when provided with valid template name and version arguments. Then, you would perform a GET request to your controller's endpoint with those arguments and assert that the response is OK and that the content matches the expected result.

Here is a template for a success test case:

```java
import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.setup.MockMvcBuilders.webAppContextSetup;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.web.context.WebApplicationContext;

// ... other imports ...

@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private WebApplicationContext webApplicationContext;

    @MockBean
    private TemplateProfileService templateProfileService;

    private static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    private static final String VALID_TEMPLATE_NAME = "ECM Delivery NewBrand";
    private static final Integer VALID_TEMPLATE_VERSION = 1;

    @BeforeEach
    public void setup() {
        this.mockMvc = webAppContextSetup(webApplicationContext).build();
    }

    @Test
    public void getTemplateProfile_Success() throws Exception {
        // Create a mock response
        List<TemplateProfileDto> mockTemplateProfileList = new ArrayList<>();
        TemplateProfileDto mockTemplateProfileDto = new TemplateProfileDto();
        // ... set properties on mockTemplateProfileDto as needed ...
        mockTemplateProfileList.add(mockTemplateProfileDto);

        // Mocking the service call
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
            .thenReturn(mockTemplateProfileList);

        // Perform the request and check for OK status and valid content type
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
            .andExpect(status().isOk())
            .andExpect(content().contentType(MediaType.APPLICATION_JSON));
    }
}
```

In this test case:

- `@WebMvcTest(ObjectProfileController.class)` is used to set up the MVC context for testing only `ObjectProfileController`.
- `@MockBean` is used to create and inject a mock `TemplateProfileService` into the application context.
- The `VALID_TEMPLATE_NAME` and `VALID_TEMPLATE_VERSION` constants represent valid test parameters.
- The `when(...).thenReturn(...)` is used to define the behavior of the mocked service when called with any string or integer, which should be replaced with specific values if you want more precise control.
- The `mockMvc.perform(...)` method is used to simulate an HTTP GET request, and `andExpect(...)` methods are used to assert the conditions of the response.

Please make sure to adapt the content type and response content assertions according to your actual response structure. If you are returning a JSON object or array, you should assert the JSON content as well.