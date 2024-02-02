

@ExtendWith(SpringExtension.class)
@WebMvcTest(excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})


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
        mockMvc = MockMvcBuilders.standaloneSetup(objectProfileController).build();
    }


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


    @Test
    public void getTemplateProfile_Returns200() throws Exception {


        mockMvc.perform(
                        get("/template-details/template-name/ECM Delivery NewBrand/template-version")
                                .headers(localTestHeaders)
                )
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print());
    }

}
=========================================================================

@Configuration
public class ITAuthConfig {

    // Party ID - 30206241, Actor ID - UN99182
    public static final String MSR_TOKEN =
            "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiIsImtpZCI6ImlkLWNsYWltLWp3dC1rZXkifQ.ew0KI"
                    + "CAgICJ0b2tlbl9uYW1lIjogImlkX2NsYWltIiwNCiAgICAic3ViIjogIjg0MzBhNzQwLWU0MzUtNGZjNS1hYmU2LTk3MGY0MWFiOGQ4ZSIsDQog"
                    + "ICAgIm5iZiI6IDE1NTY3NDcwODcsDQogICAgImlzcyI6ICJ3c2RldmludGVybmFsLnVzYWEuY29tIiwNCiAgICAiZXhwaXJlc19pbiI6IDYwMCw"
                    + "NCiAgICAiaWF0IjogMTU1Njc0NzA4NywNCiAgICAiZXhwIjogMTU1Njc0NzY4NywNCiAgICAicGFydHlfaWQiOiAiMzAyMDYyNDEiLA0KICAgIC"
                    + "JwYXJ0eV90eXBlIjogIlVTQUEiLA0KICAgICJhdWQiOiAicnN2Y2ludC51c2FhLmNvbSIsDQogICAgImp0aSI6ICIwMmVkNjVmZi1jZjQzLTQxN"
                    + "jAtYWQ4Yi05ZTdjOWYxNjliMGEiLA0KICAgICJ0b2tlbl90eXBlIjogImp3dCIsDQogICAgImFjdCI6ew0KCQkicGFydHlfaWQiOiAiVU45OTE4"
                    + "MiIsDQoJCSJwYXJ0eV90eXBlIjogIkVNUEUiLA0KCQkic3ViIjogIlVOOTkxODIiDQogICAgfQ0KfQ.VfqNlTxNlLpoXDHi6ZZGxtorTvMR8h2A"
                    + "Uqcm_wpY9q3-oRrG6HVUdWV0_h4nL7Dh_PmxgjvUCUlMpLID73GltEkz93Ngu_H2vRsm-b4bw5qASpZIqQf-pmJtB25aNJ9TQyaPqILQnnFS7aV"
                    + "zV9QvPfZYAWN0J-nfShu1P8IPcfomAG9AyfDlhT6MLAvUNnMYUGmDYUbMPcLaBp3zBJGzFhxVjD08pCGGuBoKE7NEiiXuyvx5ahgEvk-oH-DKp1"
                    + "1g4R8E6gwkI8VILRDY2FNfnYt2ArYABiS2-k6KVfYrsHD3RPPoj_MLaQlwvc2h-TZAxA_k9Gl-1NtYaHfBFcydk1ZN2hbKZZ0rDdFt0CoJO46dP"
                    + "UcNqYoriQEJHAsgOjEjaK321Kz-GAWhKO-JN3dTU_vutDoQMgz3t6pbaxgxWiV-hbuqlB-McTvwE3zbutRYigCJGx5f6SgwT6VpQpY8JwJFkEuM"
                    + "Odrik7ELxvSG3XeQkqA-167QiyVhxpeObmvF6havDU0KyXJkskGQNK2rj9Uh6NjvsjwlLKPmwy2St2ecL4i5XtOfqn-WfslNg7XACArNGGVPY6G"
                    + "lMbiyfTr_2UAU8jZY0fzJLM_vemmoT1kEGbqi9yk8U3vkP2Xf8Z_1PtiTAXyiaEvm_pnnym_o33aFf449D6MLl1YcdFKlCJY";

    public static final String MBR_ID = "029300217";
    // Sample token for Member 029300217
    public static final String MBR_TOKEN =
            "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiIsImtpZCI6ImlkLWNsYWltLWp3dC1rZXkifQ."
                    + "ew0KICAgICJ0b2tlbl9uYW1lIjogImlkX2NsYWltIiwNCiAgICAic3ViIjogImU2MGEwOTYyLTU5MGMtNDRiYi05ODY0LTQ2ODc"
                    + "3NjZkMzQ5YSIsDQogICAgIm5iZiI6IDE1MjM0MTUyNTYsDQogICAgImlzcyI6ICJ3c2RldmludGVybmFsLnVzYWEuY29tIiwNCi"
                    + "AgICAiZXhwaXJlc19pbiI6IDYwMCwNCiAgICAiaWF0IjogMTUyMzQxNTI1NiwNCiAgICAiZXhwIjogMTUyMzQxNTg1NiwNCiAgI"
                    + "CAicGFydHlfaWQiOiAiMDI5MzAwMjE3IiwNCiAgICAicGFydHlfdHlwZSI6ICJVU0FBIiwNCiAgICAiYXVkIjogInJzdmNpbnQu"
                    + "dXNhYS5jb20iLA0KICAgICJqdGkiOiAiYzliM2NjZDMtMjU3NS00MWUyLTgxZGQtNjgxOTlkMGM4MTIyIiwNCiAgICAidG9rZW5"
                    + "fdHlwZSI6ICJqd3QiDQp9.wkST0qHE0NkOKAfRNtvhu-QMj6U5QGmTWaLM_bYsFswsyhXPoALGvWPPgthE3-uNxffWNQaz7OYza"
                    + "4MSOR7qCdk9VCtdh29A9zj-Kbo1KPbFJuJRwq5xwr7q7P2VI-gOMqDrH2GuoMWsujzjWHWIR3MRc5uLU9u_mfm8RXV7LoO2DDIZ"
                    + "SbbVr3RADZWm9PTYH79cZbdKHRN8UyNGJk1PftF1nma4GdhRDq3mTITpMXWTktaXEI1bpEbP9L5hNmv6fVWh8ExWH1bCsvhLLsE"
                    + "RSWNpQFYx4jawbUTsN8gk_VvNWSTRZJ5uC-VCYlL-2jjEByoNRn5U8h2hWicGAZMkh3cOvrv9HeelFLb476gyLxeL2vS7wSLKtK"
                    + "DvyzUvEs8bnMTkXO-11LqeiB_4-KXTKGdULw72tTJkNcTV-lA3DzooXShU2zCvj9JpvdATcaBUiCVUBOW1wZnBhz3LZTygpRc0y"
                    + "punc9WbHtezR3w3xKezcHQiyMNy2uumoclb8AvXly8n3CipPNECHY35nkNtXwA1paRtASzXslL2P8I0B5pRRxGwqWJRUQV30gNh"
                    + "0FVo-yUZucMtbU_uE1haegKSvCUxk_vrbrAm40X7TLPiiv479girVI2f1EnPOfT669SnMJfvmXGKL_rU9IWjphzP22APFbDAR1h"
                    + "PuV5m9TAcEnQ";

    public static final String EMP_ID = "UN99959";
    // Sample token for Employee UN99959
    public static final String EMP_TOKEN =
            "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiIsImtpZCI6ImlkLWNsYWltLWp3dC1rZXkifQ."
                    + "ew0KICAgICJ0b2tlbl9uYW1lIjogImlkX2NsYWltIiwNCiAgICAic3ViIjogIlVOOTk5NTkiLA0KICAgICJuYmYiOiAxNTk0ODIyNj"
                    + "A3LA0KICAgICJpc3MiOiAid3NkZXZpbnRlcm5hbC51c2FhLmNvbSIsDQogICAgImV4cGlyZXNfaW4iOiA2MDAsDQogICAgImlhdCI6"
                    + "IDE1OTQ4MjI2MDcsDQogICAgImV4cCI6IDE1OTQ4MjMyMDcsDQogICAgInBhcnR5X2lkIjogIlVOOTk5NTkiLA0KICAgICJwYXJ0eV"
                    + "90eXBlIjogIkVNUEUiLA0KICAgICJhdWQiOiAicnN2Y2ludC51c2FhLmNvbSIsDQogICAgImp0aSI6ICI1ZTQyZmM0ZS1lZjk0LTRk"
                    + "ODEtYWU4ZS0zZTc2ZjQyMWZjMWYiLA0KICAgICJ0b2tlbl90eXBlIjogImp3dCIsDQogICAgInJlYWxtIiA6ICJlbXBsb3llZSINCn"
                    + "0.lyIFVurqMS0oo-KYwJjSFjYlpVtfJ7wptNp5G-r1Htg0W_KkPhdi4RiSZypwxQb3gjNY3tI0IiTmF0ONcMZjrHdoOH-B-JerBcT2"
                    + "evfhCZ3esmz4jVtqXIwpLTYnJLyeKKBjfRSaueu82aiI_531nnrzlzPLdvflL0m-R7RIt6gIN_-irhzRUwqc_Sx8yOwrtEoVVYACTN"
                    + "--pYNL2Y_Q0gcwxf_zLNh3Gp-PzLzv1_c0YIS4XWMS2fT7Lm9cVSvkocKZ8B27CDNptrmiuwlhTHNOwlRjwkLzrJQ4dp1yDbvOB-zd"
                    + "kQspT0i3u2Cp9V3TgadFm2L9DUrwq76ycyX_C1QxVDD7Yn51g5FqiO6WorbLlrpyKmMy047SONXHEkwv6NWeFaDekEtm1kr7km6obI"
                    + "nGyBbjTv_1Sk1hPIz4bXkn3lKVhxUJm0ElZ9AzF7eMY5zBGdAFQ0IanDZOfHgQwiooqeoBeUSen2-GlsYHJ2fHusepEkU138sWUliY"
                    + "YvIw-DJCbFO-uNr3t1msNOChSiU8Bya3go0GvjjBDn2VBGILB_chsbAx1S0bEsK0AtWHkwrnbkw5Ip7LOv8ngCs4R885igqs3fKohL"
                    + "wxQF51Dua78qGpaaPqWCm6WEm_4MXNarXsGhtNl0ph1IqmN92ZSCZcob946GnkdA5jOkQCKv8";

    public static final String TEST_USER = "UN98872";
    // Sample token for Employee UN98872
    public static final String TEST_TOKEN =
            "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiIsImtpZCI6ImlkLWNsYWltLWp3dC1rZXkifQ."
                    + "ew0KICAgICJ0b2tlbl9uYW1lIjogImlkX2NsYWltIiwNCiAgICAic3ViIjogInVuOTg4NzIiLA0KICAgICJuYmYiOiAxNTgyMjEzNjA"
                    + "2LA0KICAgICJpc3MiOiAid3NkZXZpbnRlcm5hbC51c2FhLmNvbSIsDQogICAgImV4cGlyZXNfaW4iOiA2MDAsDQogICAgImlhdCI6ID"
                    + "E1ODIyMTM2MDYsDQogICAgImV4cCI6IDE1ODIyMTQyMDYsDQogICAgInBhcnR5X2lkIjogInVuOTg4NzIiLA0KICAgICJwYXJ0eV90e"
                    + "XBlIjogIkVNUEUiLA0KICAgICJhdWQiOiAicnN2Y2ludC51c2FhLmNvbSIsDQogICAgImp0aSI6ICIzZmM2ODRlZC01MDVlLTRhM2UtO"
                    + "TBhZi1iM2I4ZTY5YzVkMzkiLA0KICAgICJ0b2tlbl90eXBlIjogImp3dCIsDQogICAgInJlYWxtIiA6ICJlbXBsb3llZSINCn0.dwfVT"
                    + "obBKkVUTML8_Mk6Cbk0BASvoZg_n_LsEXYERa4IaAlHafUMXdjTRj3CG1XDftVIZpABooHRse11wTcRXYAeAPNmR64ElRWYJ2meN2u0p"
                    + "X0T55zfKr-xM-x8UnwbbDLNBjoTC_Vpv-I_qezDrfj5iM6erTx2ftI0BTWYXTaSxBbh0ToiBdKZoGA0t8dMygNkIIDgt5MrvmP_tg8w_"
                    + "NwzfRTeJzob4rUKK6QaQQxoq9CTHNXpqiDaaSOVw5d5yXq88NSrh-9W8z-KgW72RWYYvkkvvvvI1bk2lswBQA8CdNUROOE4zzzAcQxH9"
                    + "OMB4ea4d032fcE57E294DafXhwWm0RdNXLBXoAwQ0K0XDaZnalxhL8qIKuYc_jMmSby1ys2xeyqg7x5GNVJbxoRCO6nSTQEytavMDqMM"
                    + "0lfJFaEShRF6sNVcpgmuseOtBZjJvwD43Iq0W5piop5ghk3pvN_c1WTgJ_Zr66EakilRvtI6SldVswyfZAYNd6NOjjCRBwwcuYvF__L-"
                    + "I2bkFPW3Usr8ZUn1zFQPXUF4Bmg_7lUsvXNjSJoOtLYYvj2j3SuI-uiEoFQFj8Na46k7MGI1C-3h9PgoumeUzzHX-pZHF68_VC8xbFcs"
                    + "FbVaCGa_g_VQRC8sKKnoRoQl_qmCPIJ-iihBN6GSd7hYzs0JrUKR28";

    public static final String HR_TEST_USER = "WDEVTFWT";
    // Sample toke for HR Service Account WDEVTFWT (need to update to our Service Account)
    public static final String HR_TEST_TOKEN =
            "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiIsImtpZCI6ImlkLWNsYWltLWp3dC1rZXkifQ.ew0KICAgIC"
                    + "J0b2tlbl9uYW1lIjogImlkX2NsYWltIiwNCiAgICAic3ViIjogIldERVZURldUIiwNCiAgICAibmJmIjogMTYyMDMxOTM2NywNCiAgICAi"
                    + "aXNzIjogIndzZGV2aW50ZXJuYWwudXNhYS5jb20iLA0KICAgICJleHBpcmVzX2luIjogNjAwLA0KICAgICJpYXQiOiAxNjIwMzE5MzY3LA"
                    + "0KICAgICJleHAiOiAxNjIwMzE5OTY3LA0KICAgICJwYXJ0eV9pZCI6ICJXREVWVEZXVCIsDQogICAgInBhcnR5X3R5cGUiOiAiRU1QRSIsD"
                    + "QogICAgImF1ZCI6ICJyc3ZjaW50LnVzYWEuY29tIiwNCiAgICAianRpIjogIjcwMjU4ODhhLWE4ODMtNDg1Yi04ZDdjLWYyZDY4ODc4Mzk4"
                    + "MyIsDQogICAgInRva2VuX3R5cGUiOiAiand0IiwNCiAgICAicmVhbG0iIDogImVtcGxveWVlIg0KfQ.ea7h_d3BE0Ll9SGiS-PPE3I2RvStF"
                    + "iYdUreO9W8O2MgoutTldx1nVP_j3QVeA8RnhFIrvkEwS6TkKpZ0iVExpjoKqiXdlziXK61vucekzSgVZyOh_9e5OPFzC_kyLyGhIbLdCvW_z"
                    + "48GD1Rrqjx8inmgMkaf1Or4fuCKYHyDCLYmgtQ_LZyBZe73REfVAtQEQW9TSY2hR33XoYukDl3FbXYwdTAz8b7NkYAjcJ8Z1hRNNWZbWq7tt"
                    + "a3Zx_zF06qEEpdrBr5X2697s6DAb9pPS4DoTtKdUbFGyX7cJVAR9ZVH0oTvRDKCsbre13LcB9ToUM89fFQt-LGqWD053bS73HnmMtTPhwbMBN"
                    + "N4KIqgzv7_GceIZEUKjUArK5w-ZmIccOd_YH4DATPtRm-ELOR3tV7CyTFvcQs0fNLsCOQdtD2-rJKqg0QbjcpV7ezcDFffeltGDpo43nIdsPL"
                    + "DgufpL8WZOTm11v3x4gb7Xn6cx76HV2v7TaT3yBCMnqa0O3LayApWt-DFf9Qb3LQa6cO27HXoIlqVv9ABXDuIHXdDPkbFdvUFCpcyZ4G1nuTO"
                    + "G7eYZ8G3x6_VT27Jk2FLRZnOPLTmiQ2LBH2V9QzkfxTajtwi6gpKX9VVeTnfIOdPqDJmMAaHLXzF97Tx87vdWlf2a6DoVd5JBiHHH8LrAlnylaY";

    // This is required to have the DevelopmentTokenValidator.java
    @Bean
    public static PropertySourcesPlaceholderConfigurer properties() {
        final PropertySourcesPlaceholderConfigurer pspc =
                new PropertySourcesPlaceholderConfigurer();
        final Properties properties = new Properties();
        properties.setProperty("usaa.security.enable-dev-validator", "true");
        pspc.setProperties(properties);
        return pspc;
    }

    // Override Token Validator, any JWT with the USAA claims will work
    @Bean
    public TokenValidator tokenValidator() {
        return new DevelopmentTokenValidator();
    }
}
===================================================================


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
//            ,@PathVariable("templateVersion") @NonNull @Size(min = 1, max = 50) String templateVersion
    ) {

        List<TemplateProfileDto> templateProfileList = new ArrayList<>();
//        LOGGER.info("Searching for template details for templateName: {} and templateVersion: {}",
//                templateName, templateVersion);

        try {
//            templateProfileList = templateProfileService.getTemplatesByNameAndVersion(templateName, templateVersion);

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

