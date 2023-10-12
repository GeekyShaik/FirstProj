import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.ResponseEntity;

import java.math.BigDecimal;

import static org.junit.jupiter.api.Assertions.assertNotNull;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class YourServiceTest {

    @LocalServerPort
    private int port;

    @Autowired
    private YourService yourService; // replace 'YourService' with the actual name of your service class

    @Autowired
    private TestRestTemplate testRestTemplate;

    @Test
    public void testRetrieveTemplateWebClientCall() {
        // Setup
        String basePath = "http://localhost:" + port;
        String pathLabels = "/path";
        String objectName = "objectName";
        BigDecimal objectVersion = BigDecimal.ONE;
        String mdata0bjTypeCd = "typeCd";

        // Actual method call
        TemplateMigrationObject result = yourService.retrieveTemplateWebClientCall(basePath, pathLabels, objectName, objectVersion, mdata0bjTypeCd);

        // Assertions
        assertNotNull(result);
        // Add more assertions based on what you expect the result to be
    }
}
