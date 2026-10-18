# AI-Powered Social Media Content Automation with n8n

An n8n workflow that automates parts of a social media content pipeline
by generating platform-specific copy with Google Gemini, creating an
image with Leonardo AI, and publishing image posts to supported social
platforms.

## Demo Video

Watch the workflow demonstration to see how the AI-powered social media automation system generates platform-specific content and images using Google Gemini and Leonardo AI, and publishes content to supported social platforms.

**[Watch the Demo Video on Google Drive](https://drive.google.com/file/d/1gPWHKrzTKN1d8CNBKJhZ0Nmb2RKQKXE5/view?usp=sharing)**



## Overview

The workflow takes a topic as input and generates social media content
for multiple platforms. It can then create a visual and use the
generated content in publishing steps configured in n8n.

### Workflow capabilities

-   Accepts a topic as workflow input.
-   Uses Google Gemini to generate a Facebook caption, Instagram
    caption, LinkedIn caption, YouTube description, hashtags, a call to
    action, and an image-generation prompt.
-   Uses Leonardo AI to generate an image.
-   Waits for image generation and retrieves the generated image URL.
-   Includes a Facebook image-posting step.
-   Can be extended to publish to Instagram using Meta's Instagram
    Content Publishing API.
-   Can be extended with validation, error branches, and execution
    logging.

> **Note:** Actual publishing depends on your n8n node configuration,
> valid API credentials, Meta app permissions, account eligibility, and
> the current APIs and plans offered by each provider. LinkedIn and
> YouTube content may be generated without being automatically published
> unless publishing nodes are configured for them.

## Technologies

-   **n8n** --- workflow automation
-   **Google Gemini API** --- content generation
-   **Leonardo AI API** --- image generation
-   **Meta Graph API** --- Facebook publishing and optional Instagram
    publishing

## High-level workflow

1.  Trigger or input receives a topic.
2.  Gemini generates platform-specific copy and an image prompt.
3.  A Code node parses and structures the model response.
4.  Leonardo AI starts image generation.
5.  A Wait node and image-retrieval request allow the image to finish
    and fetch its URL.
6.  Validation checks that an image URL is available.
7.  Configured publishing nodes send the image and caption to a social
    platform.
8.  Optional logging records success or failure.

Your exported workflow may use different node names or omit optional
steps; follow the actual nodes in your JSON export.

## Requirements

-   An n8n Cloud account or a local n8n installation.
-   A Google Gemini API key with access to the configured model.
-   A Leonardo AI API key and access to the image-generation endpoint
    used by the workflow.
-   For Facebook publishing, a Facebook Page you manage and a Page
    Access Token with the required permissions.
-   For Instagram publishing, an eligible Instagram Professional account
    linked to a Facebook Page, plus the required Meta permissions and
    publishing steps.
-   Internet access from n8n to the APIs.

Provider features, API versions, permission requirements, quotas, and
pricing may change. Check the providers' current documentation before
deployment.

## Setup

### 1. Import the workflow

1.  Export the workflow JSON from n8n.
2.  In n8n, choose the option to import a workflow from a file.
3.  Select the exported JSON.
4.  Review each node and reconnect credentials where needed.

### 2. Configure API credentials securely

Set up credentials or environment variables in n8n for the services you
use. Do not commit real credentials to this repository.

Never include these in the workflow export or README:

-   Gemini API key
-   Leonardo AI API key
-   Facebook or Instagram access tokens
-   App secrets, passwords, or private webhook URLs

Use placeholders such as `YOUR_API_KEY` in examples, and store actual
secrets in n8n credentials or an appropriate secret manager. If a token
has already been exposed, revoke or rotate it.

### 3. Configure Facebook publishing

-   Confirm that the target Facebook Page is managed by the account used
    to authorize the Meta app.
-   Generate a Page Access Token with the permissions required by your
    posting endpoint.
-   Store the token securely in n8n credentials or an environment
    variable.
-   Verify the Page ID and API endpoint in the Facebook publishing node.
-   Test with a non-sensitive sample post before enabling automatic
    runs.

Meta tokens can expire or lose permissions. If publishing returns an
authentication or permission error, inspect the response and validate
the token and granted permissions using Meta's developer tools.

### 4. Configure Instagram publishing (optional)

Instagram publishing generally requires a Professional account and a
supported Meta setup. Image publishing commonly involves creating a
media container and then publishing it. Confirm that the workflow
implements both steps, uses the correct Instagram Business Account ID,
and has the required permissions before enabling this feature.

### 5. Configure the trigger

The exact steps depend on the trigger node in your imported workflow:

-   **Webhook trigger:** use the Test URL while testing and the
    Production URL after activating the workflow. Send input in the
    format expected by the workflow, for example:

    ``` json
    {
      "topic": "AI workshop for students"
    }
    ```

-   **Manual trigger:** open the workflow and select **Execute
    Workflow** to run a test.

-   **Schedule trigger:** configure the desired schedule, save, and
    activate the workflow.

Do not activate a workflow that can publish publicly until you have
tested it and reviewed the generated content.


## Known limitations

-   **API limits and costs:** Gemini, Leonardo AI, and Meta may apply
    quotas, rate limits, or usage charges.
-   **Token expiration:** Meta tokens can expire or become invalid when
    permissions or account access change.
-   **Asynchronous image generation:** Leonardo AI may take time to
    finish; the wait duration and polling logic may need adjustment.
-   **Variable content quality:** AI-generated captions, hashtags, and
    images should be reviewed before publication.
-   **Publishing scope:** Generating copy for a platform does not mean
    the workflow automatically publishes to it. Only configured
    publishing nodes perform publishing.
-   **Basic error handling:** Depending on the exported workflow,
    retries, notifications, and centralized logs may need to be added.
-   **Public posting risk:** A successful run may publish content to a
    real account. Test carefully and use a review step when appropriate.

## Possible improvements

-   Add retry logic with backoff for transient API errors.
-   Log each run's topic, generated content, image URL, publishing
    status, and error message to a spreadsheet or database.
-   Send a notification when a run fails.
-   Add a human approval step before publishing.
-   Add content validation and duplicate-post checks.
-   Add publishing nodes for additional platforms only after configuring
    their official APIs and permissions.
-   Use n8n credentials or a dedicated secret manager for all tokens and
    API keys.


## Disclaimer

This project is provided for learning and portfolio purposes. It is not
affiliated with or endorsed by Google, Leonardo AI, Meta, Facebook,
Instagram, LinkedIn, YouTube, or n8n. You are responsible for complying
with applicable platform terms, API policies, privacy requirements, and
content rules.
