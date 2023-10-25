package com.usaa.ect;
import com.usaa.ect.dtos.TemplateProfileDTO;
import com.usaa.ect.entities.OmMsgTemplateEntity;
import org.modelmapper.PropertyMap;
import org.springframework.context.annotation.Bean;


public class ModelMapper {

    @Bean
    public ModelMapper modelMapper() {
        ModelMapper mapper = new ModelMapper();

// Define property mapping
        PropertyMap<OmMsgTemplateEntity, TemplateProfileDTO> orderMap = new PropertyMap<OmMsgTemplateEntity, TemplateProfileDTO>() {
            protected void configure() {
                map().setTemplateName(source.getObjectName());
                map().setTemplateVersion(source.getObjectVersion());
                map().setVariableSchemaName(source.getVariableSchemaNm());
                // map().setGoGetCollectionName(source.getGoGetCollectionNm());
                map().setEffectiveDate(source.getMsgTemplVersionEffectiveDt());
                map().setDisplayName(source.getDisplayNm());
                //  map().setBusArea(); //ops_procedure_area
                // map().setBusCategory();//ops_procedure_Category
                map().setChannelDefaultCode(source.getChannelDefaultCd());
                // map().setChannelName();
                map().setEnterpriseDocId(source.getEnterpriseDocumentId());
                map().setGogetCollectionName(source.getGoGetCollectionNm());
                //map().getSubChannelName();
                map().setSourceTool(source.getSourceToolDc());
                map().setMessageTemplateTypeCd(source.getMessageTemplateTypeCd());
                map().setRegulatoryClassification(source.getRegulatoryClassCd());
                map().setSourceSystemDocumentId(source.getSourceSystemDocumentId());
                map().setPageNumberHorizAlignCd(source.getPageNumHorzAlignCd());
                map().setPageNumberTypeCd(source.getPageNumTypeCd());
                map().setPageNumberVertAlignCd(source.getPageNumVertAlignCd());
                map().setChannelDefaultCode(source.getChannelDefaultCd());
                map().setChannelDefaultOptionCode(source.getChannelOptionDefaultCd());
                
            }
        };

        mapper.addMappings(orderMap);
        return mapper;
    }

   
}

