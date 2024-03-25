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
    public void testCreateVariable_WithValidInput_Success() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");

        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");
        when(variableRepository.save(any())).thenReturn(variableDefinition);

        ResponseEntity<String> response = variableService.createVariable(variableDefinition);

        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("New variable persisted with variance ID: A02", response.getBody());
    }

    @Test
    public void testCreateVariable_WithExistingVarianceId_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");
        VariableDefinitionEntity existingVariable = new VariableDefinitionEntity();

        when(variableRepository.findByVariableVarianceId(any())).thenReturn(existingVariable);

        ResponseEntity<String> response = variableService.createVariable(variableDefinition);

        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("Variable already exists with variance ID: A01", response.getBody());
    }

    @Test
    public void testCreateVariable_WithInvalidInput_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();

        ResponseEntity<String> response = variableService.createVariable(variableDefinition);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("Invalid input", response.getBody());
    }

    @Test
    public void testCreateVariable_WithNullVarianceId_Success() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();

        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A01");
        when(variableRepository.save(any())).thenReturn(variableDefinition);

        ResponseEntity<String> response = variableService.createVariable(variableDefinition);

        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("New variable persisted with variance ID: A01", response.getBody());
    }

    @Test
    public void testCreateVariable_WithSaveFailure_Failure() {
        VariableDefinitionEntity variableDefinition = new VariableDefinitionEntity();
        variableDefinition.setVariableVarianceId("A01");

        when(variableRepository.findByVariableVarianceId(any())).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");
        when(variableRepository.save(any())).thenThrow(new RuntimeException("Failed to save"));

        ResponseEntity<String> response = variableService.createVariable(variableDefinition);

        assertEquals(HttpStatus.INTERNAL_SERVER_ERROR, response.getStatusCode());
        assertEquals("Failed to save", response.getBody());
    }
}
