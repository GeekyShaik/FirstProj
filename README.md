@RestController
@RequestMapping("/group-definitions")
public class GroupDefinitionController {

    private final GroupDefinitionService groupDefinitionService;

    @Autowired
    public GroupDefinitionController(GroupDefinitionService groupDefinitionService) {
        this.groupDefinitionService = groupDefinitionService;
    }

    @GetMapping("/all-with-template")
    public ResponseEntity<List<GroupDefinitionsObjectDto>> getAllWithTemplateRelation() {
        List<GroupDefinitionsObjectDto> groupDefinitions = groupDefinitionService.getAllWithTemplateRltn();
        return new ResponseEntity<>(groupDefinitions, HttpStatus.OK);
    }
}