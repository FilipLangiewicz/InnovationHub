# Long computations on Kaggle

This document describes the method to run long computations on Kaggle from this repository. It is a collection of rules and lessons from earlier projects. `AGENTS.md` has priority over this document.

The local machines have no GPU. Typical jobs in this project are:

- calculate embeddings for large sets of documents (patents, publications, EU projects);
- run an open language model on many documents (extraction, classification);
- measure the quality of the search and of the matching on the full test set.

The local machine has the code and the records. Kaggle does the heavy computation. Git connects the two.

The application itself must not need Kaggle. A job makes a file (for example an index or a table). The application uses this file.

## 1. Local setup

The local machine needs these items:

| Item | Where | What to do |
|---|---|---|
| Kaggle account | kaggle.com | Do the phone verification (Settings). Without it, a kernel gets no GPU and no internet. |
| Python environment | `.venv` in the root folder of the repository | Make it with uv (see `AGENTS.md`, "Make the local environment"). |
| Kaggle CLI | the package `kaggle` in `requirements-dev.in` | It comes with the lock. Call it only through `jobs/kaggle.sh`. |
| Kaggle API token | `~/.kaggle/access_token` or `.kaggle/access_token` of the project | On kaggle.com, open Settings, API, and generate a new token. Write the token into the file as one line. |
| GitHub token | the Kaggle secret `GITHUB_TOKEN` | A fine-grained token with read access to the contents of this repository only. |
| Access to the data | the private Kaggle dataset of the project, if the project has one | The owner of the dataset shares it with the account. |

To prepare the local machine, do these steps:

1. Make the environment:

   ```bash
   uv venv --python 3.13 .venv
   uv pip sync --python .venv requirements.txt
   ```

2. Write the Kaggle API token into a file (see "Select the Kaggle account").
3. Do a test of the connection. The command shows the kernels of the account:

   ```bash
   bash jobs/kaggle.sh kernels list --mine
   ```

Do not commit a token. The Kaggle token gives full access to the account.

### Add the GitHub token to Kaggle

The repository is private. Thus a kernel needs a GitHub token to clone it.

To add the token, do these steps once for each Kaggle account:

1. On github.com, open Settings, Developer settings, Fine-grained tokens.
2. Make a token for this repository only, with the permission "Contents: Read-only". Set an expiry date after the event.
3. On kaggle.com, open a notebook editor, then Add-ons, Secrets.
4. Add a secret with the label `GITHUB_TOKEN` and the token as the value.

If `get_secret("GITHUB_TOKEN")` fails in a kernel, open the kernel on kaggle.com. Attach the secret in Add-ons, Secrets. Then run the kernel again.

### Select the Kaggle account

The script `jobs/kaggle.sh` selects the token. Then it runs the Kaggle CLI from `.venv`. People, agents and other scripts use the same command. Nobody sets an environment variable or gives the token in a command.

| Token file | Account |
|---|---|
| `.kaggle/access_token` in the project folder, not empty | The account of the project. Only this project uses it. |
| No project token | The personal account of the machine (`~/.kaggle/access_token`). All projects without a project token use it. |

The project token has priority. The script gives it to the CLI in the variable `KAGGLE_API_TOKEN`.

To use the personal account, do these steps once on each machine:

1. Write the token into the file:

   ```bash
   mkdir -p ~/.kaggle
   echo "<token>" > ~/.kaggle/access_token
   chmod 600 ~/.kaggle/access_token
   ```

2. Make sure that the project has no `.kaggle/access_token`, or that the file is empty.

To use a different account for this project, do these steps in the project folder:

1. Make sure that `.gitignore` has the line `.kaggle/`.
2. Write the token of the project account into the file:

   ```bash
   mkdir -p .kaggle
   echo "<token>" > .kaggle/access_token
   chmod 600 .kaggle/access_token
   ```

3. Check the account. The line `username` must show the project account:

   ```bash
   bash jobs/kaggle.sh config view
   ```

`.gitignore` prevents an accidental commit of the token. It does not hide the token from a program of the same user.

### `jobs/kaggle.sh`

```bash
#!/usr/bin/env bash
# Run the Kaggle CLI of the project with the correct account.
# Usage: bash jobs/kaggle.sh <arguments of the kaggle command>
# If .kaggle/access_token of the project is not empty, the CLI uses this token.
# Otherwise the CLI uses the personal credentials (~/.kaggle/access_token).
set -euo pipefail
ROOT="$(cd "$(dirname "${BASH_SOURCE[0]}")/.." && pwd)"
TOKEN="$ROOT/.kaggle/access_token"
# The value, not the path: a Windows Python does not know the Git Bash paths.
if [ -s "$TOKEN" ]; then
  export KAGGLE_API_TOKEN="$(tr -d '[:space:]' < "$TOKEN")"
fi
for KAGGLE in "$ROOT/.venv/Scripts/kaggle.exe" "$ROOT/.venv/bin/kaggle"; do
  [ -x "$KAGGLE" ] && exec "$KAGGLE" "$@"
done
echo "kaggle.sh: no Kaggle CLI in $ROOT/.venv" >&2
exit 127
```

The script looks for `.kaggle/` and `.venv/` in the folder above `jobs/`. The examples in this document call the script from the root folder of the repository.

## 2. Platform facts

| Item | Value |
|---|---|
| GPU machine | `"machine_shape": "NvidiaTeslaT4"` gives 2x Tesla T4, 15 GB of memory each. |
| GPU sessions at the same time | 2 for each account. A third push fails with "Maximum batch GPU session count of 2 reached". |
| GPU time | A weekly quota (approximately 30 hours). A session counts its hours, not the number of its GPUs. |
| CPU sessions | 4 cores and approximately 30 GB of RAM. They do not use the GPU quota. |
| Session limit | 12 hours. |
| Output folder | `/kaggle/working/`, approximately 20 GB. Only this folder is kept after the session. |
| Temporary folder | `/kaggle/temp/`. It is deleted after the session. |
| Input data | `/kaggle/input/<slug>/...`, read-only. |
| Python | The version of Kaggle can differ from the local Python 3.13. Do not use features of one version only. |
| Account | GPU and internet need a phone verification of the account. Without it, a GPU kernel runs on the CPU and no error occurs. |

Important behavior:

- If a kernel fails (status ERROR), Kaggle deletes its complete output. Hours of results can be lost.
- The CLI cannot stop a kernel. Only the web page can cancel it.
- The CLI does not give the version ID of a run. Thus use one kernel for each job, not many versions of one kernel.
- `bash jobs/kaggle.sh kernels logs -f <user>/radar-<job name>` shows the live log. The output of child processes is sometimes not in it.
- Use `bash jobs/kaggle.sh kernels status <user>/radar-<job name>` for the state: QUEUED, RUNNING, COMPLETE, ERROR or CANCEL.

## 3. Repository structure

```
CombAIns-HackNation2026/
  src/                    the backend (Python modules)
  web/                    the web page
  jobs/
    JOBS.md               the list of all jobs and their status
    kaggle.sh             runs the Kaggle CLI with the token of the project or the personal token
    wait.sh               follows one kernel and writes its status into JOBS.md
    runs/
      YYYY-MM-DD_NN_<job>/  one folder for each job (NN: the number of the job, see the parity rule in AGENTS.md)
        run.py              the script that Kaggle runs
        kernel-metadata.json
        <codename>.yaml     one config for each experiment of the job
        output/             the small results, with the same paths as /kaggle/working/
          <codename>/       metrics.json, report_*.csv, ...
```

Make the folder `jobs/` only when the project starts to use Kaggle.

### Names

The job folder `YYYY-MM-DD_NN_<job name>` identifies a job. NN is the number of the job in the project. It does not start again on a new day. Thus the folders sort in the order of the jobs, and the number alone identifies a job. One job folder is one Kaggle kernel and one row in `JOBS.md`.

| Name | Where it occurs |
|---|---|
| Job folder, for example `2026-11-22_02_emb-patents-ab` | The folder in `jobs/runs/` and the column Folder in `JOBS.md`. |
| Job name: the last part of the job folder, for example `emb-patents-ab` | The kernel id (`<user>/radar-emb-patents-ab`), the column Job in `JOBS.md`, the second argument of `wait.sh`. |
| Codename, for example `emb-patents-a` | The config file (`emb-patents-a.yaml`), the output folder (`output/emb-patents-a/`), and the table of results, if the project has one. |

- Each name is unique in the project. A search for a name finds all files of this job or experiment.
- A job has one or more codenames: one config file and one output folder for each codename.
- Variants of one idea get letters: `emb-patents-a` (model A), `emb-patents-b` (model B).
- The job name is the common part of its codenames plus their letters. The job `emb-patents-ab` runs `emb-patents-a` and `emb-patents-b`.
- If a job has only one experiment, its codename is the job name.
- Use only lowercase letters, digits and hyphens. Kaggle makes the kernel slug from the title, and other characters change.

### Rules

- A job is one Kaggle kernel. A job can run more than one experiment (codename), for example one on each GPU.
- The config of an experiment is in the folder of the job that runs it. There is no separate folder for configs.
- The folder of a job does not change after the start. A new variant gets a new folder.
- The job folders are in `jobs/runs/`. Thus `jobs/` shows only `JOBS.md`, `kaggle.sh`, `wait.sh` and `runs/`, also when there are many jobs.
- `output/` keeps only small files. Embeddings, indexes, model weights and caches stay on Kaggle or go to `data/cache/`.

A function finds a config from its codename:

```python
RUNS_DIR = PROJECT_DIR / "jobs" / "runs"

def load_config(codename: str) -> dict:
    matches = sorted(RUNS_DIR.glob(f"*/{codename}.yaml"))
    if len(matches) != 1:
        raise FileNotFoundError(f"need exactly one config for {codename}, found {matches}")
    cfg = yaml.safe_load(matches[0].read_text())
    cfg["codename"] = codename
    return cfg
```

`.gitignore` keeps data, embeddings, indexes, model weights and logs out of git. See the section "Data and caches" in `.gitignore`.

## 4. Code and data on Kaggle

### Code

The kernel clones the branch of the task from the private GitHub repository. Thus every run uses code from git, and the commit hash goes into the log. Push the branch before the push of the kernel.

- Clone the full repository. Do not use a sparse checkout.
- Clone into `/tmp`, not into `/kaggle/working`. Otherwise the clone becomes a part of the output.
- Get the token from the Kaggle secrets. Do not write the token into a file.
- Do not print the clone URL. It contains the token. After the clone, remove the token from the remote URL.

### Data

- If a job needs a large input file, keep it in a private Kaggle dataset. Upload one zip file, not thousands of small files. Share the dataset with the team.
- Upload only data with a license that allows the upload. Record the license in `README.md`.
- A job can also download open data during the run (for example from an API). Then use timeouts and a cache in `/kaggle/working/cache/`.
- The output of one kernel can be the input of another kernel: `"kernel_sources": ["<user>/radar-<job name>"]`. Thus embeddings from one job go to the next job without a download.
- The path under `/kaggle/input` can change. Find files with a recursive search, for example `Path("/kaggle/input").rglob("records.parquet")`.
- To make a dataset from a kernel output, use the page of the kernel: Output, New Dataset. Kaggle extracts a zip to a folder with the name of the zip. If the output also has a folder with this name, the creation fails ("Directory already exists"). Thus give a zip a name like `<codename>-set.zip`.

### `kernel-metadata.json`

```json
{
  "id": "<user>/radar-<job name>",
  "title": "radar-<job name>",
  "code_file": "run.py",
  "language": "python",
  "kernel_type": "script",
  "is_private": true,
  "enable_gpu": true,
  "enable_internet": true,
  "machine_shape": "NvidiaTeslaT4",
  "dataset_sources": [],
  "competition_sources": [],
  "kernel_sources": []
}
```

For a CPU job, use `"enable_gpu": false` and remove `machine_shape`.

The `<user>` in the `id` must be the username of the account that `jobs/kaggle.sh` selects. Otherwise the push and the status fail with "Permission denied".

### `run.py` for one experiment on one GPU

The names `src.<module>`, `load_config` and `run_experiment` are examples. Use the names of the real module.

```python
"""<What the job does, in one or two sentences.>"""
import subprocess
import sys

REPO = "github.com/MikolajRowicki/CombAIns-HackNation2026.git"
BRANCH = "<branch of the task>"
CLONE = "/tmp/repo"  # Not in the output folder, so the clone is not an output.
CODENAMES = ["<codename>"]


def clone() -> None:
    from kaggle_secrets import UserSecretsClient

    token = UserSecretsClient().get_secret("GITHUB_TOKEN")
    url = f"https://x-access-token:{token}@{REPO}"
    # capture_output keeps the URL with the token out of the log.
    result = subprocess.run(["git", "clone", "--depth", "1", "--branch", BRANCH, url, CLONE], capture_output=True, text=True)
    if result.returncode != 0:
        raise RuntimeError("git clone failed: " + result.stderr.replace(token, "***"))
    subprocess.run(["git", "-C", CLONE, "remote", "set-url", "origin", f"https://{REPO}"], check=True)
    commit = subprocess.run(["git", "-C", CLONE, "rev-parse", "HEAD"], capture_output=True, text=True).stdout.strip()
    print("commit", commit)


def install() -> None:
    subprocess.run([sys.executable, "-m", "pip", "install", "-q", "-r", f"{CLONE}/requirements-kaggle.txt"], check=True)


import torch

# A long job on the CPU wastes hours, so stop immediately without a GPU.
assert torch.cuda.is_available(), "no GPU: check the phone verification of the account"
clone()
install()
print("torch", torch.__version__)
sys.path.insert(0, CLONE)

from src.batch import load_config, run_experiment  # example names

for codename in CODENAMES:
    run_experiment(load_config(codename), "cuda:0")
```

## 5. Use of the two GPUs

A T4 x2 session has two GPUs, and the quota counts the session. A job that uses one GPU wastes half of its cost.

One process on each GPU uses both GPUs. Each process does its own work: one experiment, or one half of the documents (shard).

### `run.py` for two shards, one on each GPU

```python
"""Calculate the embeddings of one document set. Each GPU does one half (shard)."""
import subprocess
import sys

REPO = "github.com/MikolajRowicki/CombAIns-HackNation2026.git"
BRANCH = "<branch of the task>"
CLONE = "/tmp/repo"  # Not in the output folder, so the clone is not an output.
CODENAME = "<codename>"


def clone() -> None:
    ...  # The same function as in section 4.


def install() -> None:
    ...  # The same function as in section 4.


def run(shard: int, gpu: int) -> None:
    sys.path.insert(0, CLONE)
    from src.batch import embed_shard  # example name
    embed_shard(CODENAME, shard=shard, n_shards=2, device=f"cuda:{gpu}")


# A new process imports this file again, so the clone and the start must be only in the main process.
if __name__ == "__main__":
    import torch

    assert torch.cuda.device_count() == 2, "this job needs 2 GPUs (T4 x2)"
    clone()
    install()
    print("torch", torch.__version__)
    with torch.multiprocessing.get_context("spawn").Pool(2) as pool:
        pool.starmap(run, [(gpu, gpu) for gpu in range(2)])
```

For two experiments, give each process one codename instead of one shard.

Rules for this pattern:

- Use the `spawn` context. CUDA does not work in a process from `fork`.
- Put the main code under `if __name__ == "__main__":`. Each new process imports `run.py` again.
- The work function gets the device as an argument, and it calls `torch.cuda.set_device(device)`.
- Each process writes its results into its own folder or file, for example `/kaggle/working/<codename>/shard-0.npy`.
- The main process must not keep GPU memory. If it used the GPU before the start of the pool, call `torch.cuda.empty_cache()`. In one job, the main process kept 12 GB of cache on GPU 0, and the process for GPU 0 failed with "CUDA out of memory".
- If one process fails, the complete job fails, and Kaggle deletes the output of both processes. Thus do a smoke test first (section 7).
- Two jobs with two processes each give four processes at the same time.
- Two processes share the RAM of the session. Each process must stay below approximately half of it.

For a CPU job, use `multiprocessing.Pool(4)` for the 4 cores.

## 6. Start and follow a job

### Start

1. Commit and push the branch of the task.
2. Add a row to `jobs/JOBS.md` with the status `to run`.
3. Push the kernel and start `wait.sh` for it.

The push sends `run.py` and `kernel-metadata.json` of the job folder to Kaggle and starts the kernel:

```bash
# From the root folder of the repository:
bash jobs/kaggle.sh kernels push -p jobs/runs/YYYY-MM-DD_NN_<job name>
```

Output if the push is correct:

```
Kernel version 1 successfully pushed.  Please check progress at https://www.kaggle.com/code/<user>/radar-<job name>
```

The first push makes the kernel. A second push of the same folder makes version 2 of the same kernel.

If two GPU sessions are active, the push fails. This loop tries again until a slot is free, and then follows the job:

```bash
until out=$(bash jobs/kaggle.sh kernels push -p jobs/runs/YYYY-MM-DD_NN_<job name> 2>&1); echo "$(date +%H:%M) $out" | tail -1; echo "$out" | grep -q "successfully pushed"; do sleep 90; done; bash jobs/wait.sh <user>/radar-<job name> <job name>
```

### `jobs/wait.sh`

```bash
#!/usr/bin/env bash
# Follow a Kaggle kernel until it stops. Write each status change into JOBS.md.
# Usage: bash wait.sh <owner/kernel-slug> "<job name in the JOBS.md row>"
DIR="$(dirname "$0")"
last=""
while true; do
  raw=$(bash "$DIR/kaggle.sh" kernels status "$1" 2>&1)
  case "$raw" in
    *QUEUED*) status="queued" ;;
    *RUNNING*) status="running" ;;
    *COMPLETE*) status="complete" ;;
    *ERROR*) status="failed" ;;
    *CANCEL*) status="cancelled" ;;
    *) status="$last" ;;
  esac
  if [ -n "$status" ] && [ "$status" != "$last" ]; then
    sed -i "/^| [0-9-]* | $2 |/ s/| [a-z ]* |\$/| $status |/" "$DIR/JOBS.md"
    echo "$(date +%H:%M) $status"
    last="$status"
  fi
  case "$status" in complete|failed|cancelled) echo "$raw"; exit 0 ;; esac
  sleep 60
done
```

`wait.sh` calls `kaggle.sh`. Thus it uses the same account as the push.

The job folder `jobs/runs/YYYY-MM-DD_NN_<job name>/` has the kernel `<user>/radar-<job name>` and the row `<job name>` in `JOBS.md`. The call:

```bash
# From the root folder of the repository, after the push of the kernel:
bash jobs/wait.sh <user>/radar-<job name> <job name>
```

Output: one line for each status change. The last line is the final status from Kaggle.

```
13:48 running
14:32 complete
<user>/radar-<job name> has status "KernelWorkerStatus.COMPLETE"
```

Run it as a background task, because it waits until the kernel stops (minutes or hours).

### Rules for the follow-up

- Start `wait.sh` after each push: for a new job and also for a new version of an old job. Without it, the status in `JOBS.md` stays old.
- Use one background task for each job. A tool that waits for a background task gives a notice only when the complete task stops. If one task follows two jobs, the end of the first job stays without a notice until the second job ends.
- If a `wait.sh` process stops (for example after a move of the folder), start it again.
- When a job ends, check that its row shows the final status.

## 7. Protect long runs

- **Smoke test.** Before a long run, run the same code path on a small part of the data. Use a CPU kernel or the local machine. The test must do all steps, also the final steps (the index, the reports, the final files). An error at the end of a long run loses all its results.
- **Early checks.** Put cheap checks at the start. If a check fails, stop the job. Example: after the first 1000 documents, compare the dimension and the norm of the embeddings with the expected values.
- **Deadline.** A long loop gets a deadline (for example 9 hours). After it, no new batch starts, and the job writes its results before the 12-hour limit.
- **Partial results.** Write the results of each shard or batch as soon as they are ready. Then a failure at the end does not lose all work.
- **No heavy work on the local machine.** The local machine does only small checks. It can stop a process if the memory is low.

## 8. Outputs

1. List the files and their sizes: `bash jobs/kaggle.sh kernels files <user>/radar-<job name>`.
2. Download only the small files into the output folder of the job:

   ```bash
   bash jobs/kaggle.sh kernels output <user>/radar-<job name> -p jobs/runs/YYYY-MM-DD_NN_<job name>/output --file-pattern '(json|csv|png)$'
   ```

   The pattern is a regular expression for the complete path. Do not start it with `^`.
3. Download large files that the application needs (for example an index) into `data/cache/`. Git ignores this folder.
4. Delete the downloaded `.log` file if it is not necessary.
5. Commit the small results.

Write large intermediate data only to `/kaggle/temp/` or to `/kaggle/working/cache/`. Write small results as CSV or JSON.

## 9. Records

### `jobs/JOBS.md`

The file starts with a short procedure (sections 6 and 8), then the table:

```markdown
Status values: to run, waiting for another job, queued, running, complete, failed, cancelled. The status cell has only one of these values, because `wait.sh` changes it. Put results into the comment.

| Date | Job | Folder | Link | Comment | Status |
|---|---|---|---|---|---|
| YYYY-MM-DD | <job name> | `YYYY-MM-DD_NN_<job name>` | [radar-<job name>](https://www.kaggle.com/code/<user>/radar-<job name>) | <what the job does; the main results after the end> | complete |
```

- The column Job is the name that `wait.sh` gets as its second argument.
- The status cell has one word only. A result in this cell stops `wait.sh`, because its pattern does not match.
- One job folder has one row. A new version of the job (for example after an error) uses the same row and gets a note in the comment, for example "Version 1: out of memory on GPU 0".
- A second row of the same job causes errors: `wait.sh` changes all rows with the job name.

### A table of results (optional)

A table that compares the methods helps, for example embedding models for the search of similar innovations. If the project has such a table, these rules help:

- Each row has the codename and the job folder. Then the config and the results of a row are easy to find.
- Write a row as soon as its results are available. Write "pending" only for a value that does not exist yet.
- Text above the table tells what each column measures.

## 10. Experiment method

- **Codenames.** Each experiment has a codename with a version, for example `emb-patents-a`. Variants of one idea get letters (section 3).
- **One change for each variant.** A group of variants has a common base, and each variant changes one thing (for example only the embedding model). Then the difference shows the effect of this change.
- **Fixed test set.** Keep the test cases in a file (`tests/cases.json`). For the search, keep pairs of records with a label "similar" or "not similar". Do not change these files during a comparison.
- **Metrics.** Use a metric that agrees with the use. Examples:
  - the search of similar innovations: recall@10 and MRR;
  - the detection of duplicates: precision and recall at the selected threshold;
  - the extraction with a language model: the share of fields that agree with the labels.
- **Repeatable runs.** Run a language model with temperature 0. Record the version of the model, of the prompt and of the data in `metrics.json`.
- **Noise.** Two runs with the same settings and a different seed show the random difference. A difference smaller than this is not a result.
- **Training (only if the project trains a model).** Keep checkpoints, and score each checkpoint with the real metric on a separate tune part. Keep the best checkpoint. The loss does not always follow the metric.
