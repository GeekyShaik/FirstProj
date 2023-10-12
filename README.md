# FirstProj

this is the first step i have committed.

import okhttp3.mockwebserver.MockResponse;
import okhttp3.mockwebserver.MockWebServer;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.springframework.http.MediaType;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

import java.math.BigDecimal;
import java.net.URL;

import static org.junit.jupiter.api.Assertions.assertNotNull;

public class YourServiceTest {

    private MockWebServer mockWebServer;

    private YourService yourService; // replace 'YourService' with the actual name of your service class

    @BeforeEach
    public void setUp() throws Exception {
        mockWebServer = new MockWebServer();
        mockWebServer.start();

        WebClient webClient = WebClient.builder()
                .baseUrl(mockWebServer.url("/").toString())
                .build();

        yourService = new YourService(webClient); // Assuming the service accepts WebClient in its constructor
    }

    @AfterEach
    public void tearDown() throws Exception {
        mockWebServer.shutdown();
    }

    @Test
    public void testRetrieveTemplateWebClientCall() throws Exception {
        // Given
        String responseBody = "{...}"; // your expected JSON response
        mockWebServer.enqueue(new MockResponse()
                .setBody(responseBody)
                .setHeader("Content-Type", MediaType.APPLICATION_JSON_VALUE)
        );

        // When
        TemplateMigrationObject result = yourService.retrieveTemplateWebClientCall(
                mockWebServer.url("/").toString(), "/path", "objectName", BigDecimal.ONE, "typeCd"
        );

        // Then
        assertNotNull(result);
        // Add more assertions based on what you expect the result to be
    }
}
