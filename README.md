
Based on the previous discussion and suggestions, here's a revised version of the `ObjectProfileController` class with improved exception handling, logging, and validation. This version assumes the existence of custom exception classes (`DataAccessRuntimeException` and `GenericRuntimeException`) and a global exception handler (`GlobalExceptionHandler`). If these do not exist in your codebase, you would need to create them accordingly.

### Revised ObjectProfileController

```java
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import javax.annotation.security.RolesAllowed;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
import java.util.List;

@RestController
@AllArgsConstructor
@Slf4j
@Validated // Enable method parameter validation
@Tag(name = "Object Profile Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class ObjectProfileController {

    private static final Logger LOGGER = LoggerFactory.getLogger(ObjectProfileController.class);
    private final TemplateProfileService templateProfileService;

    @RolesAllowed("ROLE_GENESIS_EXP")
    @ApiOperation(value = "Retrieve Template Object details from OM_AUTHORING",
            authorizations = {
                    @Authorization(value = SwaggerSpecConfig.OAUTH, scopes = {
                            @AuthorizationScope(scope = SwaggerSpecConfig.SCOPE, description = SwaggerSpecConfig.SCOPE_DESC)})})
    @ApiResponses(
            value = {
                    @ApiResponse(responseCode = "200", description = "Successful retrieval", content = {@Content(schema = @Schema(implementation = TemplateProfileDto.class))}),
                    @ApiResponse(responseCode = "400", description = "Bad Request", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "401", description = "Unauthorized", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "403", description = "Forbidden", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "404", description = "Not Found", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "500", description = "Internal Server Error", content = {@Content(schema = @Schema())})
            })
    @GetMapping(value = "/template-details/template-name/{templateName}/template-version/{templateVersion}", produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<List<TemplateProfileDto>> getTemplateProfile(
            @PathVariable("templateName") @NotNull @Size(min = 1, max = 50) String templateName,
            @PathVariable("templateVersion") @NotNull @Size(min = 1, max = 50) Integer templateVersion) {

        LOGGER.info("Searching for template details for templateName: {} and templateVersion: {}", templateName, templateVersion);

        List<TemplateProfileDto> templateProfileList = templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion);

        if (templateProfileList.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else {
            return ResponseEntity.ok(templateProfileList);
        }
    }
}
```

### GlobalExceptionHandler

Here's a simple implementation of the `GlobalExceptionHandler` class to handle exceptions globally across your Spring Boot application. This handler will catch exceptions thrown from any controller and return an appropriate response.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(DataAccessRuntimeException.class)
    public ResponseEntity<String> handleDataAccessRuntimeException(DataAccessRuntimeException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

    @ExceptionHandler(GenericRuntimeException.class)
    public ResponseEntity<String> handleGenericRuntimeException(GenericRuntimeException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

    // Add more exception handlers as needed...

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        return new ResponseEntity<>("An unexpected error occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

### Notes

- Ensure that `@Validated` is on the class level to enable validation.
- Custom exception classes like `DataAccessRuntimeException` and `GenericRuntimeException` should extend `RuntimeException` and provide constructors that accept a message and/or a cause.
- The `@ApiOperation` and `@ApiResponses` annotations suggest you are using Swagger (OpenAPI) for API documentation. Make sure the dependencies and configuration for Swagger are correctly set up in your project.
- The `LOGGER` is used to log the incoming request details. Adjust the logging level and messages as needed for your application's logging strategy.
- This controller now relies on the `GlobalExceptionHandler` to handle exceptions thrown by the service layer, providing a cleaner separation of concerns