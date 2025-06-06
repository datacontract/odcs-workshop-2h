# Hands-on Workshop on Data Contracts using the Data Contract CLI

The workshop has been given at the [Data Mesh Live 2025 conference](https://2025.datameshlive.com/program/data-contract-workshop-using-open-source-tooling/).

## Introduction (20 mins)

We introduce the Open Data Contract Standard and the popular open source tool Data Contract CLI to lay a foundation of the YAML format and the tool we will use throughout the session.

### Steps

1. Intro:
   - Who are the trainers?
   - Have you used data contracts before?
   - Why data contracts?
2. Intro [ODCS](https://bitol-io.github.io/open-data-contract-standard/)
3. Intro [Data Contract CLI](https://github.com/datacontract/datacontract-cli)
4. Demo Data Contract CLI (In Terminal)
   - `datacontract --help`

### Resources
- [ODCS Docs](https://bitol-io.github.io/open-data-contract-standard/)
- [ODCS Source (GitHub)](https://github.com/bitol-io/open-data-contract-standard)
- [Data Contract CLI (GitHub)](https://github.com/datacontract/datacontract-cli)

## Task 1: Data-First, or Put Your Data Under Contract (40 mins)

Using our case study, participants take data under contract. They create a data contract for existing data, starting with an imported draft (datacontract import) and then add more and more iteratively, including quality checks. They will validate the data against their contract (datacontract test), create HTML documentation and visualizations (datacontract export), and even a data contract catalog (datacontract catalog). We have a short retro after this exercise.

### Steps
0. Trainers show data in databricks as starting point
1. Install Data Contract CLI (trainers will help)
   - Easiest with [uv](https://docs.astral.sh/uv/): `uv tool install 'datacontract-cli[all]'`
   - Check if it works with `datacontract --version`
   - Alternative Docker: `docker run --rm -v ${PWD}:/home/datacontract datacontract/cli --version`
2. Set up environment variables for a connection to Databricks using the environment variables provided by the trainers.
```
export DATACONTRACT_DATABRICKS_TOKEN=dapi...
export DATACONTRACT_DATABRICKS_SERVER_HOSTNAME=adb-...x.azuredatabricks.net
export DATACONTRACT_DATABRICKS_HTTP_PATH=/sql/1.0/warehouses/xxx
```

3. Use the **import** command to put the data available on Databricks under contract.
   - Tables in the format catalog.schema.table: 
       - datameshlive.orders.orders
       - datameshlive.orders.line_items
   - Command:
```
datacontract import \
  --format unity \
  --unity-table-full-name datameshlive.orders.orders \
  --unity-table-full-name datameshlive.orders.line_items \
  --spec odcs \
  --output orders.odcs.yaml
```

4. Add descriptions and semantics describing the data (It’s OK to make some business assumptions. :-) )

5. Use the **test** command to check whether the data on Databricks confirm to the Data Contract

```
datacontract test orders.odcs.yaml
```

6. Add constraints and SQL-based quality checks on the data and re-run the tests with the **test** command.
    - Add constraints like `required: true`, ...
    - [Add SQL-based quality check](https://bitol-io.github.io/open-data-contract-standard/latest/#sql) at the property or schema level.

```
  properties:
  - name: customer_email_address
    physicalType: string
    logicalType: string
    required: true
    quality:
      - type: sql
        description: Ensure that the email address is valid
        query: SELECT COUNT(*) FROM {schema} WHERE {property} NOT LIKE '%@%'; 
        mustBe: 0
```
     
7. Use the **export** command to create an HTML documentation of the data contract. 

```
datacontract export --format html orders.odcs.yaml
```

8. Try some other [exports](https://cli.datacontract.com/#export)

```
datacontract export --format sql orders.odcs.yaml
```

10. Use the **catalog** command to create a data contract catalog.
   - Command: `datacontract catalog`
11. **BONUS** Use the integration with Data Mesh Manager
    1. Create an account and add an organization in Data Mesh Manager (www.datamesh-manager.com)
    2. Create an API key and set the environment variable `export DATAMESH_MANAGER_API_KEY=dmm_live_...` 
    3. Use the **publish** command to publish the data contract to the Data Mesh Manager. You need to add a custom property
       - Command: `datacontract publish orders.odcs.yaml`
       - Note: you need to set a customProperty `owner` to the ID of the owning team in Data Mesh Manager.
       - Export to excel using the Web UI of Data Mesh Manager
    4. Use the **test --publish** command with the publish option to publish the test results to the Data Mesh Manager
       - Command: `datacontract test --publish-test-results orders.odcs.yaml`
12. Share your experience in a short retrospective.

### Resources
- [Data Contract CLI (GitHub)](https://github.com/datacontract/datacontract-cli)
- If you discover a bug during this workshop, please report it on the [GitHub Issues page](https://github.com/datacontract/datacontract-cli/issues)! :-)

## BREAK (10 mins)

### Steps
1. Close the lid of your laptop
2. Grab a coffee
3. Talk to others

## Task 2: Contract-First, or Specify a Data Contract in a Workshop (40 mins)

After a break, we switch to contract-first. A data contract is created based on requirements in a group exercise with a facilitator, a data consumer, and a data producer. The facilitator elicit the requirements from the data consumer and writes the YAML files, while making sure that the data producer is fine with it as they have to own the contract afterwards. We have a short retro after this exercise.

### Steps
0. Trainers show the Excel template
1. Form groups of three, and assign the different roles (facilitator, data provider, data consumer)
2. Open the Excel template `task2.datacontract.odcs.xlsx` (recommended)
3. Follow the [guidelines for ODCS](http://datacontract.com/workshop#guidelines-for-odcs). Ignore data quality checks for this exercise.
4. Start the workshop and play your roles. :-)
5. **BONUS** Convert excel to yaml with the Data Contract CLI via the **import --format excel** command
6. Share your experience in a short retrospective.

### Resources
- [Excel template](https://github.com/datacontract/open-data-contract-standard-excel-template)
- [Guidelines for ODCS](http://datacontract.com/workshop#guidelines-for-odcs)
- [Instructions for Data Provider](task2-dataprovider.md)
- [Instructions for Data Consumer](task2-dataconsumer.md)

## Conclusion (10 mins)

We finish the session by talking about automation potentials after having introduced data contracts.

### Steps
- Add your idea on what can be automated on the flip chart.
- Open Discussion until the end.
- Do not forget to give ODCS and the Data Contract CLI a star on GitHub! :-)
   - https://github.com/datacontract/datacontract-cli
   - https://github.com/bitol-io/open-data-contract-standard
- Connect on LinkedIn:
   - [Dr. Simon Harrer](https://www.linkedin.com/in/simonharrer/)
   - [Jochen Christ](https://www.linkedin.com/in/jochenchrist/)
- Rate the workshop on the conference platform!

## Authors

**Dr. Simon Harrer** is a Senior Consultant at INNOQ in Germany. He is software developer at heart who has now turned to the dark side, namely the world of data. He co-authored datamesh-architecture.com and translated the Data Mesh book by Zhamak Dehghani into German. Next to his data mesh consulting projects, he is currently developing the [Data Mesh Manager](https://www.datamesh-manager.com), a SaaS product to fast-track any data mesh initiative.

**Jochen Christ** is a Senior Consultant at INNOQ in Germany. He is a specialist for self-contained systems and data mesh. As a technical lead, he helps teams with complex transformations to innovative IT solutions. Jochen is maintainer of http-feeds.org and co-author of remotemobprogramming.org and datamesh-architecture.com. Together with Simon, he is developing the [Data Mesh Manager](https://www.datamesh-manager.com), a SaaS product to fast-track any data mesh initiative.
