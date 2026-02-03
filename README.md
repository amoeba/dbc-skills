# dbc-skills

Agent [skills](https://agentskills.io) for [dbc](https://docs.columnar.tech/dbc) and related technologies.

## Installation

The simplest way to install these skills to your agent of choice is with the [skills](https://www.npmjs.com/package/skills) package:

`npx skills add columnar-tech/dbc-skills`

## Skills

- dbc: Skill for using [dbc](https://docs.columnar.tech/dbc)

## Evals

Manual evals that we run to verify the skill improves the agent's performance. We check that the skill was invoked and that the result was correct.

### Methodology

- Model: Sonnet 4.5
- Agent: Claude Code

Ran each prompt with `claude $PROMPT`. Auto accepted any obvious tool use but cancelled if the agent presented options.
Ran each test three times and recorded pass/fail.

### dbc

- Installed Skills: `dbc`
- Setup: empty dir, dbc is not on system, no drivers installed with dbc, uv and pipx available

#### Prompt: "install dbc"

| Without | With: Skill Invoked? | With: Task Successful? |
| ------- | -------------------- | ---------------------- |
| 1/3     | 3/3                  | 3/3                    |

Notes: Without the skill, only sometimes will correctly guess we want to install the dbc PyPi package

#### Prompt: "install the sqlite ADBC driver"

Criterion: Should use dbc to install the sqlite driver

| Without | With: Skill Invoked? | With: Task Successful? |
| ------- | -------------------- | ---------------------- |
| 0/3     | 3/3                  | 3/3                    |

Notes: Without the skill, Claude will always install the PyPI package

#### Prompt:  "uninstall the sqlite ADBC driver"

Setup: dbc is available, sqlite is installed

| Without | With: Skill Invoked? | With: Task Successful? |
| ------- | -------------------- | ---------------------- |
| 0/3     | 3/3                  | 3/3                    |

#### Prompt: "create a dbc driver list with the sqlite and postgresql ADBC drivers"

| Without | With: Skill Invoked? | With: Task Successful? |
| ------- | -------------------- | ---------------------- |
| 0/3     | 3/3                  | 3/3                    |
