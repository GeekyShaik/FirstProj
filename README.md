@Service
public class GroupDefinitionService {

    @Autowired
    private OmGroupDefinitionRepository definitionRepository;

    @Autowired
    private ModelMapper modelMapper;

    public List<GroupDefinitionsObjectDto> getAllWithTemplateRltn() {
        List<OmGroupDefinition> entities = definitionRepository.findAllWithTemplateRltn();
        // Use ModelMapper to map the list of entities to DTOs
        List<GroupDefinitionsObjectDto> groupDefinitionsObjectDtoList = entities.stream()
                .map(entity -> modelMapper.map(entity, GroupDefinitionsObjectDto.class))
                .collect(Collectors.toList());

        return groupDefinitionsObjectDtoList;
    }
}

