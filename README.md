
I have provided Table, entity class and VarianceIdGenerator class please create the controller, service and a Repository classes
please go through the requirement line by line understand correctly and implement


Acceptance criteria : Create an endpoint[putVariableController] to validate and persist new variable

Requirement
============

When authors hit the Save button in the Create Variable web page in React, the new variable details needs to be validated and persisted in the database.

1)Basically We need to retrive "variableVarianceId", By writing a Search query with query parameters as variableName and variable definitions [i.evariableLabelName, minLengthVrblWordQty, maxLengthVrblWordQty,dataTypeCd and uiWidget].

2)If all the variable definitions are matched, no need to create the variance ID.

3) variableVarianceId needs to generate dynamically, from teh class which i have provided i.e "VarianceIdGenerator"

4)If variable variance id exists, then return the variable variance id as reference to the author else persist the new variable.
===================================================================================================================================

desc OM_AUTHORING.vrbl_defn;

Name                     Null?    Type          
------------------------ -------- ------------- 
VRBL_DEFN_ID             NOT NULL NUMBER(7)     
VRBL_NM                  NOT NULL VARCHAR2(50)  
VRBL_VARY_ID             NOT NULL VARCHAR2(3)   
VRBL_STAT_CD             NOT NULL VARCHAR2(20)  
USE_TYPE_CD              NOT NULL VARCHAR2(1)   
CMNT_TXT                          VARCHAR2(250) 
VRBL_LABEL_NM                     VARCHAR2(50)  
DATA_TYPE_CD                      VARCHAR2(2)   
EDIT_RULE_VALUE_TXT               VARCHAR2(500) 
EDIT_RULE_TYPE_CD                 VARCHAR2(2)   
MIN_VRBL_LGTH_WORD_QTY            NUMBER(3)     
MAX_VRBL_LGTH_WORD_QTY            NUMBER(5)     
SCALE_PSTN_QTY                    VARCHAR2(1)   
DFLT_VALUE_TXT                    VARCHAR2(50)  
UI_WDGT_CD                        VARCHAR2(2)   
EDIT_RULE_CASE_SENSE_IND NOT NULL VARCHAR2(1)   
UPPER_CASE_IND           NOT NULL VARCHAR2(1)   
WDGT_PLACE_CD                     VARCHAR2(1)   
AFTER_FLD_LABEL_TXT               VARCHAR2(50)  
DEL_REC_IND              NOT NULL VARCHAR2(1)   
CREA_TS                  NOT NULL DATE          
CREA_PRTY_ID             NOT NULL VARCHAR2(36)  
CREA_PRTY_ID_TYPE_CD     NOT NULL VARCHAR2(4)   
UPDT_TS                           DATE          
UPDT_PRTY_ID                      VARCHAR2(36)  
UPDT_PRTY_ID_TYPE_CD              VARCHAR2(4)   
DML_TIMESTAMP            NOT NULL TIMESTAMP(6)  
===============================================================================


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
============================================================================================


@Component
@EnableAutoConfiguration
public class VarianceIdGenerator {

    /*
     *   This method is used to create new variance id for the group and
     *   variables that are created or modified
     */
    String getNewVarianceId(String varianceId) {
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


