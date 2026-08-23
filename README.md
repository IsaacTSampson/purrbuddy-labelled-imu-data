# Purrbuddy — Labelled Cat IMU Dataset

Accelerometer and gyroscope recordings from a collar-mounted IMU worn by a domestic cat, hand-labelled against synchronised video footage.

Five sessions, **2h 49m of recording** containing **2h 13m of labelled behaviour** across 1,051,305 samples — 12 MB total.


## Files

| File | Recorded | Rows | Duration | Labelled | Coverage | Size |
|---|---|---:|---:|---:|---:|---:|
| `archi_1.parquet` | 2025-02-18 | 290,877 | 46.3 min | **32.2 min** | 69.4% | 3.0 MB |
| `archi_2.parquet` | 2025-02-22 | 191,750 | 30.5 min | **18.9 min** | 61.8% | 2.4 MB |
| `archi_3.parquet` | 2025-02-23 | 193,727 | 30.9 min | **23.5 min** | 76.1% | 2.3 MB |
| `archi_4.parquet` | 2025-05-25 (afternoon) | 181,988 | 30.0 min | **28.5 min** | 95.0% | 1.9 MB |
| `archi_5.parquet` | 2025-05-25 (evening) | 192,963 | 30.8 min | **30.2 min** | 98.1% | 2.5 MB |
| **Total** | | **1,051,305** | **169 min** | **133 min** | **79.0%** | **12.0 MB** |

## Reading the data

```python
import pandas as pd

df = pd.read_parquet("archi_1.parquet")
df["time"] = pd.to_datetime(df.t_ms, unit="ms")
```

Requires `pandas` and `pyarrow`.


## Schema

| Column | Type | Description |
|---|---|---|
| `t_ms` | int64 | Sample time, epoch milliseconds (UTC) |
| `Ax` `Ay` `Az` | float32 | Accelerometer, g — full scale ±4 g |
| `Gx` `Gy` `Gz` | float32 | Gyroscope, degrees/second — full scale ±2000 °/s |
| `Activity` | category | Behaviour label. Empty string = unlabelled |

Samples arrive in radio batches, so consecutive rows may share a timestamp. The effective rate is roughly 100 Hz.


## Label vocabularies

> [!IMPORTANT]
> **The five files do not share a common label set.** Three different vocabularies were used, and no mapping between them is included. Concatenating the files without first reconciling the labels will produce a meaningless target column.

| Vocabulary | Files | Character |
|---|---|---|
| Fine-grained ethogram | `archi_1` `archi_2` `archi_3` | `Sitting`, `Crouching`, `Walking` — Title_Case, specific postures |
| Coarse activity states | `archi_4` | `Stationary_Inactive`, `On_The_Move` — umbrella classes |
| Lowercase ethogram | `archi_5` | `still`, `active_light`, `walking` — lowercase |

Some behaviours appear under three different names across files — for example `Sniffing_down`, `Sniffing_Ground` and `ground_sniffing`; likewise `Pats_back`, `Pats` and `bum_pats`. The coarse vocabulary in `archi_4` cannot be expanded into the fine-grained one: `On_The_Move` covers walking, trotting and running indistinguishably.


## Label breakdown by file

Minutes are wall-clock time measured from sample timestamps. Gaps longer than 0.5 s are treated as radio dropouts and excluded.


### `archi_1.parquet`

*2025-02-18 · Fine-grained ethogram · 290,877 rows · 8 labels*

| Label | Minutes | m:ss | Share |
|---|---:|---:|---:|
| `Crouching` | 11.25 | 11:15 | 24.3% |
| `Sitting` | 7.81 | 7:49 | 16.9% |
| `Eating` | 4.83 | 4:50 | 10.4% |
| `Walking` | 4.74 | 4:44 | 10.2% |
| `Standing` | 2.05 | 2:03 | 4.4% |
| `Resting` | 1.36 | 1:21 | 2.9% |
| `Jumping_up` | 0.07 | 0:04 | 0.1% |
| `Jumping_down` | 0.05 | 0:03 | 0.1% |
| **Labelled total** | **32.17** | **32:10** | **69.4%** |
| *Unlabelled* | *14.17* | *14:10* | *30.6%* |

### `archi_2.parquet`

*2025-02-22 · Fine-grained ethogram · 191,750 rows · 13 labels*

| Label | Minutes | m:ss | Share |
|---|---:|---:|---:|
| `Eating` | 4.40 | 4:24 | 14.4% |
| `Standing` | 4.15 | 4:09 | 13.6% |
| `Sitting` | 3.91 | 3:55 | 12.8% |
| `Walking` | 2.90 | 2:54 | 9.5% |
| `Pats_back` | 1.57 | 1:34 | 5.1% |
| `Sniffing_down` | 0.93 | 0:56 | 3.0% |
| `Littering` | 0.59 | 0:36 | 1.9% |
| `Lying` | 0.18 | 0:11 | 0.6% |
| `Jumping_up` | 0.08 | 0:05 | 0.3% |
| `Running` | 0.08 | 0:05 | 0.3% |
| `Jumping_down` | 0.06 | 0:03 | 0.2% |
| `Scratching_chin` | 0.04 | 0:02 | 0.1% |
| `Shaking` | 0.01 | 0:01 | 0.0% |
| **Labelled total** | **18.88** | **18:53** | **61.8%** |
| *Unlabelled* | *11.67* | *11:40* | *38.2%* |

### `archi_3.parquet`

*2025-02-23 · Fine-grained ethogram · 193,727 rows · 12 labels*

| Label | Minutes | m:ss | Share |
|---|---:|---:|---:|
| `Sitting` | 9.42 | 9:25 | 30.5% |
| `Eating` | 6.43 | 6:26 | 20.8% |
| `Walking` | 2.61 | 2:37 | 8.5% |
| `Standing` | 2.18 | 2:11 | 7.0% |
| `Pats_back` | 1.05 | 1:03 | 3.4% |
| `Sniffing_down` | 0.95 | 0:57 | 3.1% |
| `Crouching` | 0.53 | 0:32 | 1.7% |
| `Grooming` | 0.21 | 0:13 | 0.7% |
| `Jumping_down` | 0.04 | 0:02 | 0.1% |
| `Jumping_up` | 0.04 | 0:02 | 0.1% |
| `Running` | 0.03 | 0:02 | 0.1% |
| `Shaking` | 0.01 | 0:01 | 0.0% |
| **Labelled total** | **23.50** | **23:30** | **76.1%** |
| *Unlabelled* | *7.38* | *7:23* | *23.9%* |

### `archi_4.parquet`

*2025-05-25 (afternoon) · Coarse activity states · 181,988 rows · 8 labels*

| Label | Minutes | m:ss | Share |
|---|---:|---:|---:|
| `Stationary_Inactive` | 8.79 | 8:48 | 29.3% |
| `Resting` | 7.02 | 7:01 | 23.4% |
| `On_The_Move` | 5.83 | 5:50 | 19.4% |
| `Stationary_Active` | 4.22 | 4:13 | 14.1% |
| `Unknown` | 1.93 | 1:56 | 6.4% |
| `Pats` | 0.30 | 0:18 | 1.0% |
| `Jumping` | 0.24 | 0:14 | 0.8% |
| `Sniffing_Ground` | 0.16 | 0:10 | 0.5% |
| **Labelled total** | **28.50** | **28:30** | **95.0%** |
| *Unlabelled* | *0.17* | *0:10* | *0.6%* |
| *Dropout (no signal)* | *1.35* | *1:21* | *4.5%* |

### `archi_5.parquet`

*2025-05-25 (evening) · Lowercase ethogram · 192,963 rows · 13 labels*

| Label | Minutes | m:ss | Share |
|---|---:|---:|---:|
| `active_light` | 10.15 | 10:09 | 32.9% |
| `eating` | 7.68 | 7:41 | 24.9% |
| `walking` | 5.84 | 5:50 | 18.9% |
| `unsure` | 2.28 | 2:17 | 7.4% |
| `still` | 2.08 | 2:05 | 6.7% |
| `ground_sniffing` | 0.90 | 0:54 | 2.9% |
| `littering` | 0.56 | 0:33 | 1.8% |
| `grooming` | 0.33 | 0:20 | 1.1% |
| `bum_pats` | 0.32 | 0:19 | 1.0% |
| `trotting` | 0.05 | 0:03 | 0.2% |
| `shaking` | 0.04 | 0:02 | 0.1% |
| `jumping_down` | 0.02 | 0:01 | 0.1% |
| `jumping_up` | 0.01 | 0:01 | 0.0% |
| **Labelled total** | **30.24** | **30:15** | **98.1%** |
| *Unlabelled* | *0.58* | *0:35* | *1.9%* |

## Notes

- All data was collected on an **Arduino Nano 33 IoT**, using its onboard LSM6DS3 accelerometer/gyroscope.

- `archi_4` contains 1.35 minutes of radio dropout where labelling exists but no samples were received.

- `Unknown` (`archi_4`) and `unsure` (`archi_5`) are explicit annotator-uncertainty classes, not behaviours.

- All five sessions are independent recordings with no overlapping samples.

