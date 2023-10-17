package com.example.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface OMMessageTemplateRepository extends JpaRepository<Object[], String> {

    @Query(value = 
           "SELECT t.om_template_nm, t.msg_templ_version_effective_dt, ... , c.DELIVERY_CHANNEL_OPTION_CD, c.special_channel_metadata_txt " +
           "FROM om_authoring.om_message_template t, om_authoring.om_message_template_channel c " +
           "WHERE t.om_template_nm = c.om_template_nm " +
           "AND t.om_template_version_nr = c.om_template_version_nr " +
           "AND t.om_template_nm = '69434_MT_Cncl_Ntc_Chattel_Mort' " +
           "AND t.om_template_version_nr = 1",
           nativeQuery = true)
    List<Object[]> findTemplateWithChannels();
}
