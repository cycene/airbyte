
<!---
N.B. I ran the page through a 508/WCAG linter to ensure it's A11y compliant. :-)
-->

# SurveyMonkey

This page guides you through the process of setting up the SurveyMonkey source connector.

:::note

OAuth for Survey Monkey is supported only for the US. We are currently working to enable it in the EU. If you run into any issues, please [reach out to us](mailto:product@airbyte.io) so we can assist you.

:::

<!-- env:oss -->

## Prerequisites

**For Airbyte Open Source:**

- Access Token
- *need to add required permissions/access level*

<!-- /env:oss -->

## Setup guide

### Step 1: Set up SurveyMonkey

1. Read the [Survey Monkey Getting Started Guide](https://developer.surveymonkey.com/api/v3/#getting-started).
2. Register your application with [Survey Monkey](https://developer.surveymonkey.com/apps/).
3. Go Survey Monkey's Settings, and copy your access token. You'll need it to connect to your Airbyte account if you're using Airbyte Open Source.

<!---
I've assumed we're referring to Survey Monkey's settings. Can that be linked in some way? 
-->

### Step 2: Set up the source connector in Airbyte

<!-- env:cloud -->

<!---
In the following procedures, I standardized on **bold** for things users click. I see no reason why multiple styles were used. 
-->

**For Airbyte Cloud:**

1. Log into your [Airbyte Cloud account](https://cloud.airbyte.com/workspaces).
2. In the left navigation bar, click **Sources > + new source**.
3. On the source setup page, select **SurveyMonkey** from the dropdown and enter a name for the connector.
4. Click **Authenticate your account**.
5. Log in to and authorize your SurveyMonkey account.
6. Choose your required start date.
7. Click **Set up source**.
<!-- /env:cloud -->

<!-- env:oss -->

**For Airbyte Open Source:**

1. Go to your local Airbyte page.
2. In the left navigation bar, click **Sources > + new source**.
3. On the source setup page, select **SurveyMonkey** from the dropdown and enter a name for the connector.
4. Add your Access Token.
5. Choose your required start date.
6. Click **Set up source**.
<!-- /env:oss -->

<!---
In both of the previous procedures if choosing the start date and then clicking "set up source" happen on the same screen (which I suspect), I'd combine the steps. In general, I want to actually do the procedures to make sure (1) the steps work as written (2) no step is left out (3) the fields are named properly (4) etc.
-->

## Supported streams and sync modes

Refer to SurveyMonkey's [API documentation](https://api.surveymonkey.com/v3/docs#SurveyMonkey-Api) for information about the streams and sync modes that Airbyte supports:

- [Surveys](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-surveys) \(Incremental\)
- [SurveyPages](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-surveys-survey_id-pages)
- [SurveyQuestions](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-surveys-survey_id-pages-page_id-questions)
- [SurveyResponses](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-surveys-id-responses-bulk) \(Incremental\)
- [SurveyCollectors](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-surveys-survey_id-collectors)
- [Collectors](https://api.surveymonkey.com/v3/docs?shell#api-endpoints-get-collectors-collector_id-)

<!---
This section is particularly opaque for a newbie. I found the following definitions in the docs:
"A sync mode governs how Airbyte reads from a source and writes to a destination."
"A Stream is the atomic unit for reading data from a Source."
"An incremental Stream is a stream which reads data incrementally."
[Ignore for the moment the inconsistent capitalization; those definitions are cut-and-pasted.]
So, bearing those definitions in mind, Surveys and SurveyResponses are incremental streams. Are the others streams or sync modes? It's not clear here, and I'm not clear on how to tell when I only see SurveyMonkey's docs linked. Thus, this section needs more information - and if they're all streams, then the words "sync modes" can go.
-->

### Performance considerations

The SurveyMonkey API subjects private apps to [rate and response limits](https://api.surveymonkey.com/v3/docs#scopes). In order to gather more data from this source, we use [caching](https://docs.airbyte.com/connector-development/cdk-python/http-streams#nested-streams--caching}).

Rate limits:

- 120 requests per minute <!--- This was inaccurate. Glad I checked!-->
- 500 requests per day

Response limits:

- Max Page Size: 1000 resources, unless otherwise specified.
- Max Survey Size: 1000 questions. Surveys over limit will return a 413 error.

<!---
My preference would be not to include the specific information and just send users to their docs, since, as I just saw, the numbers can become inaccurate as the other product changes. However, if we're going to include it, we should include response limits in addition to rate limits.
-->

## Troubleshooting

*Provide information about troubleshooting here.*

___

## Changelog

<!---
I'm not sure this is the right place for the changelog. It would be a lot easier to maintain if we could automate the changelog publication and have it become a separate page/group of pages within the doc set. Plus, the section doesn't appear in the posted template. At any rate, other than fixing a misspelling and changing one word to US English spelling, I didn't edit it.
-->

| Version | Date       | Pull Request                                             | Subject                                                                          |
| :------ | :--------- | :------------------------------------------------------- | :------------------------------------------------------------------------------- |
| 0.3.3   | 2024-05-22 | [38559](https://github.com/airbytehq/airbyte/pull/38559) | Migrate Python stream authenticator to `requests_native_auth` package            |
| 0.3.2   | 2024-05-20 | [38244](https://github.com/airbytehq/airbyte/pull/38244) | Replace AirbyteLogger with logging.Logger and upgrade base image                 |
| 0.3.1   | 2024-04-24 | [36664](https://github.com/airbytehq/airbyte/pull/36664) | Schema descriptions and CDK 0.80.0                                               |
| 0.3.0   | 2024-02-22 | [35561](https://github.com/airbytehq/airbyte/pull/35561) | Migrate connector to low-code                                                    |
| 0.2.4   | 2024-02-12 | [35168](https://github.com/airbytehq/airbyte/pull/35168) | Manage dependencies with Poetry                                                  |
| 0.2.3   | 2023-10-19 | [31599](https://github.com/airbytehq/airbyte/pull/31599) | Base image migration: remove Dockerfile and use the python-connector-base image  |
| 0.2.2   | 2023-05-12 | [26024](https://github.com/airbytehq/airbyte/pull/26024) | Fix dependencies conflict                                                        |
| 0.2.1   | 2023-04-27 | [25109](https://github.com/airbytehq/airbyte/pull/25109) | Fix add missing params to stream `SurveyResponses`                               |
| 0.2.0   | 2023-04-18 | [23721](https://github.com/airbytehq/airbyte/pull/23721) | Add `SurveyCollectors` and `Collectors` stream                                   |
| 0.1.16  | 2023-04-13 | [25080](https://github.com/airbytehq/airbyte/pull/25080) | Fix spec.json required fields and update schema for surveys and survey_responses |
| 0.1.15  | 2023-02-11 | [22865](https://github.com/airbytehq/airbyte/pull/22865) | Specified date formatting in specification                                       |
| 0.1.14  | 2023-01-27 | [22024](https://github.com/airbytehq/airbyte/pull/22024) | Set `AvailabilityStrategy` for streams explicitly to `None`                      |
| 0.1.13  | 2022-11-29 | [19868](https://github.com/airbytehq/airbyte/pull/19868) | Fix OAuth flow urls                                                              |
| 0.1.12  | 2022-10-13 | [17964](https://github.com/airbytehq/airbyte/pull/17964) | Add OAuth for Eu and Ca                                                          |
| 0.1.11  | 2022-09-28 | [17326](https://github.com/airbytehq/airbyte/pull/17326) | Migrate to per-stream states                                                     |
| 0.1.10  | 2022-09-14 | [16706](https://github.com/airbytehq/airbyte/pull/16706) | Fix 404 error when handling nonexistent surveys                                  |
| 0.1.9   | 2022-07-28 | [13046](https://github.com/airbytehq/airbyte/pull/14998) | Fix state for response stream, fixed backoff behavior, added unittest           |
| 0.1.8   | 2022-05-20 | [13046](https://github.com/airbytehq/airbyte/pull/13046) | Fix incremental streams                                                          |
| 0.1.7   | 2022-02-24 | [8768](https://github.com/airbytehq/airbyte/pull/8768)   | Add custom survey IDs to limit API calls                                         |
| 0.1.6   | 2022-01-14 | [9508](https://github.com/airbytehq/airbyte/pull/9508)   | Scopes change                                                                    |
| 0.1.5   | 2021-12-28 | [8628](https://github.com/airbytehq/airbyte/pull/8628)   | Update fields in source-connectors specifications                                |
| 0.1.4   | 2021-11-11 | [7868](https://github.com/airbytehq/airbyte/pull/7868)   | Improve 'check' using '/users/me' API call                                       |
| 0.1.3   | 2021-11-01 | [7433](https://github.com/airbytehq/airbyte/pull/7433)   | Remove unused oAuth flow parameters                                             |
| 0.1.2   | 2021-10-27 | [7433](https://github.com/airbytehq/airbyte/pull/7433)   | Add OAuth support                                                                |
| 0.1.1   | 2021-09-10 | [5983](https://github.com/airbytehq/airbyte/pull/5983)   | Fix caching for gzip compressed http response                                    |
| 0.1.0   | 2021-07-06 | [4097](https://github.com/airbytehq/airbyte/pull/4097)   | Initial Release                                                                  |
