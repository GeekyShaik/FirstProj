@WebMvcTest(ObjectProfileController.class)
class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    private void setUpUserWithRole(String role) {
        List<GrantedAuthority> authorities = Collections.singletonList(new SimpleGrantedAuthority(role));
        Authentication auth = new UsernamePasswordAuthenticationToken("user", null, authorities);
        SecurityContextHolder.getContext().setAuthentication(auth);
    }

    @AfterEach
    void clearSecurityContext() {
        SecurityContextHolder.clearContext();
    }

    @Test
    void getTemplateProfile_Returns200_WhenUserHasCorrectRole() throws Exception {
        setUpUserWithRole("ROLE_ALLOWED");
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
                .thenReturn(Collections.singletonList(new TemplateProfileDto()));

        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
               .andExpect(status().isOk());
    }

    @Test
    void getTemplateProfile_Returns403_WhenUserDoesNotHaveCorrectRole() throws Exception {
        setUpUserWithRole("ROLE_NOT_ALLOWED");
        mockMvc.perform(get("/template-details/template-name/ValidName/template-version/1"))
               .andExpect(status().isForbidden());
    }
}
