# CircleCI Orb For Sous-chefs

This repository contains the [Sous-Chefs orb](https://circleci.com/orbs/registry/orb/sous-chefs/kitchen)

## View Source

```bash
circleci orb source sous-chefs/kitchen@1.1.2
```

## Usage

Include the orb and give it a namespace, in this case `kitchen`

Then use the orb in a workflow.

Orbs can be used with semantic version pins - see [CircleCI documentation](https://circleci.com/docs/2.0/creating-orbs/#semantic-versioning-in-orbs)

### Version 1.1.2

```yaml
---
lint_and_unit: &lint_and_unit
  - delivery
  - danger
  - lint-yaml
  - lint-markdown

version: 2.1
orbs:
  kitchen: sous-chefs/kitchen@2

workflows:
  kitchen:
    jobs:
      # Lint and Unit Test
      - kitchen/yamllint:
          name: lint-yaml
      - kitchen/mdlint:
          name: lint-markdown
      - kitchen/danger:
          name: danger
          context: Danger
      - kitchen/delivery:
          name: delivery

      # Run multiple platforms at once
      - kitchen/dokken-single:
          name: default
          suite: default
          requires:
            *lint_and_unit

      # Run a single platform
      - kitchen/dokken-single:
          suite: default-centos-7
          name: default-centos-7
          requires:
            *lint_and_unit
```
