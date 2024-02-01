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
        // Actual values (what we pass to the controller)
        String actualTemplateName = "ECM_Delivery_NewBrnd";
        Integer actualTemplateVersion = 1;

        // Expected values (what we expect the service to return)
        TemplateProfileDto expectedDto = new TemplateProfileDto();
        expectedDto.setTemplateName("ECM_Delivery_NewBrnd");
        expectedDto.setTemplateVersion(1);
        // ... set other properties on expectedDto as expected to be returned from the service

        when(templateProfileService.getTemplatesByNameAndVersion(actualTemplateName, actualTemplateVersion))
                .thenReturn(Collections.singletonList(expectedDto));

        mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", actualTemplateName, actualTemplateVersion))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName").value(expectedDto.getTemplateName()))
                .andExpect(jsonPath("$[0].templateVersion").value(expectedDto.getTemplateVersion()))
                // ... other assertions for the properties
                .andDo(MockMvcResultHandlers.print());
    }

    // Other test cases...
}
