

# BrieflyAI-clean-intuitive-human-friendly



name: generate profile metrics

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  metrics:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build metrics
        uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.PAT }} # classic PAT with repo, read:org
          user: wable-j
          template: classic
          base: header, activity, community, repositories, metadata
          repositories_affiliations: owner, collaborator, organization_member
          config_timezone: America/New_York
          plugin_followup: yes
          plugin_languages: yes
          plugin_languages_ignored: html, css
          plugin_languages_limit: 8
          plugin_achievements: yes
          plugin_lines: yes
          filename: metrics.svg

      - name: Commit metrics.svg
        run: |
          mv metrics.svg ./metrics.svg
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add metrics.svg
          git commit -m "chore: update metrics" || echo "no changes"
          git push || echo "no changes"
