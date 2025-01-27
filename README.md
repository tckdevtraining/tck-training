# tck-training

This is a training about Talend Component Kit (_TCK_) framework:
- https://talend.github.io/component-runtime/

The goal is to present how to develop a Talend connector that could be deployed in:
- Talend studio: based eclipse platform to design / exec ETL jobs
  - https://www.talend.com/fr/lp/open-studio-for-data-integration/
- Talend Cloud: manage your datasets and build pipeline from the Cloud
  - https://www.talend.com/ps/free-trial/

## Steps
1. Setup a redis back-end environment
1. Setup development environment
1. Develop a producer that will read data from back-end
1. Deploy producer in Talend open studio
1. Improve unit test embedded Redis (https://github.com/ozimov/embedded-redis)
1. Tools Talend to to help developers (web tester, maven plugin)
1. Develop a sink that will write in back-Talend
1. Presentation of Talend cloud & Remote engine
1. Deploy tck connectors in remote engine
1. Improve connector with services (get list of database + suggestable + @Checkable / Service de creation de connection with cache)
1. Support several kind of data format (raw text, json, csv)
