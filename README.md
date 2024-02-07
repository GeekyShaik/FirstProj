@Test
public void getTemplateProfile_Returns500_WhenUnexpectedErrorOccurs() throws Exception {
    // Mock the service to throw a RuntimeException for this scenario
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
            .thenThrow(new RuntimeException("Unexpected error occurred"));

    // Perform the request and expect a 500 Internal Server Error response
    mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", VALID_TEMPLATE_NAME, VALID_TEMPLATE_VERSION)
                    .headers(localTestHeaders))
            .andExpect(status().isInternalServerError())
            .andDo(MockMvcResultHandlers.print());
}
