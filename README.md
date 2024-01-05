import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

import org.mockito.junit.jupiter.MockitoExtension;
import java.util.ArrayList;
import java.util.List;

@ExtendWith(MockitoExtension.class)
public class TemplateProfileServiceTest {

    @Mock
    private OmMessageTemplateRepository mockTemplateRepository;

    @Mock
    private OmMsgTemplateChannelRepository mockChannelRepository;

    @InjectMocks
    private TemplateProfileService serviceUnderTest;

    @BeforeEach
    public void setUp() {
        // With MockitoExtension, the initialization of mocks and injectMocks is taken care of.
    }

    @Test
    public void whenValidNameAndVersion_thenTemplateShouldBeFound() {
        // Setup
        String templateName = "ValidName";
        Integer templateVersion = 1;
        when(mockTemplateRepository.findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(templateName, templateVersion)).thenReturn(new OmMsgTemplateEntity());
        when(mockChannelRepository.searchAllByTemplateNameAndTemplateVersion(templateName, templateVersion)).thenReturn(new ArrayList<>());

        // Execute
        List<TemplateProfileDto> result = serviceUnderTest.getTemplatesByNameAndVersion(templateName, templateVersion);

        // Assert
        assertNotNull(result);
        assertFalse(result.isEmpty());
    }

    @Test
    public void whenInvalidNameAndVersion_thenNoTemplateShouldBeFound() {
        // Setup
        String templateName = "InvalidName";
        Integer templateVersion = 99;
        when(mockTemplateRepository.findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(templateName, templateVersion)).thenReturn(null);

        // Execute
        List<TemplateProfileDto> result = serviceUnderTest.getTemplatesByNameAndVersion(templateName, templateVersion);

        // Assert
        assertNotNull(result);
        assertTrue(result.isEmpty());
    }

    @Test
    public void whenRepositoryThrowsException_thenShouldPropagate() {
        // Setup
        String templateName = "ValidName";
        Integer templateVersion = 1;
        when(mockTemplateRepository.findOmMsgTemplateEntityByOmMsgTemplateEntityPK_TemplateNameAndOmMsgTemplateEntityPK_TemplateVersion(anyString(), anyInt())).thenThrow(new RuntimeException());

        // Assert Throws
        Exception exception = assertThrows(RuntimeException.class, () -> {
            serviceUnderTest.getTemplatesByNameAndVersion(templateName, templateVersion);
        });

        // Optional: if you want to further assert the exception message or type
        assertNotNull(exception);
    }

    // Additional test cases...
}