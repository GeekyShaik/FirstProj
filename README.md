package com.usaa.ect;
import com.usaa.ect.dtos.TemplateProfileDTO;
import com.usaa.ect.entities.OmMsgTemplateEntity;
import org.modelmapper.PropertyMap;
import org.springframework.context.annotation.Bean;


public class ModelMapper {

    @Bean
    public ModelMapper modelMapper() {
        ModelMapper mapper = new ModelMapper();


        PropertyMap<OmMsgTemplateEntity, TemplateProfileDTO> orderMap = new PropertyMap<OmMsgTemplateEntity, TemplateProfileDTO>() {
            protected void configure() {
                map().setTemplateName(source.getObjectName());
                map().setTemplateVersion(source.getObjectVersion());
                map().setVariableSchemaName(source.getVariableSchemaNm());
               
                map().setSourceTool(source.getSourceToolDc());
                map().setMessageTemplateTypeCd(source.getMessageTemplateTypeCd());
                map().setRegulatoryClassification(source.getRegulatoryClassCd());
                map().setChannelDefaultOptionCode(source.getChannelOptionDefaultCd());
                
            }
        };

        mapper.addMappings(orderMap);
        return mapper;
    }

   
}

