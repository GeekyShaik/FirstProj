package com.usaa.metadata.create.apis.controllers;

import com.usaa.metadata.create.apis.AuthoringMetadataIdObject;
//import com.usaa.metadata.create.apis.AuthoringMetadataObject;
import com.usaa.metadata.create.apis.MetadataConstants;
import com.usaa.metadata.create.apis.MetadataMigrationRequest;
import com.usaa.metadata.create.services.CreateService;
import com.usaa.metadata.create.services.PersistService;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.annotation.security.RolesAllowed;
import jakarta.validation.constraints.Max;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.Size;
import lombok.AllArgsConstructor;
import lombok.NonNull;
import lombok.extern.slf4j.Slf4j;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.math.BigDecimal;

@RestController
@RequestMapping(MetadataConstants.METADATA_CREATE_API_PATH)
@AllArgsConstructor
@Slf4j
@Tag(name = "Metadata Create Controller", description = "Set of REST Endpoints to create and persist Template Runtime Metadata based on OMNI data from the Template Library")
public class MetadataCreateController{

    private static final Logger LOGGER = LoggerFactory.getLogger(MetadataCreateController.class);

    private final CreateService createService;
    private final PersistService metadataPersistService;
    private final AuthoringMetadataIdObject authoringMetadataObject;

    @RolesAllowed("ROLE_EXTERNAL_LIBRARY")
    @Operation(
            summary = "Create Migration Runtime Object",
            description = "Search Template Library for template metadata from the OM_AUTHORING schema by templateName and templateVersion")
    @ApiResponses(
            value = {
                    @ApiResponse(responseCode = "200", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "400", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "401", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "403", content = {@Content(schema = @Schema())}),
                    @ApiResponse(responseCode = "500", content = {@Content(schema = @Schema())})
            })
    @ResponseBody
    @GetMapping(MetadataConstants.METADATA_CREATE_API_PATH)
    public ResponseEntity<String> processMigrationRequest (
            @RequestParam("objectName") @NonNull @Size(max = 50) String objectName,
            @RequestParam("objectVersion") @NonNull @Min(value = 0) @Max(value = 9999) BigDecimal objectVersion,
            @RequestParam("mdataObjTypeCd") @NonNull @Size(max = 20) String mdataObjTypeCd,
            @RequestParam("envrName") @NonNull @Size(max = 4) String envrName,
            @RequestParam("creaPrtyId") @NonNull @Size(max = 36) String creaPrtyId
    )
    throws Exception {

        MetadataMigrationRequest metadataMigrationRequest = new MetadataMigrationRequest();
        metadataMigrationRequest.setObjectName(objectName);
        metadataMigrationRequest.setObjectVersion(objectVersion);
        metadataMigrationRequest.setMdataObjTypeCd(mdataObjTypeCd);
        metadataMigrationRequest.setEnvrName(envrName);
        metadataMigrationRequest.setCreaPrtyId(creaPrtyId);

        LOGGER.trace("processMigrationRequest received metadataMigrationRequest: {}",
                metadataMigrationRequest.toString());

        HttpStatus httpStatus = createService.retrieveTemplateObjectDetails(metadataMigrationRequest);

//            LOGGER.error("MetadataCreateController.processMigrationRequest : {}", e.getMessage());
//            return HttpStatus.INTERNAL_SERVER_ERROR;

        return new ResponseEntity<>(httpStatus);
    }
}
