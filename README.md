import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;

public class VariablePostServiceTest {

    @Mock
    private VariableRepository variableRepository;

    @Mock
    private VarianceIdGenerator varianceIdGenerator;

    @InjectMocks
    private VariableService variableService;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    public void testCreateVariable_WithNewVarianceId_Success() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");
        
        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");
        when(variableRepository.save(any())).thenReturn(variableDefinition);

        String result = variableService.validateAndPersistVariable(variableDefinition);

        assertEquals("New variable persisted with variance ID: A02", result);
    }

    @Test
    public void testCreateVariable_WithExistingVarianceId_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");
        VariableDefinitionEntity existingVariable = new VariableDefinitionEntity();

        when(variableRepository.findByVariableVarianceId(any())).thenReturn(existingVariable);

        String result = variableService.validateAndPersistVariable(variableDefinition);

        assertEquals("Variable already exists with variance ID: A01", result);
    }

    @Test
    public void testCreateVariable_WithInvalidInput_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();

        String result = variableService.validateAndPersistVariable(variableDefinition);

        assertEquals("Invalid input", result);
    }
}