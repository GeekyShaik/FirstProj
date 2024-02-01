@Test
public void getTemplateProfile_Returns200Ok_WhenDataExists() throws Exception {
    // ... (other parts of the test)

    // Perform the request and capture the result for debugging
    MvcResult result = mockMvc.perform(get("/template-details/template-name/{templateName}/template-version/{templateVersion}", 
                        actualTemplateName, actualTemplateVersion))
            .andExpect(status().isOk())
            .andDo(MockMvcResultHandlers.print()) // This will print the response to the console
            .andReturn(); // Capture the result

    // Debugging: output the response body to the console
    String responseBody = result.getResponse().getContentAsString();
    System.out.println("Response body: " + responseBody);

    // Continue with assertions
    mockMvc.perform(asyncDispatch(result))
            .andExpect(jsonPath("$[0].templateName").value(expectedTemplateName))
            .andExpect(jsonPath("$[0].templateVersion").value(expectedTemplateVersion));
}
