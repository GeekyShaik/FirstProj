
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

        List<TemplateProfileDto> templateProfileList = new ArrayList<>();
        LOGGER.info("Searching for template details for templateName: {} and templateVersion: {}",
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
===============================================================


@Service
@Slf4j
@RequiredArgsConstructor
public class TemplateProfileService {

    private static final Logger LOGGER = LoggerFactory.getLogger(TemplateProfileService.class);
    private final OmMessageTemplateRepository templateRepository;
    private final OmMsgTemplateChannelRepository omMsgTemplateChannelRepository;

    public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, Integer templateVersion) {
        LOGGER.info("RECEIVED PROFILE REQUEST for templateName :{} , templateVersion{}}", templateName, templateVersion);
        List<TemplateProfileDto> templateProfileDtoList = new ArrayList<>();
        List<OmMsgTemplateChannelEntity> channelEntities = new ArrayList<>();
        List<OmMsgTemplateChannelDto> channelDtos = new ArrayList<>();
        try {
            OmMsgTemplateEntity omMsgTemplateEntity = templateRepository.findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(templateName, templateVersion);

            channelEntities = omMsgTemplateChannelRepository.searchAllByTemplateNameAndTemplateVersion(omMsgTemplateEntity.getOmMsgTemplateEntityPK().getTemplateName(), omMsgTemplateEntity.getOmMsgTemplateEntityPK().getTemplateVersion());
            channelDtos = channelEntities.stream()
                    .map(TemplateProfileService::mapTemplateChannnnelEntityToDTO)
                    .collect(Collectors.toList());
            TemplateProfileDto templateProfileDto = mapTemplateEntityToDTO(omMsgTemplateEntity);
            templateProfileDto.setAllowedChannels(channelDtos);
            templateProfileDtoList.add(templateProfileDto);

            LOGGER.info("templateProfileDtoList :: {}", templateProfileDtoList.toString());
        } catch (Exception e) {
            LOGGER.error("EXCEPTION ", e.getMessage());
        }

        return templateProfileDtoList;
    }

    public static TemplateProfileDto mapTemplateEntityToDTO(OmMsgTemplateEntity entity) {
        TemplateProfileDto dto = new TemplateProfileDto();
        try {
            dto = TemplateProfileMapper.INSTANCE.mapEntityToDTO(entity);
        } catch (Exception e) {
            LOGGER.error("Unable to map TemplateProfile entity to DTO", e.getCause().getMessage());
        }
        return dto;
    }

    public static OmMsgTemplateChannelDto mapTemplateChannnnelEntityToDTO(OmMsgTemplateChannelEntity channelEntity) {
        OmMsgTemplateChannelDto channelDto = new OmMsgTemplateChannelDto();
        try {
            channelDto = TemplateChannelMapper.INSTANCE.mapChannelEntityToDTO(channelEntity);
        } catch (Exception e) {
            LOGGER.error("Unable to map TemplateChannel entity to DTO", e.getCause().getMessage());
        }
        return channelDto;
    }
}
=================================================
dto returns
============
TemplateProfileDto(templateName=null,
 msgTemplVersionEffectiveDt=null,
 templateVersion=null,
 enterpriseDocumentId=null,...
 variableSchemaNm=null,....
 opsProcedureAreaAc=null,
 opsProcedureCategoryAc=null, 
opsProcedureSubcategoryAc=null,
 messageTemplateTypeCd=null,
 displayNm=null,
 regulatoryClassCd=null, 
sourceSystemDocumentId=null,
 sourceToolDc=null,
 goGetCollectionNm=null,
 channelDefaultCd=null,
 channelOptionDefaultCd=null, 
pageNumHorzAlignCd=null,
 pageNumTypeCd=null,
 pageNumVertAlignCd=null,
 pageNumDisplayRangeCd=null,
 pageNumShowCountInd=null,
 pageNumCountRangeCd=null,
 contNumAcrsCccoverInd=null,
 contNumAcrsMasterInd=null,
 contNrAcrossEnclInd=null, 
allowedChannels=null)

postman response Body
=====================

[
    {
        "templateName": "ECM_Delivery_NewBrnd",
        "templateVersion": 1,
        "effectiveDate": "2019-08-19",
        "enterpriseDocId": "33333",
        "variableSchemaName": "ECM_Delivery_NewBrnd",
        "busArea": "BK",
        "busCategory": "ACG",
        "busSubCategory": "ACG ",
        "messageTemplateTypeCd": "M",
        "displayName": "testAutomationNameUpdated832",
        "regulatoryClassification": null,
        "sourceSystemDocumentId": "testAutomationVIRTUAL123",
        "sourceTool": "METADATA",
        "goGetCollectionName": "10_28GOGET1",
        "channelDefaultCode": null,
        "channelDefaultOptionCode": null,
        "pageNumberHorizAlignCd": "right",
        "pageNumberTypeCd": "D",
        "pageNumberVertAlignCd": "bottom",
        "pageNumberDisplayRangeCd": "ALL",
        "showPageCountInd": "Y",
        "pageNumberCountRangeCd": "ALL",
        "continousNumberingAcrossCCCover": "Y",
        "continousNumberingAcrossMaster": "Y",
        "continousNumberingAcrossEnclosures": null,
        "allowedChannels": []
    }
]
