package com.metadata.service.repository;

import com.metadata.service.entity.TemplateProfileObject;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface TemplateProfileObjectRepository extends JpaRepository<TemplateProfileObject, Long> {

    @Query("SELECT t FROM TemplateProfileObject t WHERE t.templateName = :templateName AND t.omTemplateVersionNr = :omTemplateVersionNr")
    List<TemplateProfileObject> findByTemplateNameAndVersion(String templateName, String omTemplateVersionNr);
}
