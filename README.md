@Test
public void getTemplateProfile_Returns200() throws Exception {
    // Create and populate the TemplateProfileDto
    TemplateProfileDto dto = new TemplateProfileDto();
    dto.setTemplateName("ECM_Delivery_NewBrnd");
    dto.setTemplateVersion(1);
    dto.setEffectiveDate(LocalDate.parse("2019-08-19"));
    dto.setEnterpriseDocId("33333");
    dto.setVariableSchemaName("ECM_Delivery_NewBrnd");
    dto.setBusArea("BK");
    dto.setBusCategory("ACG");
    dto.setBusSubCategory("ACG");
    dto.setMessageTemplateTypeCd("M");
    dto.setDisplayName("testAutomationNameUpdated832");
    dto.setRegulatoryClassification(null); // Assuming null is expected here
    dto.setSourceSystemDocumentId("testAutomationVIRTUAL123");
    dto.setSourceTool("METADATA");
    dto.setGoGetCollectionName("10_28GOGET1");
    dto.setChannelDefaultCode(null); // Assuming null is expected here
    dto.setChannelDefaultOptionCode(null); // Assuming null is expected here
    dto.setPageNumberHorizAlignCd("right");
    dto.setPageNumberTypeCd("D");
    dto.setPageNumberVertAlignCd("bottom");
    dto.setPageNumberDisplayRangeCd("ALL");
    dto.setShowPageCountInd("Y");
    dto.setPageNumberCountRangeCd("ALL");
    dto.setContinousNumberingAcrossCCCover("Y");
    dto.setContinousNumberingAcrossMaster("Y");
    dto.setContinousNumberingAcrossEnclosures(null); // Assuming null is expected here
    dto.setAllowedChannels(new ArrayList<>());

    // Prepare the mock response
    List<TemplateProfileDto> mockResponse = new ArrayList<>();
    mockResponse.add(dto);

    // Set up the mock service to return the mockResponse when called
    when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt())).thenReturn(mockResponse);

    // Perform the request and expect a 200 status
    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk());
}
