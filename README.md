To write a success unit test case for the `ObjectProfileController`, we need to focus on testing the `getTemplateProfile` method. This method retrieves template profile details based on the provided template name and version. The success scenario implies that the method should return a valid list of template profiles without errors.

Here's a sample unit test case in the format you're looking for, using `MockMvc`:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.mockito.Mockito.when;

import java.util.ArrayList;
import java.util.List;

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @InjectMocks
    private ObjectProfileController objectProfileController;

    @BeforeEach
    public void setUp() {
        mockMvc = MockMvcBuilders.standaloneSetup(objectProfileController).build();
    }

    @Test
    public void getTemplateProfile_Success() throws Exception {
        String VALID_TEMPLATE_NAME = "SampleTemplate";
        Integer VALID_TEMPLATE_VERSION = 1;

        List<TemplateProfileDto> templateProfileList = new ArrayList<>();
        templateProfileList.add(new TemplateProfileDto(/* ... */)); // Add mock data

        when(templateProfileService.getTemplatesByNameAndVersion(VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
            .thenReturn(templateProfileList);

        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION)
            .contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk());
    }
}
```

In this test case:

- **Setup (`setUp` method)**: Initializes `MockMvc` with the `ObjectProfileController`. The `@InjectMocks` annotation is used to inject the mocked dependencies into the controller.
- **Test Method (`getTemplateProfile_Success`)**: This method tests the success scenario. It mocks the `getTemplatesByNameAndVersion` method of the `TemplateProfileService` to return a predefined list of `TemplateProfileDto` objects. The `MockMvc` performs a `GET` request to the controller's endpoint, and expects an `OK` status in response.

Remember to replace `TemplateProfileDto` and any other mock data with the appropriate classes and data structures used in your application.



// Assuming TemplateProfileDto has properties like id, name, description, etc.
TemplateProfileDto mockTemplateProfileDto = new TemplateProfileDto();
mockTemplateProfileDto.setId(1); // Set the id
mockTemplateProfileDto.setName("SampleTemplate");
mockTemplateProfileDto.setDescription("This is a sample template description.");
// ... set other properties as needed

List<TemplateProfileDto> templateProfileList = new ArrayList<>();
templateProfileList.add(mockTemplateProfileDto);

