

package com.usaa.ect;

import com.usaa.ect.controller.ObjectProfileController;
import com.usaa.ect.dtos.OmMsgTemplateChannelDto;
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
import java.time.LocalDate;

@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers= ObjectProfileController.class, excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})
        public class TemplateProfileControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

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


        }

