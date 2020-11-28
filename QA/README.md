# CherryTwist - Quality Assurance

This document provides overview of "Cherrytwist" quality assurance and testing activities. Some of them are in progress and some are to be started.

# Process
 
- Perform functional/non-functional testing types on reviewed "github" issues
    - Depending on the "github" issue scope, testing is performed on different environments 
- Log defects, communicate  and retest them
- Perform regression testing as part of "github" issues testing
- Automate/maintain test cases on different levels
- Test data management


# Testing types and levels

"Cherrytwist" consists of multiple services. The following test types are being applied among the different test levels:

## "Server" - testing types and levels:
- ### Functional testing: 
    - Unit testing<sup>*</sup>
    - [GraphQL API integration tests](https://github.com/cherrytwist/Server/tree/develop/test)
    - GraphQL Exploratory testing
        
- ### Non-functional testing:
    - [Performance testing](https://github.com/cherrytwist/Server/tree/develop/test/performance)
    - Security testing<sup>*</sup>
    - Database migration<sup>*</sup>

- ### Static testing:
    - Code review
    - Static code analyses tools<sup>*</sup>
 
## "Client.Web" - testing types and levels:
- ### Functional testing: 
    - Component testing<sup>*</sup>
    - Integration testing<sup>*</sup>
    - E2E automation testing<sup>*</sup>
    - Exploratory testing
        
- ### Non-functional testing:
    - Performance testing<sup>*</sup>
    - Security testing<sup>*</sup>


# Reporting
 
- [Test cases](https://travis-ci.com/github/cherrytwist/Server/builds)

    - Number of test cases
    - Number of failed test cases per service

- Defects<sup>*</sup>

    - Number of defects<sup>*</sup>
    - Number of production environment defects by severity<sup>*</sup>
    - Number of development environment defects by severity<sup>*</sup>

# Environments
- Local - setup for [Server](https://github.com/cherrytwist/Server) and [Client.Web](https://github.com/cherrytwist/Client.Web)
- [Development](https://dev.cherrytwist.org/) - auto-deployed after each merge to development branch
- [Test](https://test.cherrytwist.org/) - QA deploys from "github" repositories action, the required branch
- [Acc](https://acc.odyssey.ninja:3000/ecoverse) - Project manager deploys from "github" repositories action, from master branch 
- [Prod](https://prod.odyssey.ninja:3000/ecoverse) - Project manager deploys from "github" repositories action, from master branch

# Tools
- [Jest](https://jestjs.io/) - javascript automation test framework
- [Jmeter](https://jmeter.apache.org/) - performance test tool 
- [Postman](https://www.postman.com/) - API testing tool
- [MySQL 8](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/) - query data and testing the systems integration
- [Populator.GSheet](https://github.com/cherrytwist/Populator.GSheet) - test data population
- Test management system - for the moment, the manual testing is tracked within "github" issues
    - [KIWI TCMS](https://kiwitcms.org/features/) is being evaluated for manual test case management

**NOTE**

Test terminology meaning is based on: ["ISTQB" syllabus](https://www.istqb.org/downloads/send/69-2019-advanced-level-test-analyst/303-advanced-level-syllabus-2019-1-test-analyst.html)

<sup>*</sup>to be defined/implemented