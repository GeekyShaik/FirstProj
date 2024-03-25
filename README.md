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
    public void testCreateVariable_WithNewVarianceId_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");
        
        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");
        when(variableRepository.save(any())).thenThrow(new RuntimeException("Failed to save"));

        String result = variableService.validateAndPersistVariable(variableDefinition);

        assertEquals("Failed to save", result);
    }

    @Test
    public void testCreateVariable_WithGeneratedVarianceId_Success() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId(null);
        
        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A01");
        when(variableRepository.save(any())).thenReturn(variableDefinition);

        String result = variableService.validateAndPersistVariable(variableDefinition);

        assertEquals("New variable persisted with variance ID: A01", result);
    }
}
