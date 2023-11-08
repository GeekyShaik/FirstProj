
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

