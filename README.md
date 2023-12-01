Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'groupDefinitionService': Unsatisfied dependency expressed through field 'definitionRepository': Error creating bean with name 'omGroupDefinitionRepository' defined in com.usaa.ect.repositories.OmGroupDefinitionRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: This class [class com.usaa.ect.entities.OmGroupDefinition] does not define an IdClass
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'omGroupDefinitionRepository' defined in com.usaa.ect.repositories.OmGroupDefinitionRepository defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration: This class [class com.usaa.ect.entities.OmGroupDefinition] does not define an IdClass
Caused by: java.lang.IllegalArgumentException: This class [class com.usaa.ect.entities.OmGroupDefinition] does not define an IdClass



@Entity
@Table(schema = "OM_AUTHORING", name = "OM_GROUP_DEFINITION")
@Data
@NoArgsConstructor
@AllArgsConstructor

public class OmGroupDefinition implements Serializable {

    @Id
    @Column(columnDefinition = "VARCHAR2(30)",name = "VARIABLE_REPOSITORY_NM")
    private String variableRepositoryNm;

    @Id
    @Column(columnDefinition = "VARCHAR2(50)",name = "GROUP_NM")
    private String groupNm;

    @Column(columnDefinition = "VARCHAR2(50)",name = "PARENT_GROUP_NM")
    private String parentGroupNm;

    @Column(columnDefinition = "NUMBER(3)",name = "MINIMUM_OCCUR_QTY")
    private Integer minimumOccurQty;
}
==============================

import java.util.List;
@Repository
@Transactional


public interface OmGroupDefinitionRepository extends JpaRepository<OmGroupDefinition, Long> {

    @Query("SELECT gd FROM OM_AUTHORING.OM_GROUP_DEFINITION gd " +
            "JOIN OM_AUTHORING.OM_GROUP_TEMPLATE_RLTN rt ON gd.groupNm = rt.groupNm " +
            "AND gd.variableRepositoryNm = rt.variableRepositoryNm")
    List<OmGroupDefinition> findAllWithTemplateRltn();


}
       

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
