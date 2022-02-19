
# NIEM API 2.0 Background

**Contents**

- [Requirements](#requirements)
- [Target Audiences](#target-audiences)
- [Community content](#community-content)
- [Development strategy](#development-strategy)
- [History](#history)
  - [NIEM API 1.0](#niem-api-10)
  - [NIEM API 1.1](#niem-api-11)

## Requirements

**Support multiple NIEM formats**

- NIEM will be adding support for new representations, including JSON, OWL, and RDF.
- In order to support multiple formats, the API will need to leverage the new Common Model Format (CMF), which will serve as the basis from which the various transformers will be written.

**Integrate the fragmented NIEM tool architecture**

Tool | Purpose | Current scope
--- | --- | ---
SSGT | Search and build NIEM XSD subsets | NIEM release database only
Migration tool | Migrate NIEM XSD subsets to newer releases | NIEM release database only
ConTesA | Check schemas against NDR conformance rules | NIEM XSDs only
Movement | Search and limited NIEM JSON subset support | Current NIEM release XSDs only
CMF transformers | Transform between NIEM representations | Java cli tool not integrated with other NIEM tools
Model management | Add, modify, and delete NIEM content | NIEM release database only
NIEM QA tools | Run checks on | NIEM release database only
NIEM doc tools | Generate spreadsheets and other documentation | NIEM release database only
MEP Builder | Build IEPDs | Leverages the API for NIEM subsets <br/> Would have to replicate existing model management and other capabilities for IEPD extensions without new support

**Updated existing functionality**

- Transition existing NIEM API and libraries to open source code.
- Simplify code base, remove outdated requirements, update libraries, leverage CMF
- Add additional tests and documentation
- Move load development resources (model management, QA, XSDs, etc) to the API
- Support community content (IEPD extensions, "NIEM Lite", local data models)

## Target Audiences

**NIEM tool developers**

- Make the API easier to use.  Move from SOAP web services with XML requests and responses to a more modern REST API with JSON.
- Provide current lead developer capabilities through the API so third-party developers can leverage existing work and spend their time and efforts on novel features and UIs.
- Provide new capabilities through the API, like NIEM JSON and CMF support.
- Build complex NIEM-specific logic into simple API calls that aggregate logical sets of data together.  For example, make it easy to get inherited properties of a type or a property's code set without having to make multiple calls to walk the NIEM model.

**IEPD developers**

- Expand the capability to search and subset NIEM release components to IEPD extensions and other community content.  This allows IEPD content to also be treated as reusable building blocks instead of just all-or-nothing IEPD bundles and lets us better leverage community efforts.
- Manage IEPD and domain content with expanded NIEM tool support.
- Transform between different NIEM formats.

**NIEM Management Office**

- Support the move of NIEM to an OASIS Open Project.
- Transition existing code to open source resources.

## Community content

NIEM community members create their own NIEM-conformant content to support data requirements not available in the NIEM Model.

**Kinds of community content**

Kind | Description
--- | ---
IEPD extensions | Generate and message-specific extensions created in an IEPD
Enterprise model | Full-blown data model and optional NIEM subset for an organization to be reused across multiple IEPDs
NIEM Lite | Special-purpose content published for general reuse across the community but doesn't meet the broader scope or requirements of a NIEM domain

**Ways to publish community content**

Given user approval, community content could be published in the following ways:

- Users build IEPDs in a NIEM tool like MEP Builder that saves and manages the components via the NIEM API, leveraging the same resources that save and manage release components.
- The public IEPD repo could send IEPDs to the NIEM API to scrape and save extension components.
- A special-purpose UI could be created as a front-end to the API, allowing users to upload schemas or other NIEM formats and publish the components for reuse.

**Reusing community content**

- Users search via the SSGT or other NIEM tools that leverage NIEM API 2.0.  Results will include both NIEM release and community content and both kinds of content can be added to new subsets.
- Users can leverage NIEM tools to manage and reuse their own extensions across multiple IEPDs.

## Development strategy

**Leverage free and public GitHub resources**

- Publish OpenAPI files, code libraries, database schema, and test data sets to GitHub
- Publish documentation with GitHub Pages
- Use issues and projects for project management support
- Use GitHub Actions for CI testing

**NIEM API 1.1 support**

Support for NIEM API 1.1 and its current libraries will need to be continued until the SSGT, Migration Tool, MEP Builder and other third-party tools can transition to the new version.  The backend of 1.1 will be updated to leverage the new version so multiple database and libraries will not need to run in parallel during the transition period.

## History

### NIEM API 1.0

NIEM API 1.0 was developed years after the SSGT as a way to provide some of the same basic functionality to third-party tools.  The SSGT does not use the API internally, but they both leverage the same NIEM libraries.

**Background**

- SOAP-based web services, described by WSDL files.
- Requests and responses in XML.
- Used WS-Security to authenticate requests.  Required all requests to be signed with a X.509 certificate public key.
- Support and maintenance was discontinued after NIEM 3.0.  The API was left running as is, but eventually became non-responsive due to library and other environment changes.

**Features**

Client provides | API returns | Limitation
--- | --- | ---
Search terms | Search results | Current release only
Component name | Component details | Current release only
Wantlist | Subset in NIEM XSD |
Codes in a spreadsheet | Codes in NIEM XSD | NIEM 2.0, 2.1 only

### NIEM API 1.1

The NIEM API was updated in 2021 to restore previous functionality and to provide additional support needed for the MEP Builder tool.

**Changes**

- Removed WS-Security / X.509 certificate security requirements.
- Added search and component detail support for previous releases.
- Consolidated domain updates with their corresponding releases.
- Added support for multiple search terms.
- Simplified simple type requirements in the wantlists: all facets on a type are now added by default if none are provided.
