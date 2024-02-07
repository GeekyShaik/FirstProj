@Test
void getTemplateProfile_ThrowsInternalServerError_WhenServiceThrowsException() throws Exception {
    // Arrange
    when(templateProfileService.getTemplatesByNameAndVersion("ECM_Delivery_NewBrnd", 1))
            .thenThrow(new RuntimeException("Unexpected error"));

    // Required headers
    HttpHeaders headers = new HttpHeaders();
    headers.add("id-claim", ITAuthConfig.EMP_TOKEN); // Add other necessary headers here

    // Create a local instance of MockMvc with a custom exception resolver
    MockMvc mockMvcWithCustomExceptionResolver = MockMvcBuilders
            .standaloneSetup(new ObjectProfileController(templateProfileService))
            .setControllerAdvice(new Object() { // Anonymous inner class for exception handling
                @ExceptionHandler(RuntimeException.class)
                public ResponseEntity<String> handleRuntimeException(RuntimeException ex) {
                    return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Internal Server Error occurred");
                }
            })
            .build();

    // Act & Assert
    mockMvcWithCustomExceptionResolver.perform(get("/template-details/template-name/ECM_Delivery_NewBrnd/template-version/1")
            .headers(headers))
            .andExpect(status().isInternalServerError());
}
