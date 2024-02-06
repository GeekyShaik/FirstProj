import org.springframework.mock.web.MockHttpServletResponse;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.filter.OncePerRequestFilter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.http.HttpStatus;
import org.springframework.web.server.ResponseStatusException;
// ... other imports

@WebMvcTest(ObjectProfileController.class)
class ObjectProfileControllerTest {

    @Autowired
    private WebApplicationContext context;

    private MockMvc mockMvc;

    @BeforeEach
    public void setUpMockMvc() {
        this.mockMvc = MockMvcBuilders
            .webAppContextSetup(context)
            .addFilters(new OncePerRequestFilter() {
                @Override
                protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
                        throws ServletException, IOException {
                    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED, "Unauthorized");
                }
            })
            .build();
    }

    @Test
    public void getTemplateProfile_Returns401_Unauthorized_WhenNoAuthentication() throws Exception {
        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                .headers(localTestHeaders))
            .andExpect(status().isUnauthorized());
    }
}
