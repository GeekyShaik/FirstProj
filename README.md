Certainly! I'll adapt the test classes for `ObjectProfileController` and `TemplateProfileService` to use JUnit 5, and I'll add more test cases to each.

### ObjectProfileController Tests (JUnit 5)

```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @Test
    public void whenValidInput_thenReturns200() throws Exception {
        List<TemplateProfileDto> dtoList = Collections.singletonList(new TemplateProfileDto());
        when(templateProfileService.getTemplatesByNameAndVersion("validName", 1)).thenReturn(dtoList);

        mockMvc.perform(get("/template-details/template-name/validName/template-version/1"))
                .andExpect(status().isOk())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$", hasSize(1)));
    }

    @Test
    public void whenInvalidInput_thenReturns400() throws Exception {
        mockMvc.perform(get("/template-details/template-name/ /template-version/-1"))
                .andExpect(status().isBadRequest());
    }

    @Test
    public void whenDataNotFound_thenReturns404() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion("unknown", 1)).thenReturn(Collections.emptyList());

        mockMvc.perform(get("/template-details/template-name/unknown/template-version/1"))
                .andExpect(status().isNotFound());
    }

    @Test
    public void whenServiceThrowsException_thenReturns500() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenThrow(new RuntimeException());

        mockMvc.perform(get("/template-details/template-name/test/template-version/1"))
                .andExpect(status().isInternalServerError());
    }

    @Test
    public void whenInvalidRoleAccess_thenReturns403() throws Exception {
        // Here, you would simulate a request with an unauthorized role, expecting a 403 Forbidden response.
        // This requires additional Spring Security setup in test configuration.
    }
}
```

### TemplateProfileService Tests (JUnit 5)

```java
@ExtendWith(MockitoExtension.class)
public class TemplateProfileServiceTest {

    @Mock
    private OmMessageTemplateRepository templateRepository;

    @Mock
    private TemplateProfileModelMapperConfig templateProfileModelMapperConfig;

    @InjectMocks
    private TemplateProfileService templateProfileService;

    @Test
    public void whenValidNameAndVersion_thenProfileShouldBeFound() {
        List<OmMsgTemplateEntity> entities = Collections.singletonList(new OmMsgTemplateEntity());
        when(templateRepository.findTemplateProfile("validName", 1)).thenReturn(entities);

        TemplateProfileModelMapper mapper = new TemplateProfileModelMapper();
        when(templateProfileModelMapperConfig.templateProfileModelMapper()).thenReturn(mapper);

        List<TemplateProfileDto> result = templateProfileService.getTemplatesByNameAndVersion("validName", 1);

        assertNotNull(result);
        assertFalse(result.isEmpty());
    }

    @Test
    public void whenNoDataFound_thenEmptyListShouldBeReturned() {
        when(templateRepository.findTemplateProfile("unknown", 1)).thenReturn(Collections.emptyList());

        List<TemplateProfileDto> result = templateProfileService.getTemplatesByNameAndVersion("unknown", 1);

        assertNotNull(result);
        assertTrue(result.isEmpty());
    }

    @Test
    public void whenRepositoryThrowsException_thenServiceHandlesIt() {
        when(templateRepository.findTemplateProfile(anyString(), anyInt())).thenThrow(new RuntimeException());

        assertThrows(RuntimeException.class, () -> 
            templateProfileService.getTemplatesByNameAndVersion("test", 1));
    }

    @Test
    public void whenInvalidInput_thenHandleGracefully() {
        // Add a test case for handling invalid inputs, if applicable.
        // For instance, passing null values and asserting the service's behavior.
    }
}
```

### Notes for JUnit 5
- `@ExtendWith` is used instead of `@RunWith` for extending test classes with additional functionality.
- More test cases are added to cover exceptional scenarios and edge cases.
- Make sure to adapt the setup to your specific needs, especially concerning security and exception handling.