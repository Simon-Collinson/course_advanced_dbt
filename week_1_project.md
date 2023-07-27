# Week 1 Project

Below are some notes on the week one project, including my answers to question three in the submission document.



## dbt_project_evaluator intro

The issues below were raised by the dbt_project_evaluator package on first running the package. Below each issue I have provided notes on how I resolved it.

## Documentation rules

### [valid_documentation_coverage](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/documentation/#documentation-coverage)
|                      MEASURED_AT | TOTAL_MODELS | DOCUMENTED_MODELS | DOCUMENTATION_COVERAGE_PCT | STAGING_DOCUMENTATION_COVERAGE_PCT | INTERMEDIATE_DOCUMENTATION_COVERAGE_PCT | ... |
| -------------------------------- | ------------ | ----------------- | -------------------------- | ---------------------------------- | --------------------------------------- | --- |
| 2023-07-27 08:50:44.790000-07:00 |           11 |                10 |                      90.91 |                                100 |                                     100 | ... |

Problem: Only 10/11 models were documented, although documentation target was 100%.

Solution: I want to maintain a 100% documentation rate, so I added docs for `fct_events` model.

### [fct_undocumented_models](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/documentation/#undocumented-models)
| RESOURCE_NAME | MODEL_TYPE |
| ------------- | ---------- |
| fct_events    | marts      |

Problem: The `fct_events` model was not documented.

Solution: Added docs for `fct_events` model, this needed to be documented.

## Structure rules

### [fct_model_naming_conventions](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/structure/#model-naming-conventions)
| RESOURCE_NAME | PREFIX | MODEL_TYPE | APPROPRIATE_PREFIXES |
| ------------- | ------ | ---------- | -------------------- |
| mrr           | mrr_   | marts      | dim_, fct_           |

Problem: The `mrr` model is in the `marts` directory, so per dbt convention it should be either a dimension or fact table and labelled as such. However, it does not have either of the mandatory prefixes.

Solution: Added `fct_` prefix to mrr table, because this is indeed a fact table and I like consistency.

### [fct_model_directories](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/structure/#model-directories)
| RESOURCE_NAME | RESOURCE_TYPE | MODEL_TYPE | CURRENT_FILE_PATH                 | CHANGE_FILE_PATH_TO                |
| ------------- | ------------- | ---------- | --------------------------------- | ---------------------------------- |
| dim_dates     | model         | marts      | models/intermediate/dim_dates.sql | models/.../marts/.../dim_dates.sql |

Problem: `dim_dates` is considered to be in the wrong directory, as `fct_*` tables are expected to be under `marts/`.

Solution: exclude `dim_dates` from the test coverage. This is not a 'real' fact table, and is instead only used as a referent for other models.

## Modelling rules

### [fct_root_models](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/modeling/#root-models)
| CHILD     |
| --------- |
| dim_dates |

Problem: `dim_dates`, although a dimensional table, does not have any root models.

Solution: exclude `dim_dates` from the test coverage. This is a table generated entirely by code (the `dbt_utils` package), so it is acceptable for it to have zero root models.

## Testing rules

### [fct_missing_primary_key_tests](https://dbt-labs.github.io/dbt-project-evaluator/0.6/rules/testing/#missing-primary-key-tests)
| RESOURCE_NAME | MODEL_TYPE | IS_PRIMARY_KEY_TESTED | NUMBER_OF_TESTS_ON_MODEL |
| ------------- | ---------- | --------------------- | ------------------------ |
| fct_events    | marts      |                 False |                        0 |

Problem: The `fct_events` table was missing the tests required for all primary keys.

Solution: As part of adding docs to satisfy fct_undocumented_models requirement, added primary key tests (`not_null`, `unique`) to `event_id` column.
