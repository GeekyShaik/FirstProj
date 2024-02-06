@Autowired
private WebApplicationContext context;

@BeforeEach
public void setUpMockMvc() {
    // Setup MockMvc with a SecurityFilter that forces a 401 Unauthorized response
    this.mockMvc = MockMvcBuilders
        .webAppContextSetup(context)
        .addFilters((request, response, chain) -> {
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
        })
        .build();
}

@Test
public void getTemplateProfile_Returns401_Unauthorized_WhenNoAuthentication() throws Exception {
    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
            .headers(localTestHeaders))
        .andExpect(status().isUnauthorized());
}
