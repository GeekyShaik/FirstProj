import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.BeforeEach;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.Mockito.when;
import static org.mockito.ArgumentMatchers.eq;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    private static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    private static final String VALID_TEMPLATE_NAME = "ECM Delivery NewBrand";
    private static final Integer VALID_TEMPLATE_VERSION = 1;

    @BeforeEach
    public void setup() {
        // Assuming TemplateProfileDto has a no-args constructor and setters for name and version
        List<TemplateProfileDto> mockTemplateProfileList = new ArrayList<>();
        TemplateProfileDto mockTemplateProfileDto = new TemplateProfileDto();
        mockTemplateProfileDto.setTemplateName(VALID_TEMPLATE_NAME); // Replace with actual setter method name
        mockTemplateProfileDto.setTemplateVersion(VALID_TEMPLATE_VERSION); // Replace with actual setter method name
        mockTemplateProfileList.add(mockTemplateProfileDto);

        // Mock the service call for the specific name and version
        when(templateProfileService.getTemplatesByNameAndVersion(eq(VALID_TEMPLATE_NAME), eq(VALID_TEMPLATE_VERSION)))
            .thenReturn(mockTemplateProfileList);
    }

    @Test
    public void getTemplateProfile_Returns200() throws Exception {
        // Perform the request with the specific template name and version
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
            .andExpect(status().isOk()); // status().isOk() is equivalent to status().is(200)
    }
}
