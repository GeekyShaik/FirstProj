
@ExtendWith(SpringExtension.class)
@WebMvcTest(excludeFilters = {@ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = WebSecurityConfigurer.class)},
        excludeAutoConfiguration = {SecurityAutoConfiguration.class})
