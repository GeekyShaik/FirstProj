VariableDefinitionsObject
Search by: name and version associations - join on group name, var name, var repo
from tables: OM_VARIABLE_DEFINITION, OM_VARIABLE_TEMPLATE_RLTN


pull all OM_VARIABLE_DEFINITION first
then all from OM_VARIABLE_TEMPLATE_RLTN (name and version) join with OM_VARIABLE_DEFINITION

list all uniques, remove dupes
master list of this join

    "variableRepositoryName": add to om var temp rrltn and om group templ rlt


    "groupName":
    "variableName":
    "commentTxt":
    "variableLabelTxt":
    "datatypeCd":
    "editRuleTypeCD":
    "editRuleValueTxt":
    "minimumVariableLengthQty": Integer
    "maximumVariableLengthQty": Integer
    "scalePositionQty":
    "defaultValueTxt":
    "sampleDefaultValueTxt":
    "uiWidgetCd":
    "editRuleCaseSensitiveInd":
    "upperCaseInd":
    "widgetPlacementCd":
    "afterFieldLabelTxt":
    "logicalDeleteInd":
    "lastUpdatePartyTdId":
    "lastUpdatePartyIdTypeCd":
    "lastUpdateGmts":
