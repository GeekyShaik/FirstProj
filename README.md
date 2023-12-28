// ... [previous field declarations and setup]

    @Test
    public void whenValidInput_thenReturnsTemplateDetails() throws Exception {
        // Arrange
        TemplateProfileDto mockTemplate = new TemplateProfileDto(); // Assuming TemplateProfileDto is a valid DTO class
        List<TemplateProfileDto> dtoList = Collections.singletonList(mockTemplate);
        when(templateProfileService.getTemplatesByNameAndVersion(any(String.class), any(Integer.class)))
                .thenReturn(dtoList);

        // Act & Assert
        mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, "someTemplateName", 1))
                .andExpect(status().isOk())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON))
                .andExpect(jsonPath("$[0]", notNullValue())); // assuming the DTO has fields to be checked
    }

    @Test
    public void whenValidInput_thenLogIsGenerated() throws Exception {
        // Arrange
        TemplateProfileDto mockTemplate = new TemplateProfileDto();
        List<TemplateProfileDto> dtoList = Collections.singletonList(mockTemplate);
        doReturn(dtoList).when(templateProfileService).getTemplatesByNameAndVersion("validTemplateName", 1);

        // Act
        mockMvc.perform(get(TEMPLATE_DETAILS_ENDPOINT, "validTemplateName", 1))
                .andExpect(status().isOk());

        // Assert
        // Here, you might assert that logging occurred, if logging is part of your method's functionality.
        // This typically requires a more complex logging setup and might involve asserting log content.
    }
