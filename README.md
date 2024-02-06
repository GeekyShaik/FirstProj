@WebMvcTest(ObjectProfileController.class)
class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @Test
    void getTemplateProfile_Returns500_WhenDatabaseErrorOccurs() throws Exception {
        // Mock the service layer to throw a database exception
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
                .thenThrow(new TemplateDatabaseException("Database error occurred"));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
               .andExpect(status().isInternalServerError())
               .andExpect(result -> assertTrue(result.getResolvedException() instanceof TemplateDatabaseException))
               .andExpect(content().string(containsString("Database error occurred")));
    }
}
