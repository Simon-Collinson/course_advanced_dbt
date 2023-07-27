# Week 1 Project

Below are some notes on the week 1 project.

## List of issues raised by dbt_project_evaluator 

The issues below were raised by the dbt_project_evaluator package on first running the package. Below each issue I have provided notes on how I resolved the issue.

-- valid_documentation_coverage --
|                      MEASURED_AT | TOTAL_MODELS | DOCUMENTED_MODELS | DOCUMENTATION_COVERAGE_PCT | STAGING_DOCUMENTATION_COVERAGE_PCT | INTERMEDIATE_DOCUMENTATION_COVERAGE_PCT | ... |
| -------------------------------- | ------------ | ----------------- | -------------------------- | ---------------------------------- | --------------------------------------- | --- |
| 2023-07-27 08:50:44.790000-07:00 |           11 |                10 |                      90.91 |                                100 |                                     100 | ... |

Solution: add docs for fct_events model.

-- is_empty_fct_model_naming_conventions_ --
| RESOURCE_NAME | PREFIX | MODEL_TYPE | APPROPRIATE_PREFIXES |
| ------------- | ------ | ---------- | -------------------- |
| mrr           | mrr_   | marts      | dim_, fct_           |

Solution: add fct_ prefix to mrr table, because this is a fact table and I like consistency.

-- is_empty_fct_undocumented_models_ --
| RESOURCE_NAME | MODEL_TYPE |
| ------------- | ---------- |
| fct_events    | marts      |

Solution: add docs for fct_events model.

-- is_empty_fct_model_directories_ --
| RESOURCE_NAME | RESOURCE_TYPE | MODEL_TYPE | CURRENT_FILE_PATH                 | CHANGE_FILE_PATH_TO                |
| ------------- | ------------- | ---------- | --------------------------------- | ---------------------------------- |
| dim_dates     | model         | marts      | models/intermediate/dim_dates.sql | models/.../marts/.../dim_dates.sql |

Solution: exclude dim_dates from the test coverage. This is not a 'real' dimensional table, and is instead only used as a ref for other models.

-- is_empty_fct_root_models_ --
| CHILD     |
| --------- |
| dim_dates |

Solution: exclude dim_dates from the test coverage. This is a table generated entirely by code, so it is acceptable for it to have zero root models.

-- is_empty_fct_missing_primary_key_tests_ --
| RESOURCE_NAME | MODEL_TYPE | IS_PRIMARY_KEY_TESTED | NUMBER_OF_TESTS_ON_MODEL |
| ------------- | ---------- | --------------------- | ------------------------ |
| fct_events    | marts      |                 False |                        0 |

Solution: add primary key tests (not_null, unique) to event_id column

-- valid_test_coverage --
|                      MEASURED_AT | TOTAL_MODELS | TOTAL_TESTS | TESTED_MODELS | TEST_COVERAGE_PCT | STAGING_TEST_COVERAGE_PCT | ... |
| -------------------------------- | ------------ | ----------- | ------------- | ----------------- | ------------------------- | --- |
| 2023-07-27 08:50:47.021000-07:00 |           11 |          72 |            10 |             90.91 |                       100 | ... |