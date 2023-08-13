# Week 3 Project

Below are some notes on the week three project.

## Task 1: Identify a few redundant tests that can be removed

I removed 23 redundant tests in total, across five models:

- stg_bingeflix_events (4)
- stg_bingeflix_subscription_plans (3)
- stg_bingeflix_subscriptions (3)
- stg_bingeflix_users (9)
- fct_events (4)

All five models had tests on columns which were substantially unchanged after being tested in the source models upstream.

## Task 3: Write a unit test to confirm MRR is calculated correctly

I manually executed the fct_mrr model in Snowflake based on my seed data (using the naming convention `testing_*` for the seeds) in order to generate correct output data. I then created the `unit_testing_select_table` macro and used it to switch between the expected and actual inputs and outputs.
