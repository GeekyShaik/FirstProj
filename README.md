import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.*;

public class VariableServiceTest {

    @Mock
    private VariableRepository variableRepository;

    @Mock
    private VarianceIdGenerator varianceIdGenerator;

    @InjectMocks
    private VariableService variableService;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void testValidateAndPersistVariable_NewVariable_Success() {
        // Arrange
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableName("TestVariable");
        variableDefinition.setVariableVarianceId("A01");
        // Mock behavior to return null, indicating a new variance ID
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A01");
        // Mock behavior to return null, indicating the variable is not found
        when(variableRepository.findByVariableVarianceId("A01")).thenReturn(Optional.empty());

        // Act
        String result = variableService.validateAndPersistVariable(variableDefinition);

        // Assert
        assertEquals("New variable persisted with variance ID: A01", result);
        verify(variableRepository, times(1)).save(variableDefinition);
    }

    @Test
    public void testValidateAndPersistVariable_ExistingVariable_Failure() {
        // Arrange
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableName("TestVariable");
        variableDefinition.setVariableVarianceId("A01");
        // Mock behavior to return an existing variance ID
        when(variableRepository.findByVariableVarianceId("A01")).thenReturn(Optional.of(new VariableDefinitionEntity()));

        // Act
        String result = variableService.validateAndPersistVariable(variableDefinition);

        // Assert
        assertEquals("Variable already exists with variance ID: A01", result);
        verifyZeroInteractions(varianceIdGenerator);
        verifyZeroInteractions(variableRepository);
    }

    @Test
    public void testValidateAndPersistVariable_NewVarianceId_Success() {
        // Arrange
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableName("TestVariable");
        variableDefinition.setVariableVarianceId("A01");
        // Mock behavior to return null, indicating a new variance ID
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");
        // Mock behavior to return null, indicating the variable is not found
        when(variableRepository.findByVariableVarianceId("A01")).thenReturn(Optional.empty());

        // Act
        String result = variableService.validateAndPersistVariable(variableDefinition);

        // Assert
        assertEquals("New variable persisted with variance ID: A02", result);
        assertEquals("A02", variableDefinition.getVariableVarianceId());
        verify(variableRepository, times(1)).save(variableDefinition);
    }
}
