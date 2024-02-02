package com.usaa.ect;

import com.usaa.ect.controller.ObjectProfileController;
import com.usaa.ect.dtos.TemplateProfileDto;
import com.usaa.ect.service.TemplateProfileService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.web.context.WebApplicationContext;
import static org.mockito.ArgumentMatchers.anyInt;
import static org.mockito.ArgumentMatchers.anyString;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = ObjectProfileController.class, excludeFilters = {
    @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurerAdapter.class)
})
public class TemplateProfileControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private TemplateProfileService templateProfileService;

    @BeforeEach
    public void setUp(WebApplicationContext webApplicationContext) {
        mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
    }

    @Test
    public void getTemplateProfile_Returns200() throws Exception {
        List<TemplateProfileDto> mockResponse = new ArrayList<>();
        mockResponse.add(new TemplateProfileDto()); // Assume this constructor initializes a valid DTO instance.
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(mockResponse);

        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(print());
    }

    @Test
    public void getTemplateProfile_WithNonexistentTemplateName_Returns404() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(Collections.emptyList());

        mockMvc.perform(get("/template-details/template-name/NonexistentTemplateName/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isNotFound())
                .andDo(print());
    }

    @Test
    public void getTemplateProfile_ServiceThrowsException_Returns500() throws Exception {
        when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenThrow(new RuntimeException("Internal server error"));

        mockMvc.perform(get("/template-details/template-name/ValidTemplateName/template-version/1")
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isInternalServerError())
                .andDo(print());
    }
}
