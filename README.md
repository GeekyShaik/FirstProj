Certainly, if you have a custom `TemplateProfileModelMapperConfig` class, and you want to inject an instance of this configuration class into your service, you can follow these steps:

Assuming your `TemplateProfileModelMapperConfig` looks like this:

```java
@Configuration
public class TemplateProfileModelMapperConfig {

    @Bean
    public TemplateProfileModelMapper templateProfileModelMapper() {
        return new TemplateProfileModelMapper();
    }
}
```

And your custom mapper class `TemplateProfileModelMapper` extends `ModelMapper`:

```java
public class TemplateProfileModelMapper extends ModelMapper {

    public TemplateProfileModelMapper() {
        // Your custom mappings here
    }
}
```

You can then inject `TemplateProfileModelMapperConfig` into your service like this:

```java
@Service
public class TemplateProfileService {

    private final OmMessageTemplateRepository templateRepository;
    private final TemplateProfileModelMapperConfig templateProfileModelMapperConfig;

    @Autowired
    public TemplateProfileService(
            OmMessageTemplateRepository templateRepository,
            TemplateProfileModelMapperConfig templateProfileModelMapperConfig) {
        this.templateRepository = templateRepository;
        this.templateProfileModelMapperConfig = templateProfileModelMapperConfig;
    }

    public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, BigDecimal templateVersion) {
        List<OmMsgTemplateEntity> entities = templateRepository.findTemplateProfile(templateName, templateVersion);

        // Use the TemplateProfileModelMapperConfig to get the TemplateProfileModelMapper bean
        TemplateProfileModelMapper templateProfileModelMapper = templateProfileModelMapperConfig.templateProfileModelMapper();

        // Use the injected TemplateProfileModelMapper bean
        List<TemplateProfileDto> templateProfileDtoList = entities.stream()
                .map(entity -> templateProfileModelMapper.map(entity, TemplateProfileDto.class))
                .collect(Collectors.toList());

        return templateProfileDtoList;
    }
}
```

This way, you're injecting the `TemplateProfileModelMapperConfig` instance and then using it to obtain the `TemplateProfileModelMapper` bean, which you can use for mapping.