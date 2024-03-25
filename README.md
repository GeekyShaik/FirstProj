import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import java.util.Optional;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

public class VariableServiceTest {

    @Mock
    private VariableRepository variableRepository;

    @InjectMocks
    private VariableService variableService;

    @Test
    void testFindVariableById() {
        // Arrange
        MockitoAnnotations.openMocks(this); // Initialize mocks
        
        String variableId = "A01";
        Variable expectedVariable = new Variable(variableId, "Some Value");
        when(variableRepository.findById(variableId)).thenReturn(Optional.of(expectedVariable));
        
        // Act
        Optional<Variable> result = variableService.findVariableById(variableId);
        
        // Assert
        assertEquals(Optional.of(expectedVariable), result);
    }
    
    @Test
    void testFindVariableById_NotFound() {
        // Arrange
        MockitoAnnotations.openMocks(this); // Initialize mocks
        
        String variableId = "B02"; // Assuming this ID is not present in the repository
        when(variableRepository.findById(variableId)).thenReturn(Optional.empty());
        
        // Act
        Optional<Variable> result = variableService.findVariableById(variableId);
        
        // Assert
        assertEquals(Optional.empty(), result);
    }
}