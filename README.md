

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

import org.springframework.http.MediaType;
import org.springframework.security.config.annotation.web.WebSecurityConfigurer;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;


import java.time.LocalDate;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;


import static org.mockito.Mockito.when;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.hamcrest.Matchers.is;


@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers= ObjectProfileController.class, excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})
       public class TemplateProfileControllerTest {

           @Autowired
           private MockMvc mockMvc;

           @MockBean
           private TemplateProfileService templateProfileService;



           @BeforeEach
           public void setUp() {
               // Setup can be done here if needed
           }

    private TemplateProfileDto createTemplateProfileDto() {
        TemplateProfileDto dto = new TemplateProfileDto();
        // Set all properties on dto according to your actual DTO
        dto.setTemplateName("ECM_Delivery_NewBrnd");
        dto.setTemplateVersion(1);
        dto.setMsgTemplVersionEffectiveDt(LocalDate.ofEpochDay(2018 - 11 - 01));
        dto.setEnterpriseDocumentId("55498");
        dto.setVariableSchemaNm("PnC_Auto_Corr");
        dto.setOpsProcedureAreaAc("PC");
        dto.setOpsProcedureCategoryAc("PS");
        dto.setOpsProcedureSubcategoryAc("AO");
        dto.setMessageTemplateTypeCd("m");
        dto.setDisplayNm("Rental Reimbursement And/Or Towing And Labor Coverage - AO96");
        dto.setRegulatoryClassCd("N");
        dto.setSourceSystemDocumentId("55498_MT_No_Rent_Reimb_Pol");
        dto.setSourceToolDc("XPRESSION");
        dto.setGoGetCollectionNm("PNC_STD_USER_CCIF_MISC");
        dto.setChannelDefaultCd("POSTAL");
        dto.setChannelOptionDefaultCd("");
        dto.setPageNumHorzAlignCd("D");
        dto.setPageNumTypeCd("");
        dto.setPageNumVertAlignCd("");
        dto.setPageNumDisplayRangeCd("");
        dto.setPageNumShowCountInd("");
        dto.setPageNumCountRangeCd("");
        dto.setContNumAcrsCccoverInd("");
        dto.setContNumAcrsMasterInd("");
        dto.setContNrAcrossEnclInd("");

        List<OmMsgTemplateChannelDto> allowedChannels = new ArrayList<>();
        allowedChannels.add(new OmMsgTemplateChannelDto("EMAIL", "NA", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("FAX", "NA", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "CERT", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "EXPRESS", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "IMMED", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "USPOST", null));
        allowedChannels.add(new OmMsgTemplateChannelDto("UDO", "NA", "PC GEN CORRESPONDENCE"));
        dto.setAllowedChannels(allowedChannels);
return dto;
    }

    @Test
           public void getTemplateProfile_Returns200() throws Exception {
                         TemplateProfileDto dto = createTemplateProfileDto();


               List<TemplateProfileDto> mockResponse = Collections.singletonList(dto);

               when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                       .thenReturn(mockResponse);

               // Act & Assert
               mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                               .accept(MediaType.APPLICATION_JSON))
                       .andExpect(status().isOk())
                .andExpect(jsonPath("$[0].templateName", is("ECM_Delivery_NewBrnd")))
               .andExpect(jsonPath("$[0].templateVersion", is(1)));
               // ... (additional assertions can be added to verify the structure of the returned DTO)
           }

    @Test
    public void getTemplateProfile_Returns404() throws Exception {
        // Arrange
        when(templateProfileService.getTemplatesByNameAndVersion("NonExistentTemplate", 1))
                .thenReturn(Collections.emptyList());

        // Act & Assert
        mockMvc.perform(get("/template-details/template-name/NonExistentTemplate/template-version/1")
                        .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound());
    }       }
