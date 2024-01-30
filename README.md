import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

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
public class ObjectProfileControllerTest {

    private MockMvc mockMvc;
    private TemplateProfileService templateProfileService;

    private static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    private static final String VALID_TEMPLATE_NAME = "SampleTemplate";
    private static final int VALID_TEMPLATE_VERSION = 1;

    // Successful retrieval of template profile
    @Test
    public void getTemplateProfile_Success() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
               .andExpect(status().isOk());
    }

    // Template profile not found
    @Test
    public void getTemplateProfile_NotFound() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, "NonExistentTemplate", VALID_TEMPLATE_VERSION))
               .andExpect(status().isNotFound());
    }

    // Invalid template name (Empty)
    @Test
    public void getTemplateProfile_InvalidTemplateName_Empty() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, "", VALID_TEMPLATE_VERSION))
               .andExpect(status().isBadRequest());
    }

    // Invalid template name (Null)
    @Test
    public void getTemplateProfile_InvalidTemplateName_Null() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, null, VALID_TEMPLATE_VERSION))
               .andExpect(status().isBadRequest());
    }

    // Invalid template version (Negative)
    @Test
    public void getTemplateProfile_InvalidTemplateVersion_Negative() throws Exception {
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, VALID_TEMPLATE_NAME, -1))
               .andExpect(status().isBadRequest());
    }

    // Service layer throws an exception
    @Test
    public void getTemplateProfile_ServiceThrowsException() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
            .thenThrow(new RuntimeException("Service exception"));
        mockMvc.perform(get(TEMPLATE_DETAILS_SEARCH, VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
               .andExpect(status().isInternalServerError());
    }

    // More test cases...

}
