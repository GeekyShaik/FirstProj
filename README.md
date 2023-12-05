
import jakarta.persistence.*;
import java.io.Serializable;
import java.time.LocalDate;


import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Table(schema = "OM_AUTHORING", name = "OM_GROUP_DEFINITION")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class OmGroupDefinitionEntity implements Serializable {

    @Id
    @Column(columnDefinition = "VARCHAR2(50)",name = "GROUP_NM")
    private String groupNm;

    @Column(columnDefinition = "VARCHAR2(30)",name = "VARIABLE_REPOSITORY_NM")
    private String variableRepositoryNm;

    @Column(columnDefinition = "VARCHAR2(50)",name = "PARENT_GROUP_NM")
    private String parentGroupNm;

    @Column(columnDefinition = "NUMBER(3)",name = "MINIMUM_OCCUR_QTY")
    private Integer minimumOccurQty;

    @Column(columnDefinition = "NUMBER(3)",name = "MAXIMUM_OCCUR_QTY")
    private Integer maximumOccurQty;

    @Column(columnDefinition = "VARCHAR2(120)",name = "GROUP_COMMENT_TXT")
    private String groupCommentTxt;

    @Column(columnDefinition = "CHAR(1)",name = "LOGICAL_DELETE_IND")
    private String logicalDeleteInd;

    @Column(columnDefinition = "CHAR(9)",name = "LAST_UPDATE_PARTY_TD_ID")
    private String lastUpdatePartyTdId;

    @Column(columnDefinition = "CHAR(4)",name = "LAST_UPDATE_PARTY_ID_TYPE_CD")
    private String lastUpdatePartyIdTypeCd;

    @Column(columnDefinition = "TIMESTAMP(6)", name = "LAST_UPDATE_GMTS")
    private LocalDate lastUpdateGmts;

    @Column(columnDefinition = "VARCHAR2(50)",name = "GROUP_LABEL_TXT")
    private String groupLabelTxt;

    @Column(columnDefinition = "TIMESTAMP(6)", name = "DML_TIMESTAMP")
    private LocalDate dmlTimestamp;

}
===============================

import jakarta.persistence.*;
import java.io.Serializable;
import java.time.LocalDate;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;


@Entity
@IdClass(OmGroupTemplateRltnId.class)
@Table(schema = "OM_AUTHORING", name = "OM_GROUP_TEMPLATE_RLTN")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class OmGroupTemplateRltnEntity implements Serializable {
    private static final long serialVersionUID = 0L;

    @Id
    @Column(columnDefinition = "VARCHAR2(30)", name = "OM_TEMPLATE_NM")
    private Long omTemplateNm;

    @Id
    @Column(columnDefinition = "NUMBER(4)", name = "OM_TEMPLATE_VERSION_NR")
    private Long omTemplateVersionNr;

    @Id
    @Column(columnDefinition = "VARCHAR2(50)", name = "GROUP_NM")
    private Long groupNm;

    @Column(columnDefinition = "NUMBER(3)", name = "MINIMUM_OCCUR_QTY")
    private Long minimumOccurQty;

    @Column(columnDefinition = "NUMBER(3)", name = "MAXIMUM_OCCUR_QTY")
    private Long maximumOccurQty;

    @Column(columnDefinition = "VARCHAR2(50)", name = "OVERRIDE_LABEL_TXT")
    private String overrideLabelTxt;

    @Column(columnDefinition = "VARCHAR2(250)", name = "OVERRIDE_COMMENT_TXT")
    private String overrideCommentTxt;

    @Column(columnDefinition = "CHAR(1)", name = "LOGICAL_DELETE_IND")
    private String logicalDeleteInd;

    @Column(columnDefinition = "CHAR(2)", name = "GROUP_LOCATION_CD")
    private String groupLocationCd;

    @Column(columnDefinition = "NUMBER(4)", name = "GROUP_SEQUENCE_NR")
    private Long groupSequenceNr;

    @Column(columnDefinition = "TIMESTAMP(6)", name = "DML_TIMESTAMP")
    private LocalDate dmlTimestamp;

    @Column(columnDefinition = "VARCHAR2(30)", name = "VARIABLE_REPOSITORY_NM")
    private Long variableRepositoryNm;
    }
    ============================================

    
import java.io.Serializable;
import java.util.Objects;

public class OmGroupTemplateRltnId implements Serializable {
    private String omTemplateNm;
    private Integer omTemplateVersionNr;


    // Default constructor
    public OmGroupTemplateRltnId() {
    }

    // Constructor with fields
    public OmGroupTemplateRltnId(String omTemplateNm, Integer omTemplateVersionNr) {
        this.omTemplateNm = omTemplateNm;
        this.omTemplateVersionNr = omTemplateVersionNr;

    }


    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof OmGroupTemplateRltnId)) return false;
        OmGroupTemplateRltnId that = (OmGroupTemplateRltnId) o;
        return Objects.equals(omTemplateNm, that.omTemplateNm) &&
                Objects.equals(omTemplateVersionNr, that.omTemplateVersionNr);

    }

    @Override
    public int hashCode() {
        return Objects.hash(omTemplateNm, omTemplateVersionNr);
    }
}
=====================================================================
package com.usaa.ect.repositories;


import com.usaa.ect.entities.OmGroupTemplateRltnEntity;
import com.usaa.ect.entities.OmGroupTemplateRltnId;
import jakarta.transaction.Transactional;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.util.List;
@Repository
@Transactional

public interface OmGroupTemplateRltnRepository extends JpaRepository<OmGroupTemplateRltnEntity, OmGroupTemplateRltnId> {

    @Query("SELECT gt FROM GroupTemplateRltn gt JOIN GroupDefinition gd ON gt.groupName = gd.groupName")
    List<OmGroupTemplateRltnEntity> findAllWithGroupDefinition();
}
==========================================


        import com.usaa.ect.dtos.GroupDefinitionsObjectDto;
        import com.usaa.ect.entities.OmGroupTemplateRltnEntity;
        import com.usaa.ect.repositories.OmGroupTemplateRltnRepository;
        import org.modelmapper.ModelMapper;
        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Service;
        import java.util.List;
        import java.util.stream.Collectors;

@Service
public class GroupDefinitionService {

    @Autowired
    private OmGroupTemplateRltnRepository repository;

    @Autowired
    private ModelMapper modelMapper;

    public List<GroupDefinitionsObjectDto> getAllWithTemplateRltn() {
        List<OmGroupTemplateRltnEntity> entities = repository.findAllWithGroupDefinition();
        // Use ModelMapper to map the list of entities to DTOs
        List<GroupDefinitionsObjectDto> groupDefinitionsObjectDtoList = entities.stream()
                .map(entity -> modelMapper.map(entity, GroupDefinitionsObjectDto.class))
                .collect(Collectors.toList());

        return groupDefinitionsObjectDtoList;
    }
}








