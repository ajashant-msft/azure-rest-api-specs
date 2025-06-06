# CI Fix Guide

Short link: [aka.ms/ci-fix]

This guide provides detailed troubleshooting instructions for the [automated validation tooling] checks running on
[Azure REST API specs PR]s submitted to this repo.

If you need help with your specs PR, please first thoroughly read the [aka.ms/azsdk/pr-getting-help] document.

# Table of Contents

- [CI Fix Guide](#ci-fix-guide)
- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [Checks troubleshooting guides](#checks-troubleshooting-guides)
  - [`CredScan`](#credscan)
  - [`PoliCheck`](#policheck)
  - [`SDK Validation *` checks, like `SDK Validation - Go`](#sdk-validation--checks-like-sdk-validation---go)
  - [`SDK Breaking Change Review`](#sdk-breaking-change-review)
  - [`Swagger APIView`](#swagger-apiview)
    - [If an expected APIView was not generated, follow the step below to troubleshoot.](#if-an-expected-apiview-was-not-generated-follow-the-step-below-to-troubleshoot)
    - [Diagnosing APIView failure for SDK Language (not Swagger or TypeSpec)](#diagnosing-apiview-failure-for-sdk-language-not-swagger-or-typespec)
  - [`Swagger ApiDocPreview`](#swagger-apidocpreview)
  - [`Swagger Avocado`](#swagger-avocado)
  - [`Swagger BreakingChange` and `BreakingChange(Cross-Version)`](#swagger-breakingchange-and-breakingchangecross-version)
    - [Run `oad` locally](#run-oad-locally)
  - [`Swagger LintDiff` and `Swagger Lint(RPaaS)`](#swagger-lintdiff-and-swagger-lintrpaas)
  - [`Swagger LintDiff` for TypeSpec: troubleshooting guides](#swagger-lintdiff-for-typespec-troubleshooting-guides)
  - [`Swagger ModelValidation`](#swagger-modelvalidation)
  - [`Swagger PrettierCheck`](#swagger-prettiercheck)
    - [Prettier reference](#prettier-reference)
  - [`Swagger SemanticValidation`](#swagger-semanticvalidation)
  - [Spell Check](#spell-check)
  - [`TypeSpec Validation`](#typespec-validation)
  - [`license/cla`](#licensecla)
- [Suppression Process](#suppression-process)
- [Checks not covered by this guide](#checks-not-covered-by-this-guide)
- [Obsolete checks](#obsolete-checks)

# Prerequisites

Most guides here require for you to have `npm` installed, which you can get by installing [Node.js](https://nodejs.org/en/download).

# Checks troubleshooting guides

## `CredScan`

This check is owned by One Engineering System. See [1ES CredScan] for help.

## `PoliCheck`

This check is owned by One Engineering System. See [1ES PoliCheck] for help.

## `SDK Validation *` checks, like `SDK Validation - Go`

> [!IMPORTANT]
>
> - The `SDK Validation Status` check is a meta check that aggregates the results of all `SDK Validation - {Language}`
    checks and reports a unified status. Re-run any individual `SDK Validation - {Language}` checks will automatically
    trigger a re-run of this meta check.
> - The `SDK Validation *` checks are owned by the Shanghai division of the Azure SDK team,
    not the core Redmond Azure SDK team.
> - For more information, refer to [SDK Validation FAQ](https://aka.ms/azsdk/sdk-automation-faq).

If you have an issue or with any of checks listed in the first column of the table below:

| Check name                        | Owner          | GitHub login                                                  |
|-----------------------------------|----------------| ------------------------------------------------------------- |
| `SDK Validation - Go`            | Chenjie Shi     | [tadelesh](https://github.com/tadelesh)                       |
| `SDK Validation - Java`          | Weidong Xu      | [weidongxu-microsoft](https://github.com/weidongxu-microsoft) |
| `SDK Validation - JS`            | Qiaoqiao Zhang  | [qiaozha](https://github.com/qiaozha)                         |
| `SDK Validation - .NET`          | Wei Hu          | [live1206](https://github.com/live1206)                       |
| `SDK Validation - Python`        | Yuchao Yan      | [msyyc](https://github.com/msyyc)                             |

Do the following:

1. Attempt to diagnose the issue yourself:
    1. Look at the affected PR's `checks` tab for the failing check.
    2. Click on the `View more details on Azure Pipelines.` link from that tab and inspect the devOps logs.
       For example, for `SDK Validation - Go` check look into the `Azure Pipelines/SDK Validation - Go` pipeline run logs.
2. If your investigation denotes this is likely a bug in the check itself and not your PR, reach out
  to the owner of the check per the aforementioned table.

## `SDK Breaking Change Review`

> [!IMPORTANT]
>
> - If your PR is flagged with any label that matches the pattern `BreakingChange-{Language}-Sdk`, the SDK breaking
     changes will be reviewed by SDK reviewers around two business days after the completion of the first two review steps
     in PR review workflow, i.e. REST API breaking change review and ARM review.
> - If you need to suppress the SDK breaking changes, refer to [SDK Suppressions](https://aka.ms/azsdk/sdk-suppression).

If the SDK breaking changes haven't been reviewed after two additional business days, you may reach out to the reviewers:

| Language        | Reviewer        | GitHub login                                                  |
|-----------------|-----------------| ------------------------------------------------------------- |
| `Go`            | Chenjie Shi     | [tadelesh](https://github.com/tadelesh)                       |
| `JS`            | Qiaoqiao Zhang  | [qiaozha](https://github.com/qiaozha)                         |
| `Python`        | Yuchao Yan      | [msyyc](https://github.com/msyyc)                             |

## `Swagger APIView`

Various APIViews are generated as part of the Azure REST API specs PR build. Among these are TypeSpec and Swagger as well as any other language that is being generated in the run. When everything is successful you should see a comment box similar to the picture below showing the APIViews generated for TypeSpec or Swagger, plus all other languages being generated.

![alt text](image-3.png)

### If an expected APIView was not generated, follow the step below to troubleshoot.

- On the CI check click on `details` > `View Azure DevOps build log for more details` to view the devOps logs.
- Investigate the CI job for the language with error. TypeSpec and Swagger APIViews are generated as part of the `AzureRestApiSpecsPipeline` stage in the `TypeSpecAPIView` and `SwaggerAPIView` jobs respectively, while APIViews for other SDK languages are generated in their respective language jobs in the `SDK Automation` stage.
- Ensure that all previous checks in the job are green before proceeding.

### Diagnosing APIView failure for SDK Language (not Swagger or TypeSpec)

1. Check for an unexpected skip of the `Publish SDK APIView Artifact to Pipeline Artifacts` and `Generate SDK APIView` step.
2. Look in `SDK Automation` step to verify that the API token generation completed successfully.
3. Search logs for `Read Temp File`
4. Below `Read Temp File` find the .json object and search within to locate the `apiViewArtifact` property.
5. If not present, the APIView parser for the language failed to generate the `APIView Token Artifacts`.
6. Please contact [APIView Support Teams Channel] for assistance.

## `Swagger ApiDocPreview`

If you see `Swagger ApiDocPreview` check fail with a failure [like this one](https://github.com/Azure/azure-rest-api-specs/pull/24841/checks?check_run_id=15056283615):

| Rule | Message |
|-|-|
| ❌ RestBuild error | "logUrl":"https://apidrop.visualstudio.com/Content%20CI/_build/results?buildId=373646&view=logs&j=fd490c07-0b22-5182-fac9-6d67fe1e939b",<br/>"detail":"Run.ps1 failed with exit code 1 " |

Refer to [troubleshooting REST API documentation](https://eng.ms/docs/products/azure-developer-experience/design/api-docs-troubleshooting).

## `Swagger Avocado`

Moved to https://github.com/Azure/azure-rest-api-specs/wiki/Swagger-Avocado

## `Swagger BreakingChange` and `BreakingChange(Cross-Version)`

See [aka.ms/azsdk/pr-brch-deep](https://aka.ms/azsdk/pr-brch-deep). If you want a quick read, see only [the `summary` section](https://aka.ms/azsdk/pr-brch-deep#summary).

### Run `oad` locally

To repro issues with "breaking changes" checks, you can locally run the tool that powers them: [Azure/openapi-diff](https://github.com/Azure/openapi-diff), aka `oad`:

``` powershell
npm install -g @azure/oad
oad compare <old-spec-path> <new-spec-path>
```

Please see [readme](https://github.com/Azure/openapi-diff/blob/main/README.md) for how to install or run tool in details.
Refer to [Oad Docs](https://github.com/Azure/openapi-diff/tree/main/docs) for detailed description of all oad rules.


## `Swagger LintDiff` and `Swagger Lint(RPaaS)`

The [LintDiff validation tool](https://github.com/Azure/azure-openapi-validator) runs linting rules against specification difference. Two specifications are compared: the specification as it would be when proposed PR is merged, vs the specification as seen before the PR is merged.

Refer to [openapi-authoring-automated-guidelines](https://github.com/Azure/azure-rest-api-specs/blob/master/documentation/openapi-authoring-automated-guidelines.md) for detailed description of all lint rules and how-to-fix guidance.
If that guidance is not enough, please also refer to the [LintDiff rules.md doc](https://github.com/Azure/azure-openapi-validator/blob/main/docs/rules.md). It links to `.md` files related to given error, containing instructions how to fix them.

To reproduce LintDiff failures locally, see [CONTRIBUTING.md / How to locally reproduce a LintDiff failure occurring on a PR](https://github.com/Azure/azure-openapi-validator/blob/main/CONTRIBUTING.md#how-to-locally-reproduce-a-lintdiff-failure-occurring-on-a-pr).

## `Swagger LintDiff` for TypeSpec: troubleshooting guides

https://github.com/Azure/azure-rest-api-specs/wiki/Swagger-LintDiff-for-TypeSpec

## `Swagger ModelValidation`

To repro issues with `Swagger ModelValidation` locally:

``` powershell
npm install -g oav
oav validate-example <openapi-spec-path>
```

Please see [readme](https://github.com/Azure/oav/blob/bd04e228b4181c53769ed88e561dec5212e77253/README.md) for how to install or run tool in details.
Refer to [Semantic and Model Violations Reference](https://github.com/Azure/azure-rest-api-specs/blob/main/documentation/Semantic-and-Model-Violations-Reference.md) for detailed description of validations and how-to-fix guidance.
Refer to [Swagger-Example-Generation](https://github.com/Azure/oav/blob/develop/documentation/example-generation.md) for example automatic generation.

## `Swagger PrettierCheck`

First, ensure you have fulfilled `Prerequisites` as explained above.

To update all the spec files for a given service run the following:

``` powershell
# To fix all the files in the repo run from the root of the repo
cd <local_repo_clone_root>

# OPTIONAL STEP: To fix a particular service OpenAPI spec cd to that directory like
cd specification/contosowidgetmanager

# Install the dependencies to the local 'node_modules' folder.
npm install

# Run 'prettier --check' to verify the problems can be reproduced locally
npx prettier --check **/*.json

# Run 'prettier --write' to fix the problems.
npx prettier --write **/*.json
```

Then please commit and push changes made by prettier.

### Prettier reference

- [`prettier` npm package](https://www.npmjs.com/package/prettier)
- [Source: Swagger-Prettier-Check.ps1](https://github.com/Azure/azure-rest-api-specs/blob/main/eng/scripts/Swagger-Prettier-Check.ps1)
- [Pipeline: Swagger PrettierCheck](https://dev.azure.com/azure-sdk/public/_build?definitionId=6405)

## `Swagger SemanticValidation`

To repro issues with `Swagger SemanticValidation` locally:

``` powershell
npm install -g oav
oav validate-spec <openapi-spec-path>
```

Please see [readme](https://github.com/Azure/oav/blob/bd04e228b4181c53769ed88e561dec5212e77253/README.md) for how to install or run tool in details.
Refer to [Semantic and Model Violations Reference](https://github.com/Azure/azure-rest-api-specs/blob/main/documentation/Semantic-and-Model-Violations-Reference.md) for detailed description of validations and how-to-fix guidance.

## Spell Check

If you receive a spelling failure, first try to fix the spelling in source.

If there are new words that need to be supported for your service, add the word to the override list in the `words` list in
the `specification/<service>/cspell.yaml` file for your service. Specific files can also be overridden using the
`override` field in your service's `cspell.yaml` file (see example below).

If the `specification/<service>/cspell.yaml` file does not exist, create it using the example below and set the `words`
and `overrides` fields to the words that need to be suppressed. The `import` field is *required*.

For example (note the words list is sorted alphabetically):

```yaml
import:
  - ../../cspell.yaml
words:
  - aardvark
  - zoology

# Optional overrides example for words in a specific file
overrides: 
  - filename: '**/specification/contosowidgetmanager/somefile.yaml'
    words:
      - aardwolf
      - zoo
```

Spell checking is *case-insensitive* so you only need to add a word once in lower-case. Try to kept the word list sorted for easier discovery.

For more information see [cspell configuration](https://cspell.org/configuration/).

## `TypeSpec Validation`

https://github.com/Azure/azure-rest-api-specs/wiki/TypeSpec-Validation

## `license/cla`

This check is owned by One Engineering System. See [1ES GitHub inside Microsoft] for help.

# Suppression Process

In case there are validation errors reported against your service that you believe do not apply,
we have a suppression process you can follow to permanently remove these reported errors for your specs.
Refer to the [suppression guide](https://aka.ms/pr-suppressions) for detailed guidance.

# Checks not covered by this guide

If you have an issue with a check that is not covered by this guide and the help at [aka.ms/azsdk/pr-getting-help],
see [aka.ms/azsdk/support].

# Obsolete checks

Following checks have been removed from the validation toolchain as of August 2023.

- API Readiness Check
- Service API Readiness Test
- Traffic validation

[1ES CredScan]: https://eng.ms/docs/cloud-ai-platform/devdiv/one-engineering-system-1es/1es-docs/1es-pipeline-templates/features/sdlanalysis/credscan
[1ES GitHub inside Microsoft]: https://eng.ms/docs/more/github-inside-microsoft/policies/cla
[1ES PoliCheck]: https://eng.ms/docs/cloud-ai-platform/devdiv/one-engineering-system-1es/1es-docs/1es-pipeline-templates/features/sdlanalysis/policheck
[aka.ms/azsdk/pr-getting-help]: https://aka.ms/azsdk/pr-getting-help
[aka.ms/azsdk/support]: https://aka.ms/azsdk/support
[aka.ms/ci-fix]: https://aka.ms/ci-fix
[APIView Support Teams Channel]: https://teams.microsoft.com/l/channel/19%3A3adeba4aa1164f1c889e148b1b3e3ddd%40thread.skype/APIView?groupId=3e17dcb0-4257-4a30-b843-77f47f1d4121&tenantId=72f988bf-86f1-41af-91ab-2d7cd011db47
[automated validation tooling]: https://eng.ms/docs/products/azure-developer-experience/design/api-specs/api-tooling
[Azure REST API specs PR]: https://eng.ms/docs/products/azure-developer-experience/design/api-specs-pr/api-specs-pr
[TypeSpec Discussions Teams channel]: https://teams.microsoft.com/l/channel/19%3A906c1efbbec54dc8949ac736633e6bdf%40thread.skype/TypeSpec%20Discussion%20%F0%9F%90%AE?groupId=3e17dcb0-4257-4a30-b843-77f47f1d4121&tenantId=72f988bf-86f1-41af-91ab-2d7cd011db47
