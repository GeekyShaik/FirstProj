import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.stereotype.Service;
import org.springframework.web.reactive.function.client.WebClient;
import org.springframework.web.util.UriComponentsBuilder;
import reactor.core.publisher.Mono;

import java.math.BigDecimal;

@Service
public class OmMsgTemplateService {

    private final OmMsgTemplateRepository omMsgTemplateRepository;
    private final WebClient metadataWebServiceClient;
    private static final String API_KEY = "Your API Key"; // replace with your API key
    private static final String apiKeyLabel = "apiKey"; // replace with the appropriate header name

    @Autowired
    public OmMsgTemplateService(OmMsgTemplateRepository omMsgTemplateRepository, WebClient.Builder webClientBuilder) {
        this.omMsgTemplateRepository = omMsgTemplateRepository;
        this.metadataWebServiceClient = webClientBuilder.baseUrl("http://baseurl.com").build(); // replace with your base URL
    }

    public TemplateProfileObjectDTO retrieveTemplateWebClientCall(String basePath, String pathLabels, String objectName, BigDecimal objectVersion, String mdataObjTypeCd) throws Exception {
        omMsgTemplateEntity templateEntity = omMsgTemplateRepository.findByObjectNameAndObjectVersion(objectName, objectVersion);

        if (templateEntity == null) {
            throw new Exception("Template not found in the database.");
        }

        return metadataWebServiceClient.get()
                .uri(UriComponentsBuilder.fromUriString(basePath + pathLabels)
                        .build(templateEntity.getObjectName(), templateEntity.getObjectVersion()))
                .header(apiKeyLabel, API_KEY)
                .accept(MediaType.APPLICATION_JSON)
                .retrieve()
                .onStatus(HttpStatus::is4xxClientError, clientResponse -> Mono.error(new Exception("Client error during the web service call.")))
                .onStatus(HttpStatus::is5xxServerError, clientResponse -> Mono.error(new Exception("Server error during the web service call.")))
                .bodyToMono(TemplateProfileObjectDTO.class)
                .block();
    }
}
