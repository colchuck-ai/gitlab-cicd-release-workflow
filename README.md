# gitlab-cicd-release-workflow

The purpose of this [gitlab cicd component](https://docs.gitlab.com/ci/components/) project is to publish components that can be used in the context of a deployment workflow to control when jobs are run.

It's recommended to use these components in conjunction with semantic release.

## Contents

- [gitlab-cicd-release-workflow](#gitlab-cicd-release-workflow)
  - [Contents](#contents)
  - [Components](#components)
    - [`release-workflow` Component](#release-workflow-component)
      - [`release-workflow` Component Usage](#release-workflow-component-usage)
  - [Disclaimer](#disclaimer)

## Components

### `release-workflow` Component

This component contains a set of `workflow` `rules` and two hidden jobs.

The `workflow` `rules` allow the creation of merge request, tag, and branch pipelines, provided the branch isn't the source branch for an open merge request.

The `.release-rules` hidden job contains `rules` that run jobs in branch pipelines if the branch is protected.

The `.publish-rules` hidden job contains `rules` that run jobs in tag pipelines if the tag is a semantic version and protected.

#### `release-workflow` Component Usage

```yml
include:
  - component: gitlab.example.com/engineering/dev-tools/gitlab-cicd-components/gitlab-cicd-release-workflow/release-workflow@1.0.0

stages:
  - test
  - release
  - publish

test:
  stage: test
  script:
    - echo "testing succeeded!"

release:
  image: proxy.docker.example.com/node:21
  stage: release
  extends:
    - .release-rules
  script:
    - npm install
    - npx semantic-release

publish:
  stage: publish
  extends:
    - .publish-rules
  script:
    - echo "publish succeeded!"
```

## Disclaimer

We built this because we needed it. We welcome contributions. If you want to consume the artifacts released by this project, do so with the same consideration you would take when consuming an open source component.
