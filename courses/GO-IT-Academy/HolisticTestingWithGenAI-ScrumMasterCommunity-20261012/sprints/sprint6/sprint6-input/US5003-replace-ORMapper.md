# US5003 - Replace OR-Mapper

Background: Due to security and performance reasons the maintainance of ORMapper Versions 1.2 is deprecated. 
We must replace the version 1.2 with 2.0. 
In the version 1.2 some fields was stored in the wrong database fields althougt the mapping configuration was correct. Example: Lastname from UI was stored in firstname on backend, and firstname from UI was stored in lastname in backend.


## US5003-Replace-ORMapper

As the data engineer
I want that the actual ORMapper V1.2 will be replace with ORMapper V2.0
In order to improve the security and performance of the data access
and fix the mapping bug.


### Acceptance Criteria
- ACC-01: ORMapper Version 1.2 is fully replaced by ORMapper Version 2.0 in all application components.
- ACC-02: All existing data structures are migrated successfully to the ORMapper V2.0 schema.
- ACC-03: The data structure ACCESS is denormalized according to the V2.0 design specification.
- ACC-04: Existing application functionality using database access continues to work correctly after the migration.
- ACC-05: Data previously stored in incorrect database fields due to the V1.2 mapping bug is stored in the correct target fields after the migration.
- ACC-06: Database read and write operations execute successfully using the ORMapper V2.0 implementation.
- ACC-07: A full table scan of reference table REF_ORG_Data in PERF-ENV-Cloud-098 completes in less than 1.5 seconds.
- ACC-08: No data loss occurs during the migration from ORMapper V1.2 to ORMapper V2.0.
- ACC-09: Security vulnerabilities related to ORMapper V1.2 are no longer present after the replacement.
- ACC-10: Logging and monitoring correctly capture database mapping and migration errors in the ORMapper V2.0 implementation.