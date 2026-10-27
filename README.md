# Spaceship Titanic — Orange model comparison and stacking

This project predicts whether a passenger was transported to another dimension in Kaggle's [Spaceship Titanic competition](https://www.kaggle.com/competitions/spaceship-titanic). It is a binary classification problem, so I compared models using classification accuracy (CA) in Orange and checked the final submission on Kaggle.

## Result

| Submission | Kaggle public score | Metric |
| --- | ---: | --- |
| `spaceshipv3.csv` | **0.80173** | Accuracy (higher is better) |

The [submission screenshot](kaggle-score.png) records this score. The CSV contains 4,277 unique passengers and the required `PassengerId` and `Transported` columns.

## What I did

1. Inspected the passenger data and compared several classification models in Orange.
2. Added features with Orange's **Formula** widget:
   - `Deck` and `Side` from `Cabin`, to give the models parts of the cabin location.
   - `Total spend` from the five spending columns, to summarize onboard spending.
   - `NoSpend`, to mark passengers with zero recorded spending.
3. Compared models with **Test & Score**. I focused on CA because Kaggle scores this competition by accuracy.
4. Combined **Neural Network, Gradient Boosting, and Random Forest** with Orange's **Stacking** widget.
5. Used the stacked model to predict the competition test set and exported [`spaceshipv3.csv`](spaceshipv3.csv).

The final V3 path in the saved Orange workflow trains the stack on the **80% sample** from the Data Sampler widget. I did not retrain that submitted model on all labeled rows. The score above is the actual Kaggle result for the included CSV, not a cross-validation score.

## Files

- [`Space Titanic.ows`](Space%20Titanic.ows): Orange workflow, including the comparison and final stacking branches.
- [`spaceshipv3.csv`](spaceshipv3.csv): submitted predictions.
- [`kaggle-score.png`](kaggle-score.png): screenshot of the Kaggle public score.

## Open the workflow

Download `train.csv` and `test.csv` from the [competition data page](https://www.kaggle.com/competitions/spaceship-titanic/data), then open the `.ows` file in Orange. Reconnect the CSV File Import widgets to your local files. The published workflow has generic file paths; Kaggle's raw data is not included in this repository.

## What I learned

Feature engineering and model comparison mattered more than choosing a complex model immediately. Stacking let me combine models that made different mistakes. I also learned to separate Orange's validation score from the score of the CSV actually submitted to Kaggle.

**Limitation:** The submission was trained from an 80% sample, and the workflow depends on Orange's saved preprocessing settings. To reproduce the submission, check the Formula, Select Columns, and Data Sampler settings before exporting a new CSV.
