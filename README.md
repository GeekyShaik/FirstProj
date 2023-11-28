@Configuration
public class TemplateProfileModelMapperConfig {

    @Bean
    public ModelMapper modelMapper() {
        ModelMapper mapper = new ModelMapper();
        PropertyMap<OmMsgTemplateEntity, TemplateProfileDto> orderMap = new PropertyMap<OmMsgTemplateEntity, TemplateProfileDto>() {
            protected void configure() {
                map().setTemplateName(source.getOmMsgTemplateEntityPK().getTemplateName()); // getObjectName());
                map().setTemplateVersion(source.getOmMsgTemplateEntityPK().getTemplateVersion());  //getObjectVersion());
                map().setVariableSchemaName(source.getVariableSchemaNm());
                 map().setGoGetCollectionName(source.getGoGetCollectionNm());
                map().setEffectiveDate(source.getMsgTemplVersionEffectiveDt());
                map().setDisplayName(source.getDisplayNm());
                 map().setBusArea(source.getOpsProcedureAreaAc());
                map().setBusCategory(source.getOpsProcedureCategoryAc());
                map().setBusSubCategory(source.getOpsProcedureSubcategoryAc());
//                map().setChannelDefaultCode(source.getChannelDefaultCd());
                map().setEnterpriseDocId(source.getEnterpriseDocumentId());
                map().setSourceTool(source.getSourceToolDc());
                map().setMessageTemplateTypeCd(source.getMessageTemplateTypeCd());
                map().setRegulatoryClassification(source.getRegulatoryClassCd());
                map().setSourceSystemDocumentId(source.getSourceSystemDocumentId());
                map().setPageNumberHorizAlignCd(source.getPageNumHorzAlignCd());
                map().setPageNumberTypeCd(source.getPageNumTypeCd());
                map().setPageNumberVertAlignCd(source.getPageNumVertAlignCd());
                map().setPageNumberDisplayRangeCd(source.getPageNumDisplayRangeCd());
                map().setShowPageCountInd(source.getPageNumShowCountInd());
                map().setChannelDefaultCode(source.getChannelDefaultCd());
                map().setChannelDefaultOptionCode(source.getChannelOptionDefaultCd());
                map().setContinousNumberingAcrossCCCover(source.getContNumAcrsCccoverInd());
                map().setContinousNumberingAcrossMaster(source.getContNumAcrsMasterInd());
                map().setContinousNumberingAcrossEnclosures(source.getContNrAcrossEnclInd());

            }
        };

        mapper.addMappings(orderMap);
        return mapper;
    }


}

