@WebMvcTest(ObjectProfileController.class)
public class TemplateProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @BeforeEach
    public void setup() {
        // MockMvc is already configured through @Autowired, but any additional setup can be done here
    }

    @Test
    public void getTemplateProfile_Returns200() throws Exception {
        // Given
        String templateName = "ECM_Delivery_NewBrnd";
        Integer templateVersion = 1;
        List<TemplateProfileDto> mockResponse = new ArrayList<>();
        mockResponse.add(new TemplateProfileDto()); // Assume this initializes a valid DTO instance
        when(templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion)).thenReturn(mockResponse);

        // When & Then
        mockMvc.perform(get("/template-details/template-name/" + templateName + "/template-version/" + templateVersion)
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());
    }

    @Test
    public void getTemplateProfile_WithNonexistentTemplateName_Returns404() throws Exception {
        // Given
        String templateName = "NonexistentTemplateName";
        Integer templateVersion = 1;
        when(templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion)).thenReturn(Collections.emptyList());

        // When & Then
        mockMvc.perform(get("/template-details/template-name/" + templateName + "/template-version/" + templateVersion)
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_ServiceThrowsException_Returns500() throws Exception {
        // Given
        String templateName = "ValidTemplateName";
        Integer templateVersion = 1;
        when(templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion)).thenThrow(new RuntimeException("Internal server error"));

        // When & Then
        mockMvc.perform(get("/template-details/template-name/" + templateName + "/template-version/" + templateVersion)
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isInternalServerError());
    }
}
