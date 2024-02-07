@WebMvcTest(ObjectProfileController.class)
class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @Test
    void getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
                .thenThrow(new RuntimeException("Unexpected error"));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
                .andExpect(status().isInternalServerError());
    }
}
