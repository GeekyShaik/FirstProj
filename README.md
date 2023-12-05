OmGroupDefinitionId Composite Keyimport java.io.Serializable;
import java.util.Objects;

public class OmGroupDefinitionId implements Serializable {
    private String variableRepositoryNm;
    private String groupNm;

    // Default constructor
    public OmGroupDefinitionId() {
    }

    // Constructor with fields
    public OmGroupDefinitionId(String variableRepositoryNm, String groupNm) {
        this.variableRepositoryNm = variableRepositoryNm;
        this.groupNm = groupNm;
    }

    // Getters and Setters
    // ...

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof OmGroupDefinitionId)) return false;
        OmGroupDefinitionId that = (OmGroupDefinitionId) o;
        return Objects.equals(variableRepositoryNm, that.variableRepositoryNm) &&
                Objects.equals(groupNm, that.groupNm);
    }

    @Override
    public int hashCode() {
        return Objects.hash(variableRepositoryNm, groupNm);
    }
}OmGroupTemplateRltnId Composite Keyimport java.io.Serializable;
import java.util.Objects;

public class OmGroupTemplateRltnId implements Serializable {
    private String omTemplateNm;
    private Integer omTemplateVersionNr;
    private String groupNm;

    // Default constructor
    public OmGroupTemplateRltnId() {
    }

    // Constructor with fields
    public OmGroupTemplateRltnId(String omTemplateNm, Integer omTemplateVersionNr, String groupNm) {
        this.omTemplateNm = omTemplateNm;
        this.omTemplateVersionNr = omTemplateVersionNr;
        this.groupNm = groupNm;
    }

    // Getters and Setters
    // ...

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof OmGroupTemplateRltnId)) return false;
        OmGroupTemplateRltnId that = (OmGroupTemplateRltnId) o;
        return Objects.equals(omTemplateNm, that.omTemplateNm) &&
                Objects.equals(omTemplateVersionNr, that.omTemplateVersionNr) &&
                Objects.equals(groupNm, that.groupNm);
    }

    @Override
    public int hashCode() {
        return Objects.hash(omTemplateNm, omTemplateVersionNr, groupNm);
    }
}