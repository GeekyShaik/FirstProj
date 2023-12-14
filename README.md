Certainly! I'll provide a more detailed example of the `ObjectProfileControllerTest` class using JUnit 5, including the necessary import statements and corrected endpoints. Please ensure that the endpoint paths match those defined in your controller class.

### ObjectProfileController Tests (JUnit 5)

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

import java.util.Collections;
import java.util.List;

import static org.mockito.Mockito.when;
import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.ArgumentMatchers.anyInt;

import your.package.TemplateProfileService; // Replace with your actual package
import your.package.TemplateProfileDto; // Replace with your actual package

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class) // Make sure ObjectProfileController is in your.package
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    // Adjust the endpoints as per your controller mappings
    private static final String TEMPLATE_DETAILS_ENDPOINT = "/template-details/template-name/{templateName}/template-version/{templateVersion}";

    @Test
    public void whenValidInput_thenReturns200() throws Exception {
        List<TemplateProfileDto> dtoList = Collections.singletonList(new TemplateProfileDto());
        when(templateProfileService.getTemplatesByNameAndVersion("validName", 1)).thenReturn(dtoList);

        mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, "validName", 1))
                .andExpect(status().isOk())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$", hasSize(1)));
    }

    @Test
    public void whenInvalidInput_thenReturns400() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, " ", -1))
                .andExpect(status().isBadRequest());
    }

    @Test
    public void whenDataNotFound_thenReturns404() throws Exception