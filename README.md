// Assuming necessary imports are in place...

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
        // Expected values
        String expectedTemplateName = "ECM_Delivery_NewBrnd";
        Integer expectedTemplateVersion = 1;

        // Actual values returned from the service
        TemplateProfileDto actualDto = new TemplateProfileDto();
        actualDto.setTemplateName(expectedTemplateName);
        actualDto.setTemplateVersion(expectedTemplateVersion);
        // ... set other properties on actualDto as they would be returned from the service

        when(templateProfileService.getTemplatesByNameAndVersion(expectedTemplateName, expectedTemplateVersion))
                .thenReturn(Collections.singletonList(actualDto));

        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", expectedTemplateName, expectedTemplateVersion))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName").value(expectedTemplateName))
                .andExpect(jsonPath("$[0].templateVersion").value(expectedTemplateVersion))
                // ... other assertions for the properties
                .andDo(MockMvcResultHandlers.print());
    }

    // Other test cases...
}
