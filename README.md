

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
==========================================================================


@Repository
@Transactional
public interface OmMessageTemplateRepository
        extends JpaRepository<OmMsgTemplateEntity, OmMsgTemplateEntityPK> {
    OmMsgTemplateEntity findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(String templateName, Integer templateVersion);
}

=================================================================
@Repository
@Transactional
public interface OmMsgTemplateChannelRepository  extends JpaRepository<OmMsgTemplateChannelEntity, OmMsgTemplateChannelEntityPK>{
    @Transactional
    @Modifying
    @Query(
            value =
                    "SELECT * FROM OM_AUTHORING.OM_MESSAGE_TEMPLATE_CHANNEL "
                            + "WHERE OM_TEMPLATE_NM = :templateName "
                            + "AND OM_TEMPLATE_VERSION_NR = :versionNumber AND LOGICAL_DELETE_IND = 'N'",
            nativeQuery = true)
    List<OmMsgTemplateChannelEntity>
    searchAllByTemplateNameAndTemplateVersion(
            @Param("templateName") String templateName,
            @Param("versionNumber") int versionNumber);
}
===============================================================================
@Service
@Slf4j
@RequiredArgsConstructor
public class TemplateProfileService {

    private static final Logger LOGGER = LoggerFactory.getLogger(TemplateProfileService.class);
    private final OmMessageTemplateRepository templateRepository;
    private final OmMsgTemplateChannelRepository omMsgTemplateChannelRepository;

    //    @Autowired
//    public TemplateProfileService(
//            OmMessageTemplateRepository templateRepository,
//            TemplateProfileModelMapperConfig templateProfileModelMapperConfig) {
//        this.templateRepository = templateRepository;
//        this.templateProfileModelMapperConfig = templateProfileModelMapperConfig;
//    }
//
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
=========================================================

package com.usaa.ent.controllers;

import com.usaa.ent.apis.controllers.AuthoringPublicController;
import com.usaa.ent.apis.services.AuthoringPublicService;
import com.usaa.ent.metrics.MetricsComponent;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;
import org.springframework.security.config.annotation.web.WebSecurityConfigurer;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@ExtendWith(SpringExtension.class)
@WebMvcTest(excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})
public class AuthoringPublicControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private AuthoringPublicService authoringPublicService;

    @MockBean
    private MetricsComponent metricsComponent;

    private String DID_PARAMETER = "enterpriseDocumentId";
    private String TEST_DID_NUMBER = "99999";
    private String TEMPLATE_NAME_PARAMETER = "templateName";
    private String TEST_TEMPLATE_NAME = "54324_LG_EX_Germany_White_Dou";
    private String TEMPLATE_VERSION_PARAMETER = "templateVersion";
    private String TEST_TEMPLATE_REQUEST_ID = "aUl6s00000002ZSCAY";

    //////////////////////////////DID ATTRIBUTES//////////////////////////////
    @Test
    public void retrievedDIDAttributesSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.DID_ATTRIBUTES_SEARCH)
                                .param(DID_PARAMETER, TEST_DID_NUMBER))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedDIDAttributesSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.DID_ATTRIBUTES_SEARCH)
                                .param(DID_PARAMETER, TEST_DID_NUMBER))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedDIDAttributesSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.DID_ATTRIBUTES_SEARCH, "onetwothree"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE ATTRIBUTES//////////////////////////////
    @Test
    public void retrievedTemplateAttributesSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_ATTRIBUTES_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateAttributesSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_ATTRIBUTES_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateAttributesSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_ATTRIBUTES_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE PROFILE//////////////////////////////
    @Test
    public void retrievedTemplateProfileSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_PROFILE_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateProfileSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_PROFILE_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateProfileSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_PROFILE_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE CHANNEL//////////////////////////////
    @Test
    public void retrievedTemplateChannelSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_CHANNEL_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateChannelSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_CHANNEL_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateChannelSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_CHANNEL_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE LINK//////////////////////////////
    @Test
    public void retrievedTemplateLinkSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_LINK_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateLinkSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_LINK_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateLinkSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_LINK_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE LOCATION//////////////////////////////
    @Test
    public void retrievedTemplateLocationSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_LOCATION_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateLocationSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_LOCATION_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateLocationSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_LOCATION_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE VERSION ACTIVITY//////////////////////////////
    @Test
    public void retrievedTemplateVersionActivitySearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_VERSION_ACTIVITY_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4")
                                .param("activityTypeCd", "CR"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateVersionActivitySearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_VERSION_ACTIVITY_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4")
                                .param("activityTypeCd", "CR"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateVersionActivitySearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_VERSION_ACTIVITY_SEARCH, TEST_TEMPLATE_NAME, "four", 4))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////VARIABLE TEMPLATE RLTN//////////////////////////////
    @Test
    public void retrievedVariableTemplateRltnSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.VARIABLE_TEMPLATE_RLTN_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedVariableTemplateRltnSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.VARIABLE_TEMPLATE_RLTN_SEARCH)
                                .param(TEMPLATE_NAME_PARAMETER, TEST_TEMPLATE_NAME)
                                .param(TEMPLATE_VERSION_PARAMETER, "4"))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedVariableTemplateRltnSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.VARIABLE_TEMPLATE_RLTN_SEARCH, TEST_TEMPLATE_NAME, "four"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    //////////////////////////////TEMPLATE REQUEST ID//////////////////////////////
    @Test
    public void retrievedTemplateRequestIdSearch_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_REQUEST_SEARCH)
                                .param("templateRequestId", TEST_TEMPLATE_REQUEST_ID))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateRequestIdSearch_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.TEMPLATE_REQUEST_SEARCH)
                                .param("templateRequestId", TEST_TEMPLATE_REQUEST_ID))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedTemplateRequestIdSearch_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.TEMPLATE_REQUEST_SEARCH, TEST_TEMPLATE_REQUEST_ID))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    ////////////////////////HERCULES REPORT////////////////////////////
    @Test
    public void retrievedHerculesReport_returns200Ok() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.HERCULES_REPORT)
                                .param(DID_PARAMETER, "54324"))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void retrievedHerculesReport_returns405MethodNotAllowed() throws Exception {
        mockMvc
                .perform(
                        put(AuthoringPublicController.HERCULES_REPORT)
                                .param(DID_PARAMETER, TEST_DID_NUMBER))
                .andExpect(status().isMethodNotAllowed())
                .andDo(MockMvcResultHandlers.print());
    }


    @Test
    public void retrievedHerculesReport_returns400BadRequest() throws Exception {
        mockMvc
                .perform(
                        get(AuthoringPublicController.HERCULES_REPORT, "ninenineninenine"))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }
}
