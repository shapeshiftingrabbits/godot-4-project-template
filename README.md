# Godot Project Template

[![Build QA](https://github.com/shapeshiftingrabbits/godot-project-template/actions/workflows/build_qa.yml/badge.svg)](https://github.com/shapeshiftingrabbits/godot-project-template/actions/workflows/build_qa.yml)

[![Release to production](https://github.com/shapeshiftingrabbits/godot-project-template/actions/workflows/release_production.yml/badge.svg)](https://github.com/shapeshiftingrabbits/godot-project-template/actions/workflows/release_production.yml)

## GitHub Action Workflows

### Intentions

- Be able to test builds before merging code changes to `main`
  - Create artifacts for each platform when creating a PR
  - Create further artifacts for further pushes to the PR
- Be able to test builds after merging code changes to `main`
  - Create artifacts after merging to `main`
  - Tag merge commits with build job number to be able to track job that holds artifacts
- Be able to release game from commits on `main` we are happy with
  1. Tag commit with public version number (manual)
  2. Create artifacts for each platform (with debug mode off)
  3. Upload artifacts to Itch.io
  4. Tag merge commits with build job number to be able to track job that generated and uploaded artifact

### Diagram

```mermaid
flowchart TB
    classDef todo stroke:#f00
    classDef in_progress stroke:#770

    tag_push{{"Git tag 'v*' pushed"}}
    release_production_all["Release builds of all presets to production channels"]
    release_production_one["Release build of one preset to its production channel"]

    release_qa_all_manual_trigger{{"Manual trigger"}}:::todo
    release_qa_all["Release builds of all presets to test channels"]

    release_qa_one_manual_trigger{{"Manual trigger"}}:::todo
    release_qa_one["Release build of one preset to its test channel"]

    pull_request_trigger{{"Pull Request created or pushed to"}}
    develop_branch_trigger{{"Push to develop branch"}}
    build_all["Build all presets"]

    build_manual_trigger{{"Manual trigger"}}:::todo
    build_one["Build one preset"]

    release_production_all --> release_production_one -- debug: false --> build_one
    release_qa_all --> release_qa_one -- debug: true --> build_one

    tag_push --> release_production_all
    develop_branch_trigger --> release_qa_all
    release_qa_all_manual_trigger --> release_qa_all
    release_qa_one_manual_trigger --> release_qa_one
    pull_request_trigger --> build_all
    build_all -- debug: true --> build_one
    build_manual_trigger --> build_one
```
