I appreciate your patience. It seems I made an error in my previous responses. I apologize for the confusion. If you want to inject the `ModelMapper` from `TemplateProfileModelMapperConfig`, you need to make sure that the `ModelMapper` bean is properly defined in that configuration class. Here is a corrected version:

```java
@Configuration
public class TemplateProfileModelMapperConfig {

    @Bean
    public ModelMapper modelMapper() {
        ModelMapper mapper = new ModelMapper();
        // Your custom mappings here
        return mapper;
    }
}

@Service
public class TemplateProfileService {

    private final OmMessageTemplateRepository templateRepository;
    private final ModelMapper modelMapper;

    @Autowired
    public TemplateProfileService(
            OmMessageTemplateRepository templateRepository,
            ModelMapper modelMapper) {
        this.templateRepository = templateRepository;
        this.modelMapper = modelMapper;
    }

    public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, BigDecimal templateVersion) {
        List<OmMsgTemplateEntity> entities = templateRepository.findTemplateProfile(templateName, templateVersion);

        // Use the injected ModelMapper bean
        List<TemplateProfileDto> templateProfileDtoList = entities.stream()
                .map(entity -> modelMapper.map(entity, TemplateProfileDto.class))
                .collect(Collectors.toList());

        return templateProfileDtoList;
    }
}
```

Ensure that the `TemplateProfileModelMapperConfig` class is properly scanned by Spring or included in your main application class annotated with `@SpringBootApplication`. This will allow Spring to create and manage the `ModelMapper` bean, making it available for injection into the `TemplateProfileService`.