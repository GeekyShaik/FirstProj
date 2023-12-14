Absolutely! I'll add some additional test cases to the `ObjectProfileControllerTest` and `TemplateProfileServiceTest` classes using JUnit 5. These will cover different aspects and scenarios.

### Additional ObjectProfileController Tests

```java
// ... [previous imports]

import org.springframework.mock.web.MockHttpServletResponse;

@ExtendWith(SpringExtension.class)
@WebMvcTest(ObjectProfileController.class)
public class ObjectProfileControllerTest {
    // ... [previous field declarations and setup]

    @Test
    public void whenValidInput_thenContentTypeIsJson() throws Exception {
        List<TemplateProfileDto> dtoList = Collections.singletonList(new TemplateProfileDto());
        when(templateProfileService.getTemplatesByNameAndVersion("validName", 1)).thenReturn(dtoList);

        mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, "validName", 1))
                .andExpect(content().contentType(MediaType.APPLICATION_JSON));
    }

    @Test
    public void whenServiceReturnsEmptyList_thenResponseIsEmpty() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion("validName", 1)).thenReturn(Collections.emptyList());

        MockHttpServletResponse response = mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, "validName", 1))
                                                 .andReturn().getResponse();

        assertEquals("", response.getContentAsString());
    }

    // Additional test cases...
}
```

### Additional TemplateProfileService Tests

```java
// ... [previous imports]

import java.util.Arrays;

@ExtendWith(MockitoExtension.class)
public class TemplateProfileServiceTest {
    // ... [previous field declarations and setup]

    @Test
    public void whenMultipleTemplatesFound_thenAllAreReturned() {
        List<OmMsgTemplateEntity> entities = Arrays.asList(new OmMsgTemplateEntity(), new OmMsgTemplateEntity());
        when(templateRepository.findTemplateProfile("validName", 1)).thenReturn(entities);

        TemplateProfileModelMapper mapper = new TemplateProfileModelMapper();
        when(templateProfileModelMapperConfig.templateProfileModelMapper()).thenReturn(mapper);

        List<TemplateProfileDto> result = templateProfileService.getTemplatesByNameAndVersion("validName", 1);

        assertEquals(2, result.size());
    }

    @Test
    public void whenInvalidVersion_thenEmptyListReturned() {
        when(templateRepository.findTemplateProfile("validName", -1)).thenReturn(Collections.emptyList());

        List<TemplateProfileDto> result = templateProfileService.getTemplatesByNameAndVersion("validName", -1);

        assertTrue(result.isEmpty());
    }

    // Additional test cases...
}
```

### Explanation
- **ObjectProfileControllerTest**
  - **ContentType Test**: Checks if the response content type is JSON when a valid request is made.
  - **Empty Response Test**: Verifies the controller's response is empty when the service returns an empty list.

- **TemplateProfileServiceTest**
  - **Multiple Templates Test**: Tests the scenario where multiple templates are found, and ensures all are returned.
  - **Invalid Version Test**: Checks the service's behavior when an invalid version number is provided (e.g., a negative number).

These tests enhance coverage by checking various scenarios and edge cases. It's important to tailor these tests based on the specific logic and requirements of your application.