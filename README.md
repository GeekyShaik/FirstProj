
@ExtendWith(SpringExtension.class)

@WebMvcTest(controllers= ObjectProfileController.class, excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
       excludeAutoConfiguration = {SecurityAutoConfiguration.class})
        public class TemplateProfileControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @Autowired
    private WebApplicationContext context;

    @MockBean
    private TemplateProfileDto templateProfileDto;

//    private static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
        private static final String TEMPLATE_NAME_PATH = "/template-details/template-name/";
        private static final String TEMPLATE_VERSION_PATH = "/template-version/";
        private static final String VALID_TEMPLATE_NAME = "ECM Delivery NewBrand";
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
//


@BeforeEach


    public void setUp() {


            templateProfileDto = new TemplateProfileDto();
            // Set all properties on dto according to your actual DTO
            templateProfileDto.setTemplateName("ECM_Delivery_NewBrnd");
            templateProfileDto.setTemplateVersion(1);
            templateProfileDto.setMsgTemplVersionEffectiveDt(LocalDate.ofEpochDay(2018 - 11 - 01));
            templateProfileDto.setEnterpriseDocumentId("55498");
            templateProfileDto.setVariableSchemaNm("PnC_Auto_Corr");
            templateProfileDto.setOpsProcedureAreaAc("PC");
            templateProfileDto.setOpsProcedureCategoryAc("PS");
            templateProfileDto.setOpsProcedureSubcategoryAc("AO");
            templateProfileDto.setMessageTemplateTypeCd("m");
            templateProfileDto.setDisplayNm("Rental Reimbursement And/Or Towing And Labor Coverage - AO96");
            templateProfileDto.setRegulatoryClassCd("N");
            templateProfileDto.setSourceSystemDocumentId("55498_MT_No_Rent_Reimb_Pol");
            templateProfileDto.setSourceToolDc("XPRESSION");
            templateProfileDto.setGoGetCollectionNm("PNC_STD_USER_CCIF_MISC");
            templateProfileDto.setChannelDefaultCd("POSTAL");
            templateProfileDto.setChannelOptionDefaultCd("");
            templateProfileDto.setPageNumHorzAlignCd("D");
            templateProfileDto.setPageNumTypeCd("");
            templateProfileDto.setPageNumVertAlignCd("");
            templateProfileDto.setPageNumDisplayRangeCd("");
            templateProfileDto.setPageNumShowCountInd("");
            templateProfileDto.setPageNumCountRangeCd("");
            templateProfileDto.setContNumAcrsCccoverInd("");
            templateProfileDto.setContNumAcrsMasterInd("");
            templateProfileDto.setContNrAcrossEnclInd("");

            List<OmMsgTemplateChannelDto> allowedChannels = new ArrayList<>();
            allowedChannels.add(new OmMsgTemplateChannelDto("EMAIL", "NA", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("FAX", "NA", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "CERT", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "EXPRESS", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "IMMED", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "USPOST", null));
            allowedChannels.add(new OmMsgTemplateChannelDto("UDO", "NA", "PC GEN CORRESPONDENCE"));
            templateProfileDto.setAllowedChannels(allowedChannels);

        }


@Test
public void getTemplateProfile_Returns200() throws Exception {
    List<TemplateProfileDto> mockResponse = Collections.singletonList(templateProfileDto);

    when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
            .thenReturn(mockResponse);

            mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .headers(localTestHeaders)
                    )
        .andExpect(status().isOk())
        .andDo(MockMvcResultHandlers.print());
        }
    @Test
    public void getTemplateProfile_Returns404_Not_Found() throws Exception {
        List<TemplateProfileDto> mockResponse = Collections.singletonList(templateProfileDto);

        when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                .thenReturn(mockResponse);

        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version")
                        .headers(localTestHeaders)
                )
                .andExpect(status().isNotFound())
                .andDo(MockMvcResultHandlers.print());
    }

    
       @Test
       public void getTemplateProfile_Returns400BadRequest_WhenInvalidParameters() throws Exception {
           List<TemplateProfileDto> mockResponse = Collections.singletonList(templateProfileDto);

           when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd",1 ))
                   .thenReturn(mockResponse);

           mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/:templateVersion")
                           .headers(localTestHeaders)
                   )
                   .andExpect(status().isBadRequest())
                   .andDo(MockMvcResultHandlers.print());
       }


}
====================================================================================================


@RestController
@AllArgsConstructor
//@NoArgsConstructor(force = true) // Force no args constructor since AuthoringMetadataIdObject is not a bean yet.
@Slf4j
@Tag(name = "Object Profile Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class ObjectProfileController {

    private static final Logger LOGGER = LoggerFactory.getLogger(ObjectProfileController.class);
   // public static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
   public static final String TEMPLATE_DETAILS_SEARCH = "/template-details/template-name/{templateName}/template-version/{templateVersion}";
    public static final String TEMPLATE_NAME_PATH = "/template-details/template-name/";
    public static final String TEMPLATE_VERSION_PATH = "/template-version";

    private final TemplateProfileService templateProfileService;

    @RolesAllowed("ROLE_GENESIS_EXP")
    @ApiOperation(value = "Retrieve Template Object details from OM_AUTHORING",
            authorizations = {
                    @Authorization(value = SwaggerSpecConfig.OAUTH, scopes = {
                            @AuthorizationScope(scope = SwaggerSpecConfig.SCOPE, description = SwaggerSpecConfig.SCOPE_DESC)})})
    @ApiResponses(
            /*value = {
                    @ApiResponse(responseCode = "200", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "400", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "401", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "403", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "404", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "500", content = {@Content(schema = @Schema())})*/
            value = {
                    @ApiResponse(responseCode = "200", description = "Successful retrieval", content = {@Content(schema = @Schema(implementation = TemplateProfileDto.class))}),
                    @ApiResponse(responseCode = "400", description = "Bad Request", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "401", description = "Unauthorized", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "403", description = "Forbidden", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "404", description = "Not Found", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "500", description = "Internal Server Error", content = {@Content(schema = @Schema())})
            })
    @GetMapping(TEMPLATE_DETAILS_SEARCH)
    public ResponseEntity<List<TemplateProfileDto>> getTemplateProfile(
            @PathVariable("templateName") @NotNull @Size(min = 1, max = 50) String templateName,
            @PathVariable("templateVersion") @NonNull @Range(min = 0, max = 9999) Integer templateVersion
    ) {
    if(templateVersion == null){

        throw new ResponseStatusException(HttpStatus.BAD_REQUEST, "Template Version is required");
    }
        List<TemplateProfileDto> templateProfileList = templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion);

        LOGGER.info("Searching for template details for templateName: {} and templateVersion: {}",
                templateName, templateVersion);

        if (templateProfileList.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else
            return ResponseEntity.ok(templateProfileList);
        }
    }
