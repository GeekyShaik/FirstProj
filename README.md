@Test
public void getTemplateProfile_Success() throws Exception {
    String VALID_TEMPLATE_NAME = "ECM Delivery NewBrand";
    Integer VALID_TEMPLATE_VERSION = 1;

    // Assuming TemplateProfileDto has a no-args constructor and setters for name and version
    TemplateProfileDto mockTemplateProfileDto = new TemplateProfileDto();
    mockTemplateProfileDto.setTemplateName(VALID_TEMPLATE_NAME); // Replace with actual setter method name
    mockTemplateProfileDto.setTemplateVersion(VALID_TEMPLATE_VERSION); // Replace with actual setter method name

    List<TemplateProfileDto> templateProfileList = new ArrayList<>();
    templateProfileList.add(mockTemplateProfileDto);

    // Stubbing the service method
    when(templateProfileService.getTemplatesByNameAndVersion(VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
        .thenReturn(templateProfileList);

    // Perform the request and expect the correct content type and status
    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}",
            VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION))
        .andExpect(content().contentType(MediaType.APPLICATION_JSON))
        .andExpect(status().isOk());
}
