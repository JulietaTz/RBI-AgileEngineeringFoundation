# US5003 - Replace OR-Mapper

Background: Due to security and performance reasons the maintainance of ORMapper Versions 1.2 is deprecated. 
We must replace the version 1.2 with 2.0. In the version 1.2 fields was stored in the wrong database fields 
althougt the mapping configuration was correct.


## US5003-Replace-ORMapper

As the data engineer
I want that the actual ORMapper V1.2 will be replace with ORMapper V2.0
In order to improve the security and performance of the data access
and fix the mapping bug.


### Acceptance Criteria
- ACC1: All data structure are migrated to the new schema of V2.0.
- ACC2: Data structure ACCESS is denormalized.
- ACC3: A full table scan of reference table REF\_ORG\_Data (in PERF-ENV-Cloud-098) take < 1,5sec.