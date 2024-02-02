@Test
public void getTemplateProfile_Returns200() throws Exception {
    List<TemplateProfileDto> mockResponse = new ArrayList<>();
    mockResponse.add(new TemplateProfileDto()); // Empty DTO, details are not important for this test

    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(mockResponse);

    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk());
}

@Test
public void getTemplateProfile_Returns404_WhenNoDataFound() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(Collections.emptyList());

    mockMvc.perform(get("/template-details/template-name/NonExistent/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isNotFound());
}

@Test
public void getTemplateProfile_Returns500_WhenServiceThrowsException() throws Exception {
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenThrow(new RuntimeException("Internal error"));

    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isInternalServerError());
}
