import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.http.ResponseEntity;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;

import java.util.Collections;
import java.util.List;

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @InjectMocks
    private ObjectProfileController objectProfileController;

    public ObjectProfileControllerTest() {
        mockMvc = MockMvcBuilders.standaloneSetup(objectProfileController).build();
    }

    @Test
    public void getTemplateProfile_Returns200Ok_WhenDataExists() throws Exception {
        List<TemplateProfileDto> sampleData = Collections.singletonList(new TemplateProfileDto());
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(sampleData);

        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", "SampleName", 1))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void getTemplateProfile_Returns404NotFound_WhenDataDoesNotExist() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(Collections.emptyList());

        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", "SampleName", 1))
                .andExpect(status().isNotFound())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void getTemplateProfile_Returns400BadRequest_WhenInvalidParameters() throws Exception {
        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", "", -1))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    // Add more tests here as needed
}
