# CherryTwist - Quality Assurance

This document provides an overview of the quality assurance and testing activities that are carried out on the Cherrytwist platform.

# Process
 
- Perform functional/non-functional testing types on reviewed "github" issues
    - Depending on the "github" issue scope, testing is performed on different environments 
- Log defects, communicate  and retest them
- Perform regression testing as part of "github" issues testing
- Automate/maintain test cases on different levels
- Test data management
- Package dependency audit: regular review of reported vulnerabilities in packages dependent on and then address


# Testing types and levels

Cherrytwist consists of multiple services. The following test types are being applied among the different test levels:

## "Server" - testing types and levels:

First QA priority is the "Server" repository quality

- ### Functional testing: 
    - Unit testing<sup>*</sup>
    - [GraphQL API integration tests](https://github.com/cherrytwist/Server/tree/develop/test)
    - GraphQL Exploratory testing
        
- ### Non-functional testing:
    - [Performance testing](https://github.com/cherrytwist/Server/tree/develop/test/performance)
    - Security testing<sup>*</sup>
    - [Database migrations](https://github.com/cherrytwist/Server/edit/develop/README.md)

- ### Static testing:
    - Code review
    - Static code analyses tools:
        - Build Status:![Docker Image CI](https://github.com/cherrytwist/Server/workflows/Docker%20Image%20CI/badge.svg?branch=master)

        - Build Quality:[![BCH compliance](https://bettercodehub.com/edge/badge/cherrytwist/Server?branch=develop)](https://bettercodehub.com/)
 
## "Client.Web" - testing types and levels:

Secondary QA priority is the "Client.Web" repository quality

- ### Functional testing: 
    - Component testing<sup>*</sup>
    - Integration testing<sup>*</sup>
    - E2E automation testing<sup>*</sup>
    - Exploratory testing
        
- ### Non-functional testing:
    - Performance testing<sup>*</sup>
    - Security testing<sup>*</sup>

- ### Static testing:
    - Code review
    - Static code analyses tools:
        - Build Status:![AKS dev CI/CD pipeline](https://github.com/cherrytwist/Client.Web/workflows/AKS%20dev%20CI/CD%20pipeline/badge.svg?branch=develop)

        - Build Quality:[![BCH compliance](https://bettercodehub.com/edge/badge/cherrytwist/Client.Web?branch=develop)](https://bettercodehub.com/)

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
- [Development](https://dev.cherrytwist.org/) - auto-deployed after each merge to develop branch
- [Test](https://test.cherrytwist.org/) - QA deploys from "github" repositories action, the branch version that is being tested
- [Acc](https://acc.odyssey.ninja:3000/) - Project manager deploys from "github" repositories action, from master branch 
- [Prod](cherrytwist.odyssey.org) - Project manager deploys from "github" repositories action, from master branch

# Tools
- [VS Code] - IDE for automation test development
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