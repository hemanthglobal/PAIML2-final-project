# Code: Subject-Independent Recognition of Gym Exercises from Wearable Sensors

Hemanth Kumar Reddyvari (a3185100), PAIML2 final submission.

## What each file does

| File | What it does | Writes |
|---|---|---|
| `AIML Workout Final.ipynb` | Main pipeline and the milestone experiments: data download, windowing, the four models (majority, Keras feature network, 1D CNN, LSTM), random split and LOSO evaluation, Experiment 1 (wrist CNN), Experiment 2 (model ladder on the wrist), Experiment 3 (leg and pocket), results tables and the report figures. | `results_final/`, `figures_final/` |
| `AIML Workout Extensions.ipynb` | Experiments after the milestone: A (same input for both models), B (blocked random split), C (rotation augmentation), D (sensor fusion alignment check), E (leg-to-pocket transfer learning), and section 11, the application evaluated on all ten participants. | `results_ext/` (app results in `results_ext/app/`) |
| `SetLog Demo.ipynb` | Starts SetLog, the web interface of the workout logger, and prints a public link. Used for the live demonstration. | nothing |
| `App.zip` | The workout logger (unzip to get `App/`): `workout_app.py` (command-line app), `ui_app.py` (SetLog web page), `models/` (CNN trained with participant 6 held out, and the wrist LOSO results used to flag weak exercises), `output/` (saved app results) and `README.md` (all app commands). | `App/output/` |

The saved results used in the report are in `../3 Results/`, so nothing needs to be retrained to check the report.

## Data

RecGym from Kaggle: `zhaxidelebsz/10-gym-exercises-with-615-abstracted-features` (111 MB, not included). The notebooks download it with the Kaggle API, so you need a Kaggle API token (`kaggle.json`, from Kaggle > Settings > API > Create New Token).

## How to run (Google Colab)

1. **Main notebook.** Open `AIML Workout Final.ipynb` in Colab, choose Runtime > Change runtime type > T4 GPU, and run the sections in order. Upload `kaggle.json` when asked. By default (`var_usedrive_bool = False`) everything stays in the Colab session and section 11 downloads the results as a zip; set it to `True` to keep the window cache and results on Google Drive. The Results and Visuals sections only read saved files.
2. **Extensions notebook.** Open `AIML Workout Extensions.ipynb` the same way. It uses Google Drive by default, so it can reuse the cache and compare with `results_final` from step 1. Section 11 asks you to upload `App.zip`.
3. **SetLog demo.** Open `SetLog Demo.ipynb` with a T4 GPU. Run cell 1 (upload `kaggle.json`), then cell 2 (upload `App.zip`). Open the public link it prints, choose a participant and session, and tap **Log my workout**. The first log takes about 30 seconds.

## Running the app without Colab

Unzip `App.zip`. It needs Python 3 with `numpy`, `pandas`, `torch`, `scikit-learn` and `matplotlib` (and `gradio` for SetLog). From inside `App/`:

```bash
python workout_app.py --data <path to the Kaggle zip> log --subject 6 --session 2
```

```bash
python workout_app.py --data <path to the Kaggle zip> evaluate-all
```

The first logs one session with the model that never saw participant 6. The second trains one held-out CNN per participant and logs all 50 sessions. See `App/README.md` for every option.

## Notes

- GPU training is not exactly repeatable: reruns can differ by up to about 0.02 macro F1. The report uses the saved results in `../3 Results/`.
- Every notebook cell was run on Google Colab with a T4 GPU.
