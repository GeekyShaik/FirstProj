@Test
public void getTemplateProfile_Returns200() throws Exception {
    // ... (other setup code)

    // Create a list of OmMsgTemplateChannelDto objects
    List<OmMsgTemplateChannelDto> allowedChannels = new ArrayList<>();
    allowedChannels.add(new OmMsgTemplateChannelDto("EMAIL", "NA", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("FAX", "NA", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "CERT", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "EXPRESS", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "IMMED", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("POSTAL", "USPOST", null));
    allowedChannels.add(new OmMsgTemplateChannelDto("UDO", "NA", "PC GEN CORRESPONDENCE"));

    // Assuming TemplateProfileDto has a setter for the allowedChannels list
    TemplateProfileDto dto = new TemplateProfileDto();
    dto.setTemplateName("ECM_Delivery_NewBrnd");
    dto.setTemplateVersion(1);
    dto.setAllowedChannels(allowedChannels);
    // ... set other properties as required

    List<TemplateProfileDto> mockResponse = Arrays.asList(dto);

    when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
            .thenReturn(mockResponse);

    mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
                    .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            // Optionally, verify the content of the response
            .andExpect(jsonPath("$[0].allowedChannels", hasSize(7)))
            .andExpect(jsonPath("$[0].allowedChannels[0].channelName", is("EMAIL")))
            // ... additional assertions for other channels
            ;
}
