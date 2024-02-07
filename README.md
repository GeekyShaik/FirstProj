@Test
    void getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException() throws Exception {
        List<TemplateProfileDto> mockResponse = Collections.singletonList(templateProfileDto);

        when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
                .thenThrow(new RuntimeException("Unexpected error"));
        HttpHeaders headers =new HttpHeaders();
        headers.add("id-claim",ITAuthConfig.EMP_TOKEN);
       // headers.add("roles","[\"ROLE_GENESIS_EXP\"]");
       /*when(templateProfileService.getTemplatesByNameAndVersion(anyString(), anyInt()))
               .thenThrow(new RuntimeException("Unexpected error"));*/

        mockMvc.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1"))
                .andExpect(status().isInternalServerError());
    }
