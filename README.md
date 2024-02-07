import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.context.annotation.Bean;
import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(ObjectProfileController.class)
class TemplateProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @TestConfiguration
    static class CustomExceptionHandler {

        @Bean
        public CustomControllerAdvice controllerAdvice() {
            return new CustomControllerAdvice();
        }
        
        @RestControllerAdvice
        static class CustomControllerAdvice {

            @ExceptionHandler(RuntimeException.class)
            public ResponseEntity<String> handleRuntimeException(RuntimeException ex) {
                return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Internal Server Error occurred");
            }
        }
    }

    // Example test that simulates an internal server error
    @Test
    void getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
                .thenThrow(new RuntimeException("Unexpected error"));

        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1"))
                .andExpect(status().isInternalServerError());
    }
}
