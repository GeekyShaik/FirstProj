@WebMvcTest(ObjectProfileController.class)
public class TemplateProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @BeforeEach
    public void setUp() {
        // Setup can be done here if needed
    }

    @Test
    public void getTemplateProfile_Returns200() throws Exception {
        // Arrange
        TemplateProfileDto dto = new TemplateProfileDto();
        // Set all properties on dto according to your actual DTO
        dto.setTemplateName("ECM_Delivery_NewBrnd");
        dto.setTemplateVersion(1);
        // ... (set other properties as shown in the Postman response)
        List<TemplateProfileDto> mockResponse = Collections.singletonList(dto);

        when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                .thenReturn(mockResponse);

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName", is("ECM_Delivery_NewBrnd")))
                .andExpect(jsonPath("$[0].templateVersion", is(1)));
                // ... (additional assertions can be added to verify the structure of the returned DTO)
    }

    @Test
    public void getTemplateProfile_Returns404WhenNotFound() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("NonExistent", 1))
                .thenReturn(Collections.emptyList());

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/NonExistent/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }

    @Test
    public void getTemplateProfile_Returns500WhenServiceThrowsException() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                .thenThrow(new RuntimeException("Internal server error"));

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isInternalServerError());
    }
}
