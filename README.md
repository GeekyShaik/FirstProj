
import org.springframework.stereotype.Service;

@Service
public class VariableService {

    private final VariableRepository variableRepository;
    private final VarianceIdGenerator varianceIdGenerator;

    public VariableService(VariableRepository variableRepository, VarianceIdGenerator varianceIdGenerator) {
        this.variableRepository = variableRepository;
        this.varianceIdGenerator = varianceIdGenerator;
    }

    public String validateAndPersistVariable(VariableDefinitionEntity variableDefinition) {
        // Perform validation logic here

        // Check if variable variance ID exists
        String variableVarianceId = variableDefinition.getVariableVarianceId();
        VariableDefinitionEntity existingVariable = variableRepository.findByVariableVarianceId(variableVarianceId);

        if (existingVariable != null) {
            // Variable variance ID exists, return reference
            return "Variable already exists with variance ID: " + variableVarianceId;
        } else {
            // Generate new variance ID
            String newVarianceId = varianceIdGenerator.getNewVarianceId(variableVarianceId);
            variableDefinition.setVariableVarianceId(newVarianceId);

            // Persist the new variable
            variableRepository.save(variableDefinition);

            return "New variable persisted with variance ID: " + newVarianceId;
        }
    }
}
===============================================================


import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

    @Repository
    public interface VariableRepository extends JpaRepository<VariableDefinitionEntity, Long> {
        VariableDefinitionEntity findByVariableVarianceId(String variableVarianceId);
    }

=========================================================


import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.stereotype.Component;

import java.util.Objects;

@Component
@EnableAutoConfiguration
public class VarianceIdGenerator {

    /*
     *   This method is used to create new variance id for the group and
     *   variables that are created or modified
     */
    public String getNewVarianceId(String varianceId) {
        String newVarianceId = "A01";
        if ( !Objects.isNull(varianceId)) {
            if (!varianceId.isEmpty()) {
                if (varianceId.length() == 3) {
                    char firstAlpha = varianceId.charAt(0);
                    char secondNumeric = varianceId.charAt(1);
                    char thirdNumeric = varianceId.charAt(2);
                    int nextNumeric = 0;
                    int nextAlpha = 0;
                    String concatNumerics = secondNumeric + String.valueOf(thirdNumeric);
                    nextNumeric = Integer.valueOf(concatNumerics);
                    nextNumeric++;
                    if (Integer.valueOf(String.valueOf(secondNumeric)) > 0) {
                        if (nextNumeric > 99) {
                            nextAlpha = firstAlpha; //ASCII value
                            nextAlpha++;
                            newVarianceId = (char)nextAlpha + "01";
                        } else {
                            newVarianceId = firstAlpha + String.valueOf(nextNumeric);
                        }
                    } else {
                        if (nextNumeric > 9) {
                            newVarianceId = firstAlpha + String.valueOf(nextNumeric);
                        } else {
                            newVarianceId = firstAlpha + String.valueOf(secondNumeric) + nextNumeric;
                        }
                    }
                }
            }
        }
        return newVarianceId;
    }
}
==========================================================



@RestController
@RequestMapping("/variables")
public class VariableController {

    private final VariableService variableService;

    public VariableController(VariableService variableService) {
        this.variableService = variableService;
    }

    @PostMapping("/create")
    public ResponseEntity<String> createVariable(@RequestBody VariableDefinitionEntity variableDefinition) {
        String result = variableService.validateAndPersistVariable(variableDefinition);
        return ResponseEntity.ok(result);
    }
}
======================================


import jakarta.persistence.*;
import jakarta.validation.constraints.NotNull;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;
import java.time.LocalDateTime;
import java.util.Set;

@Entity
@Table(schema = "OM_AUTHORING", name = "VRBL_DEFN", uniqueConstraints = {@UniqueConstraint(columnNames = {"VRBL_NM","VRBL_VARY_ID"})})
@Data
@NoArgsConstructor
@AllArgsConstructor
/**
 * This method handles all the variables, including the primary keys the service needs to get from
 * OM_AUTHORING.VRBL_DEFN table
 */
public class VariableDefinitionEntity implements Serializable {


    private static final long serialVersionUID = 0L;

    @EmbeddedId
    private VariableDefinitionEntityPK variableDefinitionEntityPK;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(50)", name = "VRBL_NM")
    private String variableName;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(3)", name = "VRBL_VARY_ID")
    private String variableVarianceId;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(20)", name = "VRBL_STAT_CD")
    private String variableStatusCd;

    @NotNull
    @Column(columnDefinition = "CHAR(1)", name = "USE_TYPE_CD")
    private String useTypeCd;

    @Column(columnDefinition = "VARCHAR2(250)", name = "CMNT_TXT")
    private String commentTxt;

    @Column(columnDefinition = "VARCHAR2(50)", name = "VRBL_LABEL_NM")
    private String variableLabelName;

    @Column(columnDefinition = "VARCHAR2(2)", name = "DATA_TYPE_CD")
    private String dataTypeCd;

    @Column(columnDefinition = "VARCHAR2(2)", name = "EDIT_RULE_TYPE_CD")
    private String editRuleTypeCd;

    @Column(columnDefinition = "VARCHAR2(500)", name = "EDIT_RULE_VALUE_TXT")
    private String editRuleValueTxt;

    @Column(columnDefinition = "NUMBER(3,0)", name = "MIN_VRBL_LGTH_WORD_QTY")
    private String minLengthVrblWordQty;

    @Column(columnDefinition = "NUMBER(5,0)", name = "MAX_VRBL_LGTH_WORD_QTY")
    private String maxLengthVrblWordQty;

    @Column(columnDefinition = "CHAR(1)", name = "SCALE_PSTN_QTY")
    private String scalePositionQty;

    @Column(columnDefinition = "VARCHAR2(50)", name = "DFLT_VALUE_TXT")
    private String defaultValueTxt;

    @Column(columnDefinition = "VARCHAR2(2)", name = "UI_WDGT_CD")
    private String uiWidget;

    @NotNull
    @Column(columnDefinition = "CHAR(1)", name = "EDIT_RULE_CASE_SENSE_IND")
    private String editRuleCaseSensitiveInd;

    @NotNull
    @Column(columnDefinition = "CHAR(1)", name = "UPPER_CASE_IND")
    private String upperCaseInd;

    @Column(columnDefinition = "CHAR(1)", name = "WDGT_PLACE_CD")
    private String widgetPlacementCd;

    @Column(columnDefinition = "VARCHAR2(50)", name = "AFTER_FIELD_LABEL_TXT")
    private String afterFieldLabelTxt;

    @NotNull
    @Column(columnDefinition = "CHAR(1)", name = "DEL_REC_IND")
    private String logicalDeleteInd;

    @NotNull
    @Column(columnDefinition = "CHAR(36)", name = "CREA_PRTY_ID")
    private String createPartyId;

    @NotNull
    @Column(columnDefinition = "CHAR(4)", name = "CREA_PRTY_ID_TYPE_CD")
    private String createPartyIdTypeCd;

    @NotNull
    @Column(columnDefinition = "TIMESTAMP(6)", name = "CREA_TS")
    private LocalDateTime createGmts;

    @Column(columnDefinition = "CHAR(36)", name = "UPDT_PRTY_ID")
    private String lastUpdatePartyTdId;

    @Column(columnDefinition = "CHAR(4)", name = "UPDT_PRTY_ID_TYPE_CD")
    private String lastUpdatePartyIdTypeCd;

    @Column(columnDefinition = "TIMESTAMP(6)", name = "UPDT_TS")
    private LocalDateTime lastUpdateGmts;

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "schemaVariableRelationList")
    private Set<SchemaGroupVariableRelationEntity> variableSchemaRelationList;

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "groupVariableRelationList")
    private Set<GroupVariableRelationEntity> variableGroupRelationList;

}
