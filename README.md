Below are the implementations for a `GlobalExceptionHandler`, custom exception classes, and a revised `TemplateProfileService` as discussed. These implementations work together to provide detailed error handling and response messaging for the template profile retrieval feature.

### Custom Exception Classes

First, we define the custom exception classes for database and general retrieval errors.

#### TemplateDatabaseException.java

```java
public class TemplateDatabaseException extends RuntimeException {

    private final String templateName;
    private final Integer templateVersion;

    public TemplateDatabaseException(String templateName, Integer templateVersion, String message, Throwable cause) {
        super(message, cause);
        this.templateName = templateName;
        this.templateVersion = templateVersion;
    }

    // Getters and custom getMessage() as previously defined
}
```

#### TemplateRetrievalException.java

```java
public class TemplateRetrievalException extends RuntimeException {

    private final String templateName;
    private final Integer templateVersion;

    public TemplateRetrievalException(String templateName, Integer templateVersion, String message) {
        super(message);
        this.templateName = templateName;
        this.templateVersion = templateVersion;
    }

    // Getters and custom getMessage() as previously defined
}
```

### GlobalExceptionHandler

This class handles exceptions globally across the application, providing specific responses for the custom exceptions.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(TemplateDatabaseException.class)
    public ResponseEntity<Object> handleTemplateDatabaseException(TemplateDatabaseException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

    @ExceptionHandler(TemplateRetrievalException.class)
    public ResponseEntity<Object> handleTemplateRetrievalException(TemplateRetrievalException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<Object> handleException(Exception ex) {
        return new ResponseEntity<>("An unexpected error occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

### Revised TemplateProfileService

Incorporating the custom exceptions into the service layer:

```java
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.dao.DataAccessException;
import org.springframework.stereotype.Service;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

@Service
@Slf4j
@RequiredArgsConstructor
public class TemplateProfileService {

    private final OmMessageTemplateRepository templateRepository;
    private final OmMsgTemplateChannelRepository omMsgTemplateChannelRepository;

    public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, Integer templateVersion) {
        try {
            OmMsgTemplateEntity omMsgTemplateEntity = templateRepository.findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(templateName, templateVersion);
            if (omMsgTemplateEntity == null) {
                LOGGER.info("No template found for templateName: {}, templateVersion: {}", templateName, templateVersion);
                return new ArrayList<>();
            }

            List<OmMsgTemplateChannelEntity> channelEntities = omMsgTemplateChannelRepository.searchAllByTemplateNameAndTemplateVersion(omMsgTemplateEntity.getOmMsgTemplateEntityPK().getTemplateName(), omMsgTemplateEntity.getOmMsgTemplateEntityPK().getTemplateVersion());
            List<OmMsgTemplateChannelDto> channelDtos = channelEntities.stream()
                    .map(this::mapTemplateChannelEntityToDTO)
                    .collect(Collectors.toList());

            TemplateProfileDto templateProfileDto = mapTemplateEntityToDTO(omMsgTemplateEntity);
            templateProfileDto.setAllowedChannels(channelDtos);

            return List.of(templateProfileDto);
        } catch (DataAccessException e) {
            throw new TemplateDatabaseException(templateName, templateVersion, "Failed to access database for template data", e);
        } catch (Exception e) {
            throw new TemplateRetrievalException(templateName, templateVersion, "General error occurred while retrieving template data", e);
        }
    }

    private TemplateProfileDto mapTemplateEntityToDTO(OmMsgTemplateEntity entity) {
        // Implementation needed
        return new TemplateProfileDto();
    }

    private OmMsgTemplateChannelDto mapTemplateChannelEntityToDTO(OmMsgTemplateChannelEntity channelEntity) {
        // Implementation needed
        return new OmMsgTemplateChannelDto();
    }
}
```

### Note
These implementations are structured to work together to provide clear and helpful error handling for the template profile retrieval process. Custom exception classes help encapsulate specific error information, the `GlobalExceptionHandler` ensures these exceptions result in appropriate HTTP responses, and the service layer leverages these exceptions to signal errors effectively. Make sure to replace placeholders and add missing implementations as needed to fit into your application's overall architecture and coding standards.
