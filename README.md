
        @Repository
        @Transactional
        public interface OmMessageTemplateRepository
            extends JpaRepository<OmMsgTemplateEntity,Long> {

        @Query("SELECT t FROM TemplateProfileDto t WHERE t.templateName = :templateName" +
                " AND t.templateVersion = :templateVersion")
        //returns a list of  TemplateProfileDto
                List<TemplateProfileDto> findByTemplateNameAndVersion(String templateName, String templateVersionNr);



      }

===========================================================================================

import jakarta.persistence.*;
import jakarta.validation.constraints.NotNull;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;
import org.springframework.format.annotation.NumberFormat;

import java.io.Serializable;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(schema = "OM_AUTHORING", name = "OM_MESSAGE_TEMPLATE")
@Data
@NoArgsConstructor

public class OmMsgTemplateEntity implements Serializable {

    private static final long serialVersionUID = 0L;


    @NotNull
    @Column(columnDefinition = "VARCHAR2(50)", name = "OM_TEMPLATE_NM")
    private String objectName;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(50)", name = "OM_TEMPLATE_VERSION_NR")
    private String objectVersion;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(50)", name = "VARIABLE_SCHEMA_NM")
    private String variableSchemaNm;

    @Column(columnDefinition = "VARCHAR2(30)", name = "GOGET_COLLECTION_NM")
    private String goGetCollectionNm;

    @Column(columnDefinition = "DATE", name = "MSG_TEMPL_VERSION_EFFECTIVE_DT")
    private LocalDate msgTemplVersionEffectiveDt;

    @Column(columnDefinition = "VARCHAR2(120)", name = "DISPLAY_NM")
    private String displayNm;

    @Column(columnDefinition = "VARCHAR2(200)", name = "DISPLAY_COMMENT_TXT")
    private String displayCommentTxt;

    @Column(columnDefinition = "CHAR(2)", name = "OPS_PROCEDURE_AREA_AC")
    private String opsProcedureAreaAc;

    @Column(columnDefinition = "CHAR(3)", name = "OPS_PROCEDURE_CATEGORY_AC")
    private String opsProcedureCategoryAc;

    @Column(columnDefinition = "CHAR(4)", name = "OPS_PROCEDURE_SUBCATEGORY_AC")
    private String opsProcedureSubcategoryAc;

    @Column(columnDefinition = "VARCHAR2(20)", name = "CCS_DOCUMENT_NM")
    private String ccsDocumentNm;

    @Column(columnDefinition = "VARCHAR2(50)", name = "WCM_PREVIEW_ID")
    private String wcmPreviewId;

    @Column(columnDefinition = "CHAR(1)", name = "MESSAGE_TEMPLATE_TYPE_CD")
    private String messageTemplateTypeCd;

    @Column(columnDefinition = "VARCHAR2(15)", name = "SOURCE_TOOL_DC")
    private String sourceToolDc;

    @Column(columnDefinition = "VARCHAR2(30)", name = "SOURCE_SYSTEM_DOCUMENT_ID")
    private String sourceSystemDocumentId;

    @Column(columnDefinition = "VARCHAR2(255)", name = "SSDID_OVERFLOW_TXT")
    private String ssdidOverflowTxt;

    @Column(columnDefinition = "CHAR(1)", name = "COMPOSITION_SEARCHABLE_IND")
    private String compositionSearchableInd;

    @Column(columnDefinition = "CHAR(1)", name = "MSR_CREATE_IND")
    private String msrCreateInd;

    @Column(columnDefinition = "CHAR(1)", name = "MSR_ACCESS_CD")
    private String msrAccessCd;

    @Column(columnDefinition = "CHAR(1)", name = "ALLOW_DIEE_IND")
    private String allowDieeInd;

    @Column(columnDefinition = "VARCHAR2(10)", name = "CHANNEL_DEFAULT_CD")
    private String channelDefaultCd;

    @Column(columnDefinition = "VARCHAR2(20)", name = "CHANNEL_OPTION_DEFAULT_CD")
    private String channelOptionDefaultCd;

    @Column(columnDefinition = "CHAR(1)", name = "ENCLOSURE_TYPE_CD")
    private String enclosureTypeCd;

    @Column(columnDefinition = "CHAR(1)", name = "ALLOW_CHANNEL_SELECTION_IND")
    private String allowChannelSelectionInd;

    @Column(columnDefinition = "VARCHAR2(50)", name = "ENTERPRISE_DOCUMENT_ID")
    @NumberFormat
    private String enterpriseDocumentId;

    @Column(columnDefinition = "CHAR(1)", name = "ALLOW_CRTSY_COPIES_IND")
    private String allowCrtsyCopiesInd;

    @Column(columnDefinition = "CHAR(1)", name = "ALLOW_MULT_PARTY_IND")
    private String allowMultPartyInd;

    @Column(columnDefinition = "VARCHAR2(15)", name = "ARCHIVE_PACKAGE_DOC_FORM_CD")
    private String archivePackageDocFormCd;

    @Column(columnDefinition = "CHAR(1)", name = "CONT_NR_ACROSS_ENCL_IND")
    private String contNrAcrossEnclInd;

    @Column(columnDefinition = "CHAR(1)", name = "MAAC_MSR_INTEGRATION_IND")
    private String maacMsrIntegrationInd;

    @Column(columnDefinition = "VARCHAR2(50)", name = "MAAC_BPH_DESCRIPTION_TXT")
    private String maacBphDescriptionTxt;

    @Column(columnDefinition = "CHAR(1)", name = "ALLOW_FREE_TEXT_CD")
    private String allowFreeTextCd;

    @Column(columnDefinition = "CHAR(1)", name = "MANDATORY_REVIEW_IND")
    private String mandatoryReviewInd;

    @Column(columnDefinition = "CHAR(1)", name = "LOGICAL_DELETE_IND")
    private String logicalDeleteInd;

    @Column(columnDefinition = "CHAR(1)", name = "AUTOMATICALLY_GET_CAPS_CD")
    private String automaticallyGetCapsCd;

    @Column(columnDefinition = "CHAR(1)", name = "ORIG_RECIPIENT_DETER_CD")
    private String origRecipientDeterCd;

    @Column(columnDefinition = "VARCHAR2(50)", name = "ALT_RCPT_GOGET_LABEL_TXT")
    private String altRcptGogetLabelTxt;

    @Column(columnDefinition = "CHAR(1)", name = "RECIPIENT_SELECTION_BYPASS_IND")
    private String recipientSelectionBypassInd;

    @Column(columnDefinition = "CHAR(1)", name = "CHANNEL_SELECTION_BYPASS_IND")
    private String channelSelectionBypassInd;

    @Column(columnDefinition = "CHAR(1)", name = "MASTER_VARIABLE_BYPASS_IND")
    private String masterVariableBypassInd;

    @Column(columnDefinition = "CHAR(1)", name = "SECTION_SELECTION_BYPASS_IND")
    private String sectionSelectionBypassInd;

    @Column(columnDefinition = "CHAR(1)", name = "SUB_VARIABLE_BYPASS_IND")
    private String subVariableBypassInd;

    @NotNull
    @Column(columnDefinition = "CHAR(9)", name = "LAST_UPDATE_PARTY_TD_ID")
    private String lastUpdatePartyTdId;

    @NotNull
    @Column(columnDefinition = "CHAR(4)", name = "LAST_UPDATE_PARTY_TYPE_CD")
    private String lastUpdatePartyTypeCd;

    @Column(columnDefinition = "TIMESTAMP(6)", name = "LAST_UPDATE_GMTS")
    private LocalDateTime lastUpdateGmts;

    @Column(columnDefinition = "CHAR(4)", name = "CUR_ENVIRONMENT_CD")
    private String curEnvironmentCd;

    @Column(columnDefinition = "VARCHAR2(200)", name = "SEARCH_KEYWORDS_TXT")
    private String searchKeywordsTxt;

    @Column(columnDefinition = "CHAR(1)", name = "ENCLOSURE_SELECTION_BYPASS_IND")
    private String enclosureSelectionBypassInd;

    @Column(columnDefinition = "VARCHAR2(500)", name = "TEMPLATE_RST_ACT_DIR_TXT")
    private String templateRstActDirTxt;

    @Column(columnDefinition = "CHAR(1)", name = "PAGE_NUM_TYPE_CD")
    private String pageNumTypeCd;

    @Column(columnDefinition = "CHAR(1)", name = "CONT_NUM_ACRS_MASTER_IND")
    private String contNumAcrsMasterInd;

    @Column(columnDefinition = "CHAR(1)", name = "CONT_NUM_ACRS_CCCOVER_IND")
    private String contNumAcrsCccoverInd;

    @Column(columnDefinition = "VARCHAR2(6)", name = "PAGE_NUM_HORZ_ALIGN_CD")
    private String pageNumHorzAlignCd;

    @Column(columnDefinition = "VARCHAR2(6)", name = "PAGE_NUM_VERT_ALIGN_CD")
    private String pageNumVertAlignCd;

    @Column(columnDefinition = "VARCHAR2(1)", name = "PAGE_NUM_SHOW_COUNT_IND")
    private String pageNumShowCountInd;

    @Column(columnDefinition = "VARCHAR2(4)", name = "PAGE_NUM_DISPLAY_RANGE_CD")
    private String pageNumDisplayRangeCd;

    @Column(columnDefinition = "VARCHAR2(6)", name = "PAGE_NUM_COUNT_RANGE_CD")
    private String pageNumCountRangeCd;

    @Column(columnDefinition = "VARCHAR2(500)", name = "TEMPLATE_LIMIT_FUN_ACT_DIR_TXT")
    private String templateLimitFunActDirTxt;

    @Column(columnDefinition = "CHAR(4)", name = "CONT_ENVIRONMENT_CD")
    private String contEnvironmentCd;

    @Column(columnDefinition = "VARCHAR2(12)", name = "COMM_PURPOSE_CD")
    private String commPurposeCd;

    @Column(columnDefinition = "VARCHAR2(1)", name = "ESIGNATURE_ENABLED_IND")
    private String esignatureEnabledInd;

    @Column(columnDefinition = "VARCHAR2(25)", name = "INBOUND_LEGAL_FORM_CD")
    private String inboundLegalFormCd;

    @Column(columnDefinition = "VARCHAR2(25)", name = "INBOUND_FLATTEN_FORM_CD")
    private String inboundFlattenFormCd;

    @Column(columnDefinition = "VARCHAR2(40)", name = "ESIGN_CONFIG_TYPE_DC")
    private String esignConfigTypeDc;

    @Column(columnDefinition = "VARCHAR2(40)", name = "ESIGN_ACTIVITY_TYPE_DC")
    private String esignActivityTypeDc;

    @Column(columnDefinition = "VARCHAR2(1)", name = "REGULATORY_CLASS_CD")
    private String regulatoryClassCd;

    @Column(columnDefinition = "DATE", name = "MSG_CONTENT_EFFECTIVE_DT")
    private LocalDate msgContentEffectiveDt;

    @Column(columnDefinition = "VARCHAR2(1)", name = "CSY_COP_REGULATORY_CLASS_CD")
    private String csyCopRegulatoryClassCd;

    @Column(columnDefinition = "VARCHAR2(1)", name = "NEW_PAGE_IND")
    private String newPageInd;

    @Column(columnDefinition = "VARCHAR2(15)", name = "INBOUND_PACKAGE_DOC_FORM_CD")
    private String inboundPackageDocFormCd;

    @Column(columnDefinition = "VARCHAR2(15)", name = "TEMPLATE_VERSION_STATUS_CD")
    private String templateVersionStatus;

    @NotNull
    @Column(columnDefinition = "VARCHAR2(18)", name = "OM_TEMPLATE_REQUEST_ID")
    private String omTemplateRequestId;

    @Column(columnDefinition = "DATE", name = "LEGACY_REVISION_DT")
    private LocalDate legacyRevisionDt;

   ===============================================================

   import com.fasterxml.jackson.annotation.JsonProperty;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDate;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class TemplateProfileDto {


    @JsonProperty("templateName")
        private String templateName;
    @JsonProperty("effectiveDate")
        private LocalDate effectiveDate;
    @JsonProperty("templateVersion")
        private String templateVersion;
    @JsonProperty("enterpriseDocId")
        private String enterpriseDocId;
    @JsonProperty("variableSchemaName")
        private String variableSchemaName;
    @JsonProperty("busArea")
        private String busArea;
    @JsonProperty("busCategory")
        private String busCategory;

    @JsonProperty("busSubCategory")
    private String busSubCategory;

    @JsonProperty("messageTemplateTypeCd")
        private String messageTemplateTypeCd;
    @JsonProperty("displayName")
        private String displayName;
    @JsonProperty("regulatoryClassification")
        private String regulatoryClassification;
    @JsonProperty("sourceSystemDocumentId")
        private String sourceSystemDocumentId;
    @JsonProperty("sourceTool")
        private String sourceTool;
    @JsonProperty("goGetCollectionName")
        private String goGetCollectionName;

    @JsonProperty("pageNumberHorizAlignCd")

        private String pageNumberHorizAlignCd;
    @JsonProperty("pageNumberTypeCd")
        private String pageNumberTypeCd;
    @JsonProperty("pageNumberVertAlignCd")
        private String pageNumberVertAlignCd;

    @JsonProperty("channelDefaultCode")
        private String channelDefaultCode;
    @JsonProperty("channelDefaultOptionCode")
        private String channelDefaultOptionCode;
    @JsonProperty("pageNumberDisplayRangeCd")
    private String pageNumberDisplayRangeCd;

    @JsonProperty("showPageCountInd")
    private String showPageCountInd;

    @JsonProperty("pageNumberCountRangeCd")
    private String pageNumberCountRangeCd;

    @JsonProperty("continousNumberingAcrossCCCover")
    private String continousNumberingAcrossCCCover;

    @JsonProperty("continousNumberingAcrossMaster")
    private String continousNumberingAcrossMaster;

    @JsonProperty("continousNumberingAcrossEnclosures")
    private String continousNumberingAcrossEnclosures;





}
==============================================

@RestController
@RequestMapping("/templates")
public class TemplateProfileController {
    @Autowired
    private TemplateProfileService templateService;

    @GetMapping("/retrieve")
    public ResponseEntity<List<TemplateProfileDto>> getTemplatesByNameAndVersion(
            @RequestParam String templateName,
            @RequestParam String templateVersion
    ) {
        List<TemplateProfileDto> templates = templateService.getTemplatesByNameAndVersion(templateName, templateVersion);

        if (templates.isEmpty()) {
            return ResponseEntity.notFound().build();
        } else {
            return ResponseEntity.ok(templates);
        }
    }
}
================================================================================================



@Service
public class TemplateProfileService {

        @Autowired
        private OmMessageTemplateRepository templateRepository;

        public List<TemplateProfileDto> getTemplatesByNameAndVersion(String templateName, String templateVersion) {
            return templateRepository.findByTemplateNameAndVersion(templateName, templateVersion);
        }
    }
    ========================================================================

   

@Configuration
public class ModelMapperConfig {

    @Bean
    public ModelMapper modelMapper() {
        ModelMapper mapper = new ModelMapper();
        PropertyMap<OmMsgTemplateEntity, TemplateProfileDto> orderMap = new PropertyMap<OmMsgTemplateEntity, TemplateProfileDto>() {
            protected void configure() {
                map().setTemplateName(source.getObjectName());
                map().setTemplateVersion(source.getObjectVersion());
                map().setVariableSchemaName(source.getVariableSchemaNm());
                 map().setGoGetCollectionName(source.getGoGetCollectionNm());
                map().setEffectiveDate(source.getMsgTemplVersionEffectiveDt());
                map().setDisplayName(source.getDisplayNm());
                 map().setBusArea(source.getOpsProcedureAreaAc());
                map().setBusCategory(source.getOpsProcedureCategoryAc());
                map().setBusSubCategory(source.getOpsProcedureSubcategoryAc());
                map().setChannelDefaultCode(source.getChannelDefaultCd());
                map().setEnterpriseDocId(source.getEnterpriseDocumentId());
                map().setSourceTool(source.getSourceToolDc());
                map().setMessageTemplateTypeCd(source.getMessageTemplateTypeCd());
                map().setRegulatoryClassification(source.getRegulatoryClassCd());
                map().setSourceSystemDocumentId(source.getSourceSystemDocumentId());
                map().setPageNumberHorizAlignCd(source.getPageNumHorzAlignCd());
                map().setPageNumberTypeCd(source.getPageNumTypeCd());
                map().setPageNumberVertAlignCd(source.getPageNumVertAlignCd());
                map().setPageNumberDisplayRangeCd(source.getPageNumDisplayRangeCd());
                map().setShowPageCountInd(source.getPageNumShowCountInd());
                map().setChannelDefaultCode(source.getChannelDefaultCd());
                map().setChannelDefaultOptionCode(source.getChannelOptionDefaultCd());
                map().setContinousNumberingAcrossCCCover(source.getContNumAcrsCccoverInd());
                map().setContinousNumberingAcrossMaster(source.getContNumAcrsMasterInd());
                map().setContinousNumberingAcrossEnclosures(source.getContNrAcrossEnclInd());

            }
        };

        mapper.addMappings(orderMap);
        return mapper;
    }


}




