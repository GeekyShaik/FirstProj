  @Test
           public void getTemplateProfile_Returns200() throws Exception {
               // Arrange
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
               // ... (set other properties as shown in the Postman response)
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
