# XHS Automation System

This project now includes a full Xiaohongshu automation system under `xhs_toolkit/` with OpenClaw-style skills.

## Structure

- `xhs_toolkit/`: shared Python runtime, clients, services, and CLI modules
- `skills/xhs-*`: OpenClaw-style skill folders with `SKILL.md`, `skill.yaml`, and `scripts/main.sh`
- `config/xhs_jobs.yaml`: scheduler job definitions
- `requirements-xhs.txt`: optional dependency set for full online/runtime behavior

## Core CLI Commands

Run from repository root:

```bash
./scripts/xhs-hot notes --category food --limit 10
./scripts/xhs-hot topics
./scripts/xhs-hot keywords

./scripts/xhs-ai title --topic "mask review"
./scripts/xhs-ai content --topic "mask review" --keywords skincare,repair
./scripts/xhs-ai full --topic "mask review" --category beauty

./scripts/xhs-publisher login --cookies "a=b; c=d"
./scripts/xhs-publisher publish --title "demo" --content "text" --images ./data/demo.svg --tags "#demo"

./scripts/xhs-data profile
./scripts/xhs-analytics performance --days 7
./scripts/xhs-finder notes --keyword "commute outfit" --category fashion
```

## End-to-End Pipeline

```bash
./scripts/xhs-system --topic "desk setup" --category digital
./scripts/xhs-system --topic "desk setup" --category digital --publish
./scripts/xhs-system --topic "desk setup" --category digital --schedule-at 2026-02-14T08:00:00
```

## Scheduler

```bash
./scripts/xhs-scheduler list
./scripts/xhs-scheduler run-due
```

## Tests

```bash
python3 -m unittest discover -s tests/xhs_toolkit -v
```
