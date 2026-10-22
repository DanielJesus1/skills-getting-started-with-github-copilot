# Copilot instructions

## Project structure

This repository combines a GitHub Skills exercise with the Mergington High School activities web app. The FastAPI app is in `src/app.py`; it owns the in-memory `activities` mapping, exposes the API, and serves the UI files from `src/static/`. The browser code in `src/static/app.js` reads and updates that API, while `index.html` and `styles.css` define the page and presentation. Keep API data changes and their frontend use consistent.

The exercise content lives in `.github/steps/`, and `.github/workflows/` validates each exercise step and posts the next one. Those checks intentionally depend on specific branches, changed file paths, and—in some steps—key phrases or file existence. Preserve those expectations when editing exercise content or workflows.

## Setup and commands

Install the dependencies from the repository root:

```sh
python -m pip install -r requirements.txt
```

Run the app from the repository root:

```sh
uvicorn src.app:app --reload --reload-include 'src/static/*'
```

Open `http://localhost:8000/` for the UI and `http://localhost:8000/docs` for the API documentation.

`pytest.ini` configures the repository root on the Python path. The current requirements do not include pytest and there are no tests yet; once pytest and tests are added, run the full suite with:

```sh
python -m pytest
```

Run one test module or one test by node ID with:

```sh
python -m pytest tests/test_app.py
python -m pytest tests/test_app.py::test_example
```

There is no configured build or lint command in the repository.

## Code and exercise conventions

- Activity names are the keys in the in-memory `activities` dictionary; each value currently carries its description, schedule, participant limit, and participant email list. The data resets when the process restarts.
- The API uses `/activities` to return that mapping and `/activities/{activity_name}/signup` to sign up by email. The frontend calls these same routes; encode user-provided values when building request URLs.
- The UI is static content mounted at `/static`, with `/` redirecting to `/static/index.html`. Keep browser assets together under `src/static/`.
- Workflow step checks are part of the learning experience, not a general app test suite. For example, later steps check changes to particular `src/` paths, require `pytest` in `requirements.txt`, and look for a `tests/` directory. Consult the corresponding workflow before changing a step's expected result.
