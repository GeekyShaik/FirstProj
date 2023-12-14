Please write unit tests for the TemplateProfile process we have in place: controller and service classes. We need 95% coverage in the jacoco report.
======================================================

@RestController
@AllArgsConstructor
//@NoArgsConstructor(force = true) // Force no args constructor since AuthoringMetadataIdObject is not a bean yet.
@Slf4j
@Tag(name = "Object Profile Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class ObjectProfileController {

    private static final Logger LOGGER = LoggerFactory.getLogger(ObjectProfileController.class);
    public static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";

    private final TemplateProfileService templateProfileService;

    @RolesAllowed("ROLE_GENESIS_EXP")
    @ApiOperation(value = "Retrieve Template Object details from OM_AUTHORING",
            authorizations = {
                    @Authorization(value = SwaggerSpecConfig.OAUTH, scopes = {
                            @AuthorizationScope(scope = SwaggerSpecConfig.SCOPE, description = SwaggerSpecConfig.SCOPE_DESC)})})
    @ApiResponses(
            value = {
                    @ApiResponse(responseCode = "200", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "400", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "401", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "403", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "404", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "500", content = {@Content(schema = @Schema())})
            })
    @GetMapping(TEMPLATE_DETAILS_SEARCH)
    public ResponseEntity<List<TemplateProfileDto>> getTemplateProfile(
            @PathVariable("templateName") @NotNull @Size(min = 1, max = 50) String templateName,
            @PathVariable("templateVersion") @NonNull @Range(min = 0, max = 9999) Integer templateVersion
    ) {
//
        List<TemplateProfileDto> templateProfileList = new ArrayList<>();
        LOGGER.info("Searching for template details for templateName: {} and templateName: {}",
                templateName, templateVersion);

        try {
            templateProfileList = templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion);

        } catch (Exception e) {
            LOGGER.error("Exception occurred: {}", e.getMessage());
        }

        if (templateProfileList.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else {
            return ResponseEntity.ok(templateProfileList);
        }
    }
}
=========================================================================================



@Service
@Slf4j
@RequiredArgsConstructor
public class TemplateProfileService {

    private static final Logger LOGGER = LoggerFactory.getLogger(TemplateProfileService.class);
    private final OmMessageTemplateRepository templateRepository;
    private final TemplateProfileModelMapperConfig templateProfileModelMapperConfig;


    public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, Integer templateVersion) {
        List<OmMsgTemplateEntity> entities = templateRepository.findTemplateProfile(templateName, templateVersion);

        LOGGER.info("RECEIVED PROFILE REQUEST for templateName :------- {}-------------", templateName);
        // Using the TemplateProfileModelMapperConfig to get the TemplateProfileModelMapper bean
        TemplateProfileModelMapper templateProfileModelMapper = templateProfileModelMapperConfig.templateProfileModelMapper();

        // Using the injected TemplateProfileModelMapper bean
        List<TemplateProfileDto> templateProfileDtoList = entities.stream()
                .map(entity -> templateProfileModelMapper.map(entity, TemplateProfileDto.class))
                .collect(Collectors.toList());

        return templateProfileDtoList;
    }
}
================================================================================================

@Repository
@Transactional
public interface OmMessageTemplateRepository
    extends JpaRepository<OmMsgTemplateEntity, OmMsgTemplateEntityPK> {

        @Query(value = "SELECT * FROM OM_AUTHORING.OM_MESSAGE_TEMPLATE  WHERE OM_TEMPLATE_NM = :templateName AND OM_TEMPLATE_VERSION_NR = :templateVersion",
                nativeQuery = true )
        List<OmMsgTemplateEntity> findTemplateProfile(String templateName,
                                              Integer templateVersion
        );
                //returns a list of  TemplateProfileDto


      }





