# Godot 4 Project Template

[![Build QA](https://github.com/shapeshiftingrabbits/godot-4-project-template/actions/workflows/build_qa.yml/badge.svg)](https://github.com/shapeshiftingrabbits/godot-4-project-template/actions/workflows/build_qa.yml)

[![Release to production](https://github.com/shapeshiftingrabbits/godot-4-project-template/actions/workflows/release_production.yml/badge.svg)](https://github.com/shapeshiftingrabbits/godot-4-project-template/actions/workflows/release_production.yml)

## GitHub Action Workflows

### Intentions and processes

- We want to be able to test builds before merging code changes to `main`
  - We generate downloadable artifacts when creating a PR
  - We generate further artifacts for further changes in the PR
- We want to be able to test builds after merging code changes to `main`
  - We this ability call this 'QA'
  - We generate downloadable artifacts after merging to `main`, with debug mode on
  - We tag commits to `main` with the 'QA' build job number, so we are able to reference a commit back to a specific 'QA' build job and its specific artifacts
- We want to be able to release the game from specific commits on `main` we are happy with
  1. We manually publish a GitHub release with a new version number
     - This version number will be publicly visible
     - The version number progression does not necessarily follow semantic versioning
  1. We generate artifacts, with debug mode off
  1. We upload the artifacts to Itch.io
  1. We tag the commit used to create the release with the 'Release' build job number, so we are able to reference a commit back to a specific 'Release' build job and its specific artifacts

### Diagram

```mermaid
flowchart TB
    classDef todo stroke:#f00
    classDef in_progress stroke:#770

    github_release_trigger{{"GitHub release published"}}
    release_production_all["Release builds of all presets to production channels"]
    release_production_one["Release build of one preset to its production channel"]

    release_qa_all_manual_trigger{{"Manual trigger"}}:::todo
    release_qa_all["Build all presets"]

    pull_request_trigger{{"Pull Request created or pushed to"}}
    main_branch_trigger{{"Push to main branch"}}
    build_all["Build all presets"]

    build_manual_trigger{{"Manual trigger"}}:::todo
    build_one["Build one preset"]

    release_production_all --> release_production_one -- debug: false --> build_one
    release_qa_all -- debug: true --> build_one

    github_release_trigger --> release_production_all
    main_branch_trigger --> release_qa_all
    release_qa_all_manual_trigger --> release_qa_all
    pull_request_trigger --> build_all
    build_all -- debug: true --> build_one
    build_manual_trigger --> build_one
```
