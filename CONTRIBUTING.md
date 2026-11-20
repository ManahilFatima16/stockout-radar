# Contributing to Stockout Radar

This is a team project (3 members). These rules keep the repository clean and stop us overwriting each other's work.

## Branches

| Branch | Purpose | Who can push |
|---|---|---|
| `main` | Stable, submission-ready code only | Nobody directly. Only through a pull request from `develop` |
| `develop` | Integration branch where finished features are combined | Nobody directly. Only through pull requests from feature branches |
| `feature/<name>` | One task each (for example `feature/streamlit-app`) | The person working on that task |
| `fix/<name>` | Bug fixes | The person fixing it |
| `docs/<name>` | Report, README, slides | The person writing it |

Branch names use lowercase letters and hyphens.

## Daily workflow

1. Get the latest code:
   ```
   git checkout develop
   git pull origin develop
   ```
2. Create your branch from `develop`:
   ```
   git checkout -b feature/your-task
   ```
3. Work, then commit small and often:
   ```
   git add <files>
   git commit -m "feat: add permutation importance plot"
   ```
4. Push your branch:
   ```
   git push -u origin feature/your-task
   ```
5. On GitHub, open a **pull request into `develop`**. Fill in the template. At least **one teammate reviews** it before merging.
6. After the merge, delete the feature branch and pull `develop` again.
7. When `develop` is stable (all notebooks run, results match the report), open a pull request from `develop` into `main`.

## Commit messages

Start with a type, then a short description in the present tense.

| Type | Use for |
|---|---|
| `feat:` | New feature, notebook step or app function |
| `fix:` | Bug fix |
| `data:` | Cleaning or feature changes |
| `exp:` | Model experiments and tuning |
| `docs:` | README, report, comments |
| `chore:` | Folders, requirements, configuration |

## Notebook rules

Notebooks are JSON files and merge badly.

- **One owner per notebook.** Only the owner edits it. Ask the owner (or open a pull request to them) for changes.
- Pull before you start, push when you stop.
- Run **Restart and Run All** before committing, so the saved outputs match the code.
- Keep shared functions in `src/` instead of copying code between notebooks.
- Never commit large data files. The `data/` folder is ignored on purpose.

| Notebook | Owner (suggested) |
|---|---|
| `01_cleaning`, `02_aggregation`, `03_eda` | Shahzaib |
| `04_features`, `05_split_baselines`, `06_models` | Qasim |
| Evaluation, `app/` | Manahil |

## Reproducibility rules (from the course guidelines)

- Fix random seeds (`random_state=42`).
- Decide the split before modelling. Never tune on the test set.
- Every number in the report must come from the code and match it.
- If you use an AI tool, note it for the AI Use Disclosure in the report.

## Setup

See the "How to run" section in `README.md`.