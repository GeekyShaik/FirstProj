@RestController
@AllArgsConstructor
@Slf4j
@Tag(name = "Object Profile Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class ObjectProfileController {

    private static final Logger LOGGER = LoggerFactory.getLogger(ObjectProfileController.class);
    private final TemplateProfileService templateProfileService;

    @GetMapping(TEMPLATE_DETAILS_SEARCH)
    public ResponseEntity<List<TemplateProfileDto>> getTemplateProfile(
            @RequestHeader(value = "roles", required = false) String rolesHeader,
            @PathVariable("templateName") @Size(min = 1, max = 50) String templateName,
            @PathVariable("templateVersion") @Range(min = 0, max = 9999) Integer templateVersion
    ) {
        // Simulate role check
        if (rolesHeader == null || !rolesHeader.contains("ROLE_GENESIS_EXP")) {
            LOGGER.error("Access denied. User does not have ROLE_GENESIS_EXP");
            throw new ResponseStatusException(HttpStatus.FORBIDDEN, "Access Denied");
        }

        // ... existing code to handle the request
    }
}