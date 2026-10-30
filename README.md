# SMP 28-Day Fitness Data Analysis

A small data analysis project that uses **Pandas** and **NumPy** to explore 28 days of fitness tracking data for an SMP member. It looks at daily steps, sleep, water intake, bench press performance, and the eating protocol followed (OMAD vs 2MAD), then prints a formatted report.

## Questions This Analysis Answers

- How consistently are the 10,000-step goals being hit?
- Is bench press improving over the 4 weeks?
- Do OMAD and 2MAD days differ in steps, sleep, or strength?
- Do more steps go together with a higher bench press?
- Which days were the best step days?

## Dataset

28 daily records (4 weeks), one row per day.

| Column | Type | Description |
|---|---|---|
| `day` | int | Day number, 1 to 28 |
| `steps` | int | Steps walked that day |
| `sleep_hr` | float | Hours of sleep |
| `water` | int | Water intake (glasses) |
| `protocol` | str | Eating protocol: `OMAD` or `2MAD` |
| `bench_kg` | int | Bench press weight in kg |

The protocol column follows a repeating 7-day weekly pattern (`["OMAD", "2MAD", "OMAD", "OMAD", "2MAD", "OMAD", "OMAD"] * 4`). The data is sample data entered directly in the script, not loaded from a file.

## Requirements

- Python 3.9+
- `pandas`
- `numpy`

```bash
pip install pandas numpy
```

> **Running in the browser (Pyodide)?** Load the packages first:
> ```python
> import pyodide_js
> await pyodide_js.loadPackage("micropip")
> import micropip
> await micropip.install(["numpy", "pandas"])
> ```

## How to Run

```bash
python data_analysis_project.py
```

## What the Script Does

1. **Builds the dataset** from Python lists into a Pandas `DataFrame`, and also keeps `steps` and `bench` as NumPy arrays for fast array math.
2. **Overall metrics**: total and average steps, number and percentage of days hitting 10k, average sleep, bench press range, and the week 1 to week 4 bench trend.
3. **Weekly breakdown**: loops through 4 slices of 7 days (`steps[wk*7:(wk+1)*7]`) to report average steps, 10k days, and average bench per week.
4. **Protocol comparison**: `df.groupby("protocol")` to compare average steps, sleep, and bench between OMAD and 2MAD days.
5. **Top days**: `df.nlargest(3, "steps")` to list the three highest step days.

### Techniques Used

| Technique | Where it's used |
|---|---|
| Boolean masking (`steps >= 10000`) | Counting goal-hit days |
| `.mean()` on a boolean array | Percentage of days hitting the goal |
| Array slicing (`bench[:7]`, `bench[-7:]`) | Week 1 vs week 4 trend |
| `groupby()` + `.mean()` | Protocol comparison |
| `.nlargest()` | Top 3 step days |
| `np.corrcoef()` | Correlation between steps and bench press |
| f-string alignment (`:<25`, `:>12,.0f`) | Formatted report output |

## Sample Output

```
======================================================
  SMP 28-DAY FITNESS ANALYSIS REPORT
======================================================

  OVERALL METRICS
  Days tracked:             28
  Total steps:              268,700
  Avg daily steps:          9,596
  Days hitting 10k:         12/28  (43%)
  Avg sleep:                7.6 hrs
  Bench press range:        78 to 88 kg
  Bench press trend:        +1.7 kg (wk1 to wk4)

  WEEKLY BREAKDOWN
  Week        Avg Steps  10k Days   Avg Bench
  --------------------------------------------
  Week 1          9,529         3/7       81.7 kg
  Week 2          9,657         3/7       82.6 kg
  Week 3          9,786         3/7       84.0 kg
  Week 4          9,414         3/7       83.4 kg

  PROTOCOL COMPARISON
  2MAD: avg steps=9,112, sleep=8.4h, bench=82.2kg
  OMAD: avg steps=9,790, sleep=7.2h, bench=83.2kg

  TOP 3 STEP DAYS
  Day 18: 11,500 steps  (OMAD)
  Day 11: 11,200 steps  (OMAD)
  Day  4: 11,000 steps  (OMAD)
```

## Key Findings

- **Step goal:** 12 of 28 days (43%) reached 10,000 steps, with an average of 9,596 per day.
- **Strength:** bench press ranged from 78 to 88 kg, and the week 4 average was 1.7 kg higher than week 1. Week 3 was the peak (84.0 kg).
- **Protocols:** OMAD days averaged more steps (9,790 vs 9,112) and a slightly higher bench (83.2 vs 82.2 kg). 2MAD days averaged more sleep (8.4 vs 7.2 hours).
- **Steps vs bench:** the correlation is about 0.88 (strong positive). Sleep vs bench is about 0.09 (essentially none).
- **Best step days:** days 18, 11, and 4, all OMAD.

## Limitations

- **Small sample:** 28 data points for one person. Correlations at this size are noisy and can shift a lot with a few days.
- **Correlation is not causation:** more steps coinciding with a heavier bench does not show that one causes the other. Sleep, recovery, or training schedule could be driving both.
- **Unequal groups:** OMAD has 20 days and 2MAD has 8, so the protocol averages are not equally reliable.
- **Weekly pattern:** the protocol column repeats the same weekly cycle, so protocol is tied to the day of the week, which can confound the comparison.

## Possible Next Steps

- Load real data from a CSV with `pd.read_csv()` instead of hardcoding lists.
- Add a `cold_shower` column and compare its effect on sleep or bench.
- Plot steps and bench over time with Matplotlib.
- Extend the tracker to multiple SMP members and compare them with `groupby("member")`.

## Project Structure

```
.
├── data_analysis_project.py   # the full analysis script
└── README.md    
```
