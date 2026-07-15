# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a small, standalone data-viz script repo, not an installable package — there is no `requirements.txt`, `pyproject.toml`, test suite, or build tooling. `build_ppt.py` is the only source file; everything else is data or generated output.

## Running

```bash
python build_ppt.py
```

Requires `matplotlib` and `python-pptx` (`pip install matplotlib python-pptx`). Also depends on the `Meiryo` font being available (set via `plt.rcParams['font.family']`) for Japanese chart labels — on non-Windows systems without Meiryo, chart text will fall back to a default font or fail to render Japanese characters correctly.

There is no lint or test command configured in this repo.

## Architecture

`build_ppt.py` runs top to bottom as a single script with no functions/classes beyond a couple of small helpers, in two phases:

1. **Data + charts**: reads `iris.csv` with the stdlib `csv` module (no pandas), computes per-variety and overall stats manually, and renders three charts with matplotlib, saved as `chart_bar.png`, `chart_scatter.png`, `chart_box.png`.
2. **Slide deck**: builds `iris_analysis.pptx` with `python-pptx`, using two local helpers (`add_title_bar`, `add_bullets`) for consistent slide styling, and embeds the three PNGs generated in phase 1.

The script always reads `iris.csv` and writes/overwrites the three `chart_*.png` files and `iris_analysis.pptx` in the working directory — there's no CLI args or config; edit constants at the top of the file (`COLS`, `COL_JP`, `COLORS`) to change what's plotted.

## Subagents

`.claude/agents/test-runner.md` defines a `test-runner` subagent for running this project's test suite. Since no test suite exists yet, it currently has nothing to run.
