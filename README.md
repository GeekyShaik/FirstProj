@Test
public void testValidateAndPersistVariable_ExistingVariable_Failure() {
    // Arrange
    VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
    variableDefinition.setVariableName("TestVariable");
    variableDefinition.setVariableVarianceId("A01");
    variableDefinition.setVariableStatusCd("ACTIVE");
    variableDefinition.setUseTypeCd("TYPE_A");
    variableDefinition.setCreatePartyId("user123");
    variableDefinition.setCreatePartyIdTypeCd("EMP");

    VariableDefinitionEntity existingVariable = new VariableDefinitionEntity();
    existingVariable.setVariableVarianceId("A01");

    when(variableRepository.findByVariableVarianceId("A01")).thenReturn(existingVariable);
    // Mock behavior to return null when generating new variance ID for existing variable
    when(varianceIdGenerator.getNewVarianceId(any())).thenReturn(null);

    // Act
    String result = variableService.validateAndPersistVariable(variableDefinition);

    // Assert
    assertThat(result).isEqualTo("Variable already exists with variance ID: A01");
}
