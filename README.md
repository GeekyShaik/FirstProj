@Test
public void getTemplateProfile_Returns200Ok_WhenDataExists() throws Exception {
    // "Actual" values that you're using to call the service
    String actualTemplateName = "ECM_Delivery_NewBrnd";
    Integer actualTemplateVersion = 1;

    // "Expected" values that you anticipate the service will return
    TemplateProfileDto expectedDto = new TemplateProfileDto();
    expectedDto.setTemplateName(actualTemplateName);
    expectedDto.setTemplateVersion(actualTemplateVersion); // This should match the "actual" for the test to pass

    // Setting up the mock service to return the "expected" DTO
    when(templateProfileService.getTemplatesByNameAndVersion(actualTemplateName, actualTemplateVersion))
            .thenReturn(Collections.singletonList(expectedDto));

    // Perform the request and assert the results
    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
                        actualTemplateName, actualTemplateVersion))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].templateName").value(actualTemplateName))
            .andExpect(jsonPath("$[0].templateVersion").value(actualTemplateVersion)); // This assertion checks the "actual" against the "expected"
}
