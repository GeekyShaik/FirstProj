Certainly! Based on the JSON response you've provided, we can populate the `TemplateProfileDto` object with the relevant data for the test case where data exists. Here is the test case with the populate logic included:

```java
import static org.mockito.ArgumentMatchers.eq;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;
import org.springframework.http.MediaType;
import java.util.Collections;
import java.util.List;

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(new ObjectProfileController(templateProfileService)).build();
    }

    @Test
    public void getTemplateProfile_Returns200Ok_WhenDataExists() throws Exception {
        // Arrange
        String templateName = "ECM_Delivery_NewBrnd";
        Integer templateVersion = 1;
        TemplateProfileDto dto = new TemplateProfileDto();
        dto.setTemplateName(templateName);
        dto.setTemplateVersion(templateVersion);
        dto.setEffectiveDate("2019-08-19");
        dto.setEnterpriseDocumentId("33333");
        dto.setVariableSchemaName("ECM_Delivery_NewBrnd");
        dto.setBusArea("BK");
        dto.setBusCategory("ACG");
        dto.setBusSubCategory("ACG ");
        dto.setMessageTemplateTypeCd("M");
        dto.setDisplayName("testAutomationNameUpdated832");
        dto.setSourceSystemDocumentId("testAutomationVIRTUAL123");
        dto.setSourceTool("METADATA");
        dto.setGoGetCollectionName("10_28GOGET1");
        dto.setPageNumberHorizAlignCd("right");
        dto.setPageNumberTypeCd("D");
        dto.setPageNumberVertAlignCd("bottom");
        dto.setPageNumberDisplayRangeCd("ALL");
        dto.setShowPageCountInd("Y");
        dto.setContinousNumberingAcrossCCCover("Y");
        dto.setContinousNumberingAcrossMaster("Y");
        // ... Populate other fields as necessary

        List<TemplateProfileDto> sampleData = Collections.singletonList(dto);
        
        when(templateProfileService.getTemplatesByNameAndVersion(eq(templateName), eq(templateVersion))).thenReturn(sampleData);

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", templateName, templateVersion)
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName").value(templateName))
                .andExpect(jsonPath("$[0].templateVersion").value(templateVersion))
                .andExpect(jsonPath("$[0].effectiveDate").value("2019-08-19"))
                .andExpect(jsonPath("$[0].enterpriseDocumentId").value("33333"))
                // ... Assert other fields as necessary
                .andDo(MockMvcResultHandlers.print());
    }

    // Additional test cases...
}
```

In this `getTemplateProfile_Returns200Ok_WhenDataExists` test method:

- A `TemplateProfileDto` object is created and populated with the data from the JSON response you provided.
- The `when` statement is configured to return this populated `TemplateProfileDto` list when the `getTemplatesByNameAndVersion` method is called with the specific `templateName` and `templateVersion`.
- The `andExpect` statements are then used to assert that the JSON response contains the expected values for the fields. Make sure to add assertions for all the fields you want to verify in the response.
- The `eq` method from `org.mockito.ArgumentMatchers` is used to match the specific values of the parameters.
