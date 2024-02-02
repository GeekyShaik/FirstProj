


@RestController
@AllArgsConstructor
//@NoArgsConstructor(force = true) // Force no args constructor since AuthoringMetadataIdObject is not a bean yet.
@Slf4j
@Tag(name = "Object Profile Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class ObjectProfileController {

    private static final Logger LOGGER = LoggerFactory.getLogger(ObjectProfileController.class);
    public static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    public static final String TEMPLATE_NAME_PATH = "/template-details/template-name";
    public static final String TEMPLATE_VERSION_PATH = "/template-version";
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
//    @GetMapping(value = TEMPLATE_DETAILS_SEARCH, produces = MediaType.APPLICATION_JSON_VALUE)
//    @GetMapping(value = TEMPLATE_NAME_PATH + "/{templateName}" + TEMPLATE_VERSION_PATH + "/{templateVersion}", produces = MediaType.APPLICATION_JSON_VALUE)
    @GetMapping(value = TEMPLATE_NAME_PATH + "/{templateName}"+ TEMPLATE_VERSION_PATH, produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<List<TemplateProfileDto>> getTemplateProfile(
            @PathVariable("templateName") @NotNull @Size(min = 1, max = 50) String templateName
            ,@PathVariable("templateVersion") @NonNull @Size(min = 1, max = 50) Integer templateVersion
    ) {

        List<TemplateProfileDto> templateProfileList = new ArrayList<>();
//        LOGGER.info("Searching for template details for templateName: {} and templateVersion: {}",
//                templateName, templateVersion);

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

=============================================================


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
====================================================

package com.usaa.ect;

import com.usaa.ect.controller.ObjectProfileController;
import com.usaa.ect.dtos.TemplateProfileDto;
import com.usaa.ect.service.TemplateProfileService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.security.config.annotation.web.WebSecurityConfigurer;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.result.MockMvcResultHandlers;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.setup.MockMvcBuilders.webAppContextSetup;
import com.usaa.ect.integration.ITAuthConfig;

@ExtendWith(SpringExtension.class)
/*@WebMvcTest(excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})*/
@WebMvcTest(ObjectProfileController.class)


public class TemplateProfileControllerTest {
    @Autowired
    private MockMvc mockMvc;
    @MockBean
    private TemplateProfileService templateProfileService;

    @MockBean
    private ObjectProfileController objectProfileController;

    @Autowired
    private WebApplicationContext webApplicationContext;


    @BeforeEach
    public void setUp() {
        mockMvc = MockMvcBuilders.standaloneSetup(new ObjectProfileController(templateProfileService)).build();
    }
    /*@BeforeEach
    public void setUp() {
        mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    }*/


    //    private static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    private static final String TEMPLATE_NAME_PATH = "/template-details/template-name/";
    private static final String TEMPLATE_VERSION_PATH = "/template-version/";
    private static final String VALID_TEMPLATE_NAME = "ECM_Delivery_NewBrnd";
    private static final Integer VALID_TEMPLATE_VERSION = 1;

    HttpHeaders localTestHeaders = populateHttpHeadersMap();

    private HttpHeaders populateHttpHeadersMap() {
        HttpHeaders headers = new HttpHeaders();
        headers.add("id-claim", ITAuthConfig.EMP_TOKEN);
        headers.add("roles", "[\"ROLE_ERATOS_API_READ_ACCESS\"]");
        headers.add("party-id", ITAuthConfig.EMP_ID);
        headers.add("party-type", "EMPE");
        headers.add("party-guid", ITAuthConfig.EMP_ID);

        return headers;
    }


    @Test
    public void getTemplateProfile_Returns200() throws Exception {


        mockMvc.perform(
                        get("/template-details/template-name/ECM Delivery NewBrand/template-version/1")
                                .headers(localTestHeaders)
                )
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void getTemplateProfile_WithInvalidParameters_Returns400() throws Exception {
        mockMvc.perform(get(TEMPLATE_NAME_PATH + "InvalidTemplateName" + TEMPLATE_VERSION_PATH)
                        .headers(localTestHeaders))
                .andExpect(status().isBadRequest())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void getTemplateProfile_WithNonexistentTemplateName_Returns404() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(Collections.emptyList());

        mockMvc.perform(get(TEMPLATE_NAME_PATH + "NonexistentTemplateName" + TEMPLATE_VERSION_PATH)
                        .headers(localTestHeaders))
                .andExpect(status().isNotFound())
                .andDo(MockMvcResultHandlers.print());
    }

    @Test
    public void getTemplateProfile_ServiceThrowsException_Returns500() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenThrow(new RuntimeException("Internal server error"));

        mockMvc.perform(get(TEMPLATE_NAME_PATH + "ValidTemplateName" + TEMPLATE_VERSION_PATH)
                        .headers(localTestHeaders))
                .andExpect(status().isInternalServerError())
                .andDo(MockMvcResultHandlers.print());
    }

}
