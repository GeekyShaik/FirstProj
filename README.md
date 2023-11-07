
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

 public TemplateProfileDTO retrieveTemplateWebClientCall(String basePath, String pathLabels, String objectName, BigDecimal objectVersion) throws Exception {

        return metadataWebServiceClient.get()
                .uri(
                        UriComponentsBuilder.fromUriString(
                                basePath + pathLabels)
                                .build(objectName, objectVersion))
                .header(apiKeyLabel, API_KEY)
                .accept(MediaType.APPLICATION_JSON)
                .retrieve()
                .onStatus(HttpStatusCode::is4xxClientError, clientResponse ->
                        Mono.error(new Throwable()))
                .onStatus(HttpStatusCode::is5xxServerError, clientResponse ->
                        Mono.error(new Throwable()))
                .bodyToMono(TemplateProfileDTO.class)
                .block();
    }


public TemplateProfileDTO retrieveTemplateWebClientCall(String basePath, String pathLabels, String objectName, BigDecimal objectVersion) throws Exception {
    // Make the web client call as before

    // Fetch data from the repository
    List<TemplateProfileDto> templateProfiles = omMessageTemplateRepository.findByTemplateNameAndVersion(objectName, objectVersion.toString());

    // Check if any results were found
    if (templateProfiles != null && !templateProfiles.isEmpty()) {
        // You can return the first result assuming it's unique or handle multiple results as needed
        return templateProfiles.get(0);
    } else {
        // Handle the case where no data is found
        throw new Exception("No matching template found");
    }
}
