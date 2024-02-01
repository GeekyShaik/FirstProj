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
    public void getTemplateProfile_WhenDataExists_ShouldReturnOk() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
            .thenReturn(Collections.singletonList(new TemplateProfileDto()));

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isOk());
    }

    @Test
    public void getTemplateProfile_WhenDataNotFound_ShouldReturnNotFound() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("NonExisting", 1))
            .thenReturn(Collections.emptyList());

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/NonExisting/template-version/1"))
            .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_WhenServiceThrowsException_ShouldReturnInternalServerError() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("ValidName", 1))
            .thenThrow(new RuntimeException("Service exception"));

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isInternalServerError());
    }

    @Test
    public void getTemplateProfile_WhenInvalidTemplateName_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ /template-version/1"))
            .andExpect(status().isBadRequest());
    }

    @Test
    public void getTemplateProfile_WhenInvalidTemplateVersion_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/"))
            .andExpect(status().isBadRequest());
    }

    @Test
    public void getTemplateProfile_WhenTemplateNameTooLong_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/NameThatIsWayTooLongToBeValid/template-version/1"))
            .andExpect(status().isBadRequest());
    }

    @Test
    public void getTemplateProfile_WhenTemplateVersionNegative_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/-1"))
            .andExpect(status().isBadRequest());
    }

    @Test
    public void getTemplateProfile_WhenMethodNameNotAllowed_ShouldReturnMethodNotAllowed() throws Exception {
        // Act & Assert
        mockMvc.perform(post("/template-details/template-name/ValidName/template-version/1"))
            .andExpect(status().isMethodNotAllowed());
    }

    @Test
    public void getTemplateProfile_WhenRequestHasExtraPath_ShouldReturnNotFound() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1/extra"))
            .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_WhenQueryParametersUsedInsteadOfPathVariables_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName?templateVersion=1"))
            .andExpect(status().isBadRequest());
    }

    @Test
    public void getTemplateProfile_WhenVersionIsNonNumeric_ShouldReturnBadRequest() throws Exception {
        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/abc"))
            .andExpect(status().isBadRequest());
    }

    // Other test cases...
}
