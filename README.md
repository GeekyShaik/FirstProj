// Example test cases
public class ObjectProfileControllerTest {

    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @BeforeEach
    public void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(new ObjectProfileController(templateProfileService)).build();
    }

    @Test
    public void getTemplateProfile_ValidData_ShouldReturnOk() throws Exception {
        // Arrange valid data expectations
        TemplateProfileDto dto = new TemplateProfileDto();
        dto.setTemplateName("ValidName");
        dto.setTemplateVersion(1);
        List<TemplateProfileDto> dtos = Collections.singletonList(dto);

        // Service returns valid data
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1)).thenReturn(dtos);

        // Act & Assert - Valid request should return OK
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isOk());
    }

    @Test
    public void getTemplateProfile_ValidData_ShouldReturnExpectedContent() throws Exception {
        // Arrange valid data expectations
        TemplateProfileDto dto = new TemplateProfileDto();
        dto.setTemplateName("ValidName");
        dto.setTemplateVersion(1);
        List<TemplateProfileDto> dtos = Collections.singletonList(dto);

        // Service returns valid data
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1)).thenReturn(dtos);

        // Act & Assert - Valid request should return expected content
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].templateName").value("ValidName"))
            .andExpect(jsonPath("$[0].templateVersion").value(1));
    }

    // Repeat similar structure for other test cases, ensuring service returns expected data for valid scenarios
    // ...

    @Test
    public void getTemplateProfile_EmptyData_ShouldReturnNotFound() throws Exception {
        // Arrange - Service returns empty list for valid input, indicating not found
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 2)).thenReturn(Collections.emptyList());

        // Act & Assert - Valid request but no data should return Not Found
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/2"))
            .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_ServiceThrowsException_ShouldReturnInternalServerError() throws Exception {
        // Arrange - Service throws exception
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 3)).thenThrow(new RuntimeException());

        // Act & Assert - Service exception should result in Internal Server Error
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/3"))
            .andExpect(status().isInternalServerError());
    }

    // Other test cases that match the conditions your service/controller can handle correctly
    // ...

}
