My apologies for the misunderstanding. Here are the other test cases and the `TemplateProfileDto` entries separately:

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;

import java.time.LocalDate;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import static org.hamcrest.Matchers.*;

@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private TemplateProfileService templateProfileService;

    @Test
    public void getTemplateProfile_Returns200() throws Exception {
        // Arrange
        TemplateProfileDto dto = createTemplateProfileDto();
        List<TemplateProfileDto> mockResponse = Collections.singletonList(dto);

        when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                .thenReturn(mockResponse);

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName", is("ECM_Delivery_NewBrnd")))
                .andExpect(jsonPath("$[0].templateVersion", is(1)));
    }

    @Test
    public void getTemplateProfile_Returns404() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("NonExistentTemplate", 1))
                .thenReturn(Collections.emptyList());

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/NonExistentTemplate/template-version/1")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_Returns500() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("ExceptionTemplate", 1))
                .thenThrow(new RuntimeException("Internal Server Error"));

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ExceptionTemplate/template-version/1")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isInternalServerError());
    }

    // Separate TemplateProfileDto entries for different test cases
    private TemplateProfileDto createTemplateProfileDto() {
        TemplateProfileDto dto = new TemplateProfileDto();
        dto.setTemplateName("ECM_Delivery_NewBrnd");
        dto.setTemplateVersion(1);
        dto.setMsgTemplVersionEffectiveDt(LocalDate.ofEpochDay(2018 - 11 - 01));
        // Set other properties here
        return dto;
    }
}
```

You can create additional test cases as needed and adjust the properties of the `TemplateProfileDto` to match the expected responses.