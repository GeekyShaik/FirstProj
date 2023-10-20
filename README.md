@Bean
public ModelMapper modelMapper() {
    ModelMapper mapper = new ModelMapper();
    
    // Define property mapping
    PropertyMap<omMsgTemplateEntity, TemplateProfileObjectDTO> orderMap = new PropertyMap<omMsgTemplateEntity, TemplateProfileObjectDTO>() {
        protected void configure() {
            map().setTemplateName(source.getObjectName());
            map().setTemplateVersion(source.getObjectVersion());
            map().setVariableSchemaName(source.getVariableSchemaNm());
            map().setGoGetCollectionName(source.getGoGetCollectionNm());
            map().setTemplateVersionEffectiveDate(source.getMsgTemplVersionEffectiveDt());
            map().setDisplay(source.getDisplayNm());
            map().setDisplayComment(source.getDisplayCommentTxt());
            // ... (Continue for all the other fields)

            // Mapping lists (assuming DTOs for these lists exist)
            using(ctx -> ((omMsgTemplateEntity) ctx.getSource()).getOmVariableTemplateRltnEntityList())
                .map(source, destination.getVariableTemplateRelationList());
            using(ctx -> ((omMsgTemplateEntity) ctx.getSource()).getOmGroupTemplateRltnEntityList())
                .map(source, destination.getGroupTemplateRelationList());
        }
    };
    
    mapper.addMappings(orderMap);
    return mapper;
}
