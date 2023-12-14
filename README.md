Certainly! I'll provide example unit test cases for both the `ObjectProfileController` and the `TemplateProfileService`. These examples are based on JUnit and Mockito, which are commonly used in Java testing.

### ObjectProfileController Tests

First, let's write tests for the `ObjectProfileController`. We'll focus on testing the `getTemplateProfile` method.

```java
@RunWith(SpringRunner.class)
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

    // Additional tests for other scenarios...
}
```

### TemplateProfileService Tests

Now, let's write tests for the `TemplateProfileService`. We'll focus on testing the `getTemplatesByNameAndVersion` method.

```java
@RunWith(MockitoJUnitRunner.class)
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

    // Additional tests for other scenarios...
}
```

### Notes
- In the controller tests, `MockMvc` is used to simulate HTTP requests and verify the response.
- In the service tests, dependencies are mocked and behavior is asserted based on the mocked responses.
- These tests cover basic scenarios. You should add more tests to cover edge cases and error handling for thorough coverage.
- Always assert both the result and the expected interactions with the mocked dependencies.