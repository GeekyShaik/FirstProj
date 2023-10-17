package com.example.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface OMMessageTemplateRepository extends JpaRepository<Object[], String> {

    @Query(value = 
           "SELECT " +
           "t.om_template_nm, t.msg_templ_version_effective_dt, t.om_template_version_nr, t.enterprise_document_id, " +
           "t.variable_schema_nm, t.ops_procedure_area_ac, t.OPS_PROCEDURE_CATEGORY_AC, t.OPS_PROCEDURE_SUBCATEGORY_AC, " +
           "t.MESSAGE_TEMPLATE_TYPE_CD, t.DISPLAY_NM, t.REGULATORY_CLASS_CD, t.SOURCE_SYSTEM_DOCUMENT_ID, t.SOURCE_TOOL_DC, " +
           "t.goget_collection_nm, t.PAGE_NUM_HORZ_ALIGN_CD, t.PAGE_NUM_TYPE_CD, t.PAGE_NUM_VERT_ALIGN_CD, " +
           "t.PAGE_NUM_DISPLAY_RANGE_CD, t.PAGE_NUM_SHOW_COUNT_IND, t.PAGE_NUM_COUNT_RANGE_CD, t.CONT_NUM_ACRS_CCCOVER_IND, " +
           "t.CONT_NUM_ACRS_MASTER_IND, t.channel_default_cd, t.channel_option_default_cd, c.DELIVERY_CHANNEL_CD, " +
           "c.DELIVERY_CHANNEL_OPTION_CD, c.special_channel_metadata_txt " +
           "FROM om_authoring.om_message_template t " +
           "JOIN om_authoring.om_message_template_channel c ON t.om_template_nm = c.om_template_nm AND t.om_template_version_nr = c.om_template_version_nr " +
           "WHERE t.om_template_nm = '69434_MT_Cncl_Ntc_Chattel_Mort' " +
           "AND t.om_template_version_nr = 1",
           nativeQuery = true)
    List<Object[]> findTemplateWithChannels();
}
