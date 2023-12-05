import javax.persistence.*;
import java.sql.Timestamp;

@Entity
@Table(name = "OM_GROUP_DEFINITION")
public class GroupDefinition {

    @Id
    @Column(name = "GROUP_NM")
    private String groupName;

    // Other fields based on the database schema
    @Column(name = "PARENT_GROUP_NM")
    private String parentGroupName;

    @Column(name = "MINIMUM_OCCUR_QTY")
    private Integer minimumOccurQuantity;

    // ... include other fields here

    @Column(name = "DML_TIMESTAMP")
    private Timestamp dmlTimestamp;

    // Standard getters and setters
}

@Entity
@Table(name = "OM_GROUP_TEMPLATE_RLTN")
@IdClass(GroupTemplateId.class) // Composite key
public class GroupTemplateRltn {

    @Id
    @Column(name = "OM_TEMPLATE_NM")
    private String templateName;

    @Id
    @Column(name = "OM_TEMPLATE_VERSION_NR")
    private Integer templateVersionNumber;

    // Other fields based on the database schema
    @Column(name = "GROUP_NM")
    private String groupName;

    // ... include other fields here

    @Column(name = "DML_TIMESTAMP")
    private Timestamp dmlTimestamp;

    // Standard getters and setters
}

// This class is used for the composite key
public class GroupTemplateId implements Serializable {
    private String templateName;
    private Integer templateVersionNumber;

    // Standard getters and setters, equals(), and hashCode() methods
}
Now, for the Spring Data JPA repository class with a custom query, let's assume you want to find all GroupTemplateRltn entities joined with GroupDefinition based on the group name:

java
Copy code
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import java.util.List;

public interface GroupTemplateRltnRepository extends JpaRepository<GroupTemplateRltn, GroupTemplateId> {

    @Query("SELECT gt FROM GroupTemplateRltn gt JOIN GroupDefinition gd ON gt.groupName = gd.groupName")
    List<GroupTemplateRltn> findAllWithGroupDefinition();
}
