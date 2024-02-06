I understand your requirement. Here's the modified test case where we directly call the controller method without invoking the service:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;

import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(ObjectProfileController.class)
@ExtendWith(SpringExtension.class)
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
        // Mock the behavior of the service (it won't be invoked)
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(new ArrayList<>());

        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk());
    }

    // Add other test methods as needed
}
```

In this test case, the `when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))` line mocks the service behavior but doesn't actually invoke the service method. The test directly calls the controller method and verifies its behavior without involving the service.
