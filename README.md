 public TemplateProfileObjectDTO retrieveTemplateWebClientCall(String basePath, String pathLabels, String objectName, BigDecimal objectVersion,
                                                                  String mdataObjTypeCd) throws Exception {

        //GET AuthoringMetadataObject
//        AuthoringMetadataObjectDTO authoringMetadataObject = null;
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
                .bodyToMono(TemplateProfileObjectDTO.class)
                .block();

    }
