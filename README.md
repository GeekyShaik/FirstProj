
Description
===========

When authors hit the Save button in the Create Variable web page in React, the new variable details needs to be validated and persisted in the database. The Variable Variance ID needs to be created through v3-core api.  Use select query to retrieve the variable variance id with query parameters (variable name and variable definitions). If variable variance id exists, then return the variable variance id as reference to the author else persist the new variable.

Acceptance Criteria: Create an endpoint to validate and persist new variable

Requirement:
===========
please create an post or put controller, service and Repository class based on the above requirement. I will provide you 
entity class, VarianceIdGenerator and [retrievedVariableDefinitions controller for reference]


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
===============================================================
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
=====================================================================================
@ResponseBody
    @GetMapping(VARIABLE_DEFINITION)
    public ResponseEntity<List<OmVariableDefinitionDTO>> retrievedVariableDefinitions(
            @RequestParam("variableRepositoryNm") @NonNull @Size(max = 30) String variableRepositoryNm,
            @RequestParam("groupNm") @NonNull @Size(max = 50) String groupNm,
            @RequestParam(value = "variableNm", required = false) @Size(max = 50) String variableNm) {

        HttpStatus httpStatus = OK;

        Map<String, String> parameterMap = new HashMap<>();
        parameterMap.put("variableRepositoryNm", variableRepositoryNm);
        parameterMap.put("groupNm", groupNm);
        parameterMap.put("variableNm", variableNm);

        LOGGER.trace("retrievedVariableDefinitions received: parameterMap = {}",parameterMap);

        List<OmVariableDefinitionDTO> omVariableDefinitionDTOS =
                coreLibraryGETService.getVariableDefinitions(parameterMap);

        if (omVariableDefinitionDTOS.isEmpty()) {
            httpStatus = NO_CONTENT;
        }

        return new ResponseEntity<>(omVariableDefinitionDTOS, httpStatus);
    }


=========================================================================================

Tables: OM_AUTHORING.VRBL_DEFN
