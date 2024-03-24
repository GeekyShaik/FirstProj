import org.assertj.core.api.AssertionsForClassTypes;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import java.time.LocalDateTime;
import java.util.*;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;

@ExtendWith(SpringExtension.class)
@SpringBootTest
@EnableAutoConfiguration
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
        variableDefinition.setVariableStatusCd("ACTIVE");
        variableDefinition.setUseTypeCd("TYPE_A");
        variableDefinition.setCreatePartyId("user123");
        variableDefinition.setCreatePartyIdTypeCd("EMP");

        when(variableRepository.findByVariableVarianceId("A01")).thenReturn(null);
        when(varianceIdGenerator.getNewVarianceId(any())).thenReturn("A02");

        // Act
        String result = variableService.validateAndPersistVariable(variableDefinition);

        // Assert
        assertThat(result).isEqualTo("New variable persisted with variance ID: A02");
    }

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

        // Act
        String result = variableService.validateAndPersistVariable(variableDefinition);

        // Assert
        assertThat(result).isEqualTo("Variable already exists with variance ID: A01");
    }
}
