# Inheritance Expert System

A rule-based expert system written in Python that calculates inheritance shares (*Fara'id*) under Islamic law, based on the family members the deceased left behind. It is built with [Experta](https://github.com/nilp0inter/experta), a Python library for building expert systems (a port of CLIPS).

The program interviews the user through the command line, builds a family tree from the answers, and then fires the matching inheritance rule to print each heir's share as a percentage of the estate.

> **Disclaimer:** This project is for educational and academic purposes only. It implements a simplified subset of inheritance rules and is **not** a substitute for a qualified Islamic scholar or a legal professional.

---

## Features

- Interactive question-and-answer flow driven by a forward-chaining rule engine
- Models a family tree with a `Person` class (gender, parents, spouse, sons, daughters, alive/deceased status, allotment)
- Handles several inheritance scenarios:
  - Deceased has sons and daughters
  - Deceased has daughters only
  - Father is deceased, with the grandfather and/or siblings as heirs
  - No father, grandfather or siblings, so uncles and aunts inherit
- Supports **representation of deceased children**: if a son or daughter has died, their living children receive that child's share
- Applies fixed shares for the father, mother and spouse (e.g. 1/6, 1/8, 1/4, 1/2) depending on whether descendants exist
- Applies the 2:1 male-to-female ratio between sons and daughters
- Prints each heir's name and share (out of 100)

---

## How It Works

The system is split into two parts:

1. **Data model**: the `Person` class and helper functions (`has_children`, `children_count`) describe the family members.
2. **Knowledge base**: the `FamilyTree` class extends `KnowledgeEngine`. Its rules fall into two groups:
   - **Data-gathering rules** (`gender`, `has_father`, `has_mother`, `has_spouse`, `has_daughters`, `has_sons`, `has_grandfather`, `has_siblings`, `has_uncleOrAnte`) ask the user questions and declare facts.
   - **Distribution rules** (`inheritance_distribution1` to `inheritance_distribution5`) fire once enough facts are known and compute the shares for the relevant scenario.

Facts such as `has_father`, `has_sons` or `has_grandfather` act as the working memory that decides which distribution rule is activated.

| Rule | Scenario |
|------|----------|
| `inheritance_distribution1` | The deceased has sons (and possibly daughters) |
| `inheritance_distribution2` | Father is alive, daughters only (no sons) |
| `inheritance_distribution3` | Father deceased, no sons, siblings exist |
| `inheritance_distribution4` | Father deceased, grandfather alive, no siblings |
| `inheritance_distribution5` | No father, grandfather or siblings, so uncles and aunts inherit |

---

## Requirements

- Python 3.x
- [Experta](https://pypi.org/project/experta/)

```bash
pip install experta
```

> **Note:** Experta depends on `frozendict` and uses older Python APIs, so it may not import correctly on very recent Python versions (3.10+). If you see an error such as `AttributeError: module 'collections' has no attribute 'Mapping'`, use Python 3.9 or lower, or install a maintained fork of Experta.

---

## Usage

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
python main.py
```

The program will then ask a series of questions in the terminal, for example:

```
what is you gender
what is your fathers name?
Is he alive? (yes/no):
what is your mothers name?
Is she alive? (yes/no):
did you have a spouse?
How many daughters do you have?
How many sons do you have?
...
```

At the end it prints each heir with their share of the inheritance:

```
<father name> : 16.666666666666668
<mother name> : 16.666666666666668
<spouse name> : 12.5
...
```

### Input conventions

- Answer alive/deceased questions with `yes` or `no`.
- Answer gender questions with `male` or `female` (lowercase).
- Answer counts (children, siblings, uncles, aunts) with whole numbers.

---

## Project Structure

```
.
├── main.py      # Person model, Experta rules and program entry point
└── README.md
```

---

## Known Limitations

This is a simplified implementation, and some cases are not covered or are handled approximately:

- Not all heir categories are supported (for example maternal siblings, full vs. half siblings, and *'Awl* / *Radd* adjustments)
- Input validation is minimal and expects exact lowercase answers
- The code relies on a global `Person` object and repeats a lot of logic between distribution rules, so it could be refactored
- Shares are printed as raw floating-point numbers without rounding

Contributions that address any of these points are welcome.

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-improvement`
3. Commit your changes: `git commit -m "Add my improvement"`
4. Push the branch: `git push origin feature/my-improvement`
5. Open a Pull Request

---

## License

Add a license of your choice (for example [MIT](https://choosealicense.com/licenses/mit/)) before publishing.
