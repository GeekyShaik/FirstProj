import javax.persistence.*;
import java.io.Serializable;
import java.sql.Timestamp;

@Entity
@IdClass(OmGroupDefinitionId.class)
@Table(name = "OM_GROUP_DEFINITION")
public class OmGroupDefinition implements Serializable {

    @Id
    @Column(name = "VARIABLE_REPOSITORY_NM")
    private String variableRepositoryNm;

    @Id
    @Column(name = "GROUP_NM")
    private String groupNm;

    @Column(name = "PARENT_GROUP_NM")
    private String parentGroupNm;

    @Column(name = "MINIMUM_OCCUR_QTY")
    private Integer minimumOccurQty;

    @Column(name = "MAXIMUM_OCCUR_QTY")
    private Integer maximumOccurQty;

    @Column(name = "GROUP_COMMENT_TXT")
    private String groupCommentTxt;

    @Column(name = "LOGICAL_DELETE_IND", length = 1)
    private String logicalDeleteInd;

    @Column(name = "LAST_UPDATE_PARTY_ID")
    private String lastUpdatePartyId;

    @Column(name = "LAST_UPDATE_PARTY_ID_TYPE_CD")
    private String lastUpdatePartyIdTypeCd;

    @Column(name = "LAST_UPDATE_GMTS")
    private Timestamp lastUpdateGmts;

    @Column(name = "GROUP_LABEL_TXT")
    private String groupLabelTxt;

    @Column(name = "DML_TIMESTAMP")
    private Timestamp dmlTimestamp;

    // Getters and setters for all fields
}

public class OmGroupDefinitionId implements Serializable {
    private String variableRepositoryNm;
    private String groupNm;

    // Getters, setters, equals, and hashCode methods
}OmGroupTemplateRltn Entity and Composite Keyimport javax.persistence.*;
import java.io.Serializable;
import java.sql.Timestamp;

@Entity
@IdClass(OmGroupTemplateRltnId.class)
@Table(name = "OM_GROUP_TEMPLATE_RLTN")
public class OmGroupTemplateRltn implements Serializable {

    @Id
    @Column(name = "OM_TEMPLATE_NM")
    private String omTemplateNm;

    @Id
    @Column(name = "OM_TEMPLATE_VERSION_NR")
    private Integer omTemplateVersionNr;

    @Id
    @Column(name = "GROUP_NM")
    private String groupNm;

    @Column(name = "MINIMUM_OCCUR_QTY")
    private Integer minimumOccurQty;

    @Column(name = "MAXIMUM_OCCUR_QTY")
    private Integer maximumOccurQty;

    @Column(name = "OVERRIDE_LABEL_TXT")
    private String overrideLabelTxt;

    @Column(name = "OVERRIDE_COMMENT_TXT")
    private String overrideCommentTxt;

    @Column(name = "LOGICAL_DELETE_IND", length = 1)
    private String logicalDeleteInd;

    @Column(name = "GROUP_LOCATION_CD")
    private String groupLocationCd;

    @Column(name = "GROUP_SEQUENCE_NR")
    private Integer groupSequenceNr;

    @Column(name = "DML_TIMESTAMP")
    private Timestamp dmlTimestamp;

    @Column(name = "VARIABLE_REPOSITORY_NM")
    private String variableRepositoryNm;

    // Getters and setters for all fields
}

public class OmGroupTemplateRltnId implements Serializable {
    private String omTemplateNm;
    private Integer omTemplateVersionNr;
    private String groupNm;

    // Getters, setters, equals, and hashCode methods
}