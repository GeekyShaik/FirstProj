import org.junit.jupiter.api.BeforeEach;
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

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testFindVariableById_VariableExists() {
        // Arrange
        long variableId = 1L;
        Variable expectedVariable = new Variable(variableId, "Some Value");
        when(variableRepository.findById(variableId)).thenReturn(Optional.of(expectedVariable));

        // Act
        Optional<Variable> result = variableService.getVariableById(variableId);

        // Assert
        assertEquals(Optional.of(expectedVariable), result);
    }

    @Test
    void testFindVariableById_VariableNotFound() {
        // Arrange
        long variableId = 2L; // Assuming this ID is not present in the repository
        when(variableRepository.findById(variableId)).thenReturn(Optional.empty());

        // Act
        Optional<Variable> result = variableService.getVariableById(variableId);

        // Assert
        assertEquals(Optional.empty(), result);
    }
}