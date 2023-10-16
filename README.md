

import com.fasterxml.jackson.annotation.JsonProperty;
import lombok.Data;

import java.math.BigDecimal;
import java.time.LocalDate;

@Data
public class TemplateProfileDTO {

    @JsonProperty("objectName")
    private String objectName;

    @JsonProperty("effectiveDate")
    private LocalDate effectiveDate;

    @JsonProperty("objectVersion")
    private BigDecimal objectVersion;

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

    @JsonProperty("gogetCollectionName")
    private String gogetCollectionName;

    @JsonProperty("channelName")
    private String channelName;

    @JsonProperty("subChannelName")
    private String subChannelName;

    @JsonProperty("pageNumberHorizAlignCd")
    private String pageNumberHorizAlignCd;

    @JsonProperty("pageNumberTypeCd")
    private String pageNumberTypeCd;

    @JsonProperty("pageNumberVertAlignCd")
    private String pageNumberVertAlignCd;

    //    private String continousNumberingAcrossEnclosures; //--?

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

    @JsonProperty("channelDefaultCode")
    private String channelDefaultCode;

    @JsonProperty("channelDefaultOptionCode")
    private String channelDefaultOptionCode;
}
