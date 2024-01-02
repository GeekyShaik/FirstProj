@Repository
public interface GroupDefinitionRepository extends JpaRepository<GroupDefinition, String> {
    // Assuming custom query is needed here as well
    @Query("SELECT gd FROM GroupDefinition gd JOIN GroupTemplateRltn gtr ON gd.groupNm = gtr.groupNm AND gd.variableRepositoryNm = gtr.variableRepositoryNm")
    List<GroupDefinition> findJoinedGroupDefinitions();
}
GroupTemplateRltnRepository.java
java
Copy code
@Repository
public interface GroupTemplateRltnRepository extends JpaRepository<GroupTemplateRltn, String> {
    @Query("SELECT gtr, gd FROM GroupTemplateRltn gtr JOIN GroupDefinition gd ON gtr.groupNm = gd.groupNm AND gtr.variableRepositoryNm = gd.variableRepositoryNm")
    List<Object[]> findGroupTemplateRltnsWithDefinitions();
}
