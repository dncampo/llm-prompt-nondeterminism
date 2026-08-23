# llm-prompt-nondeterminism

Measure non determinism in LLM logits when batching prompts.

Same prompt, temperature 0, run alone vs run packed into a batch with other
filler prompts. Output tokens mostly stay the same. Logit values do not
always stay the same.

## Status

results and datasets are in. notebooks used to produce them are not pushed
yet, they come in a later commit.

## Layout

```
results/
  <condition>_<gpu>/
    <dataset>/
      dataset/<model>/          input prompts fed to that model
      results_<gpu>/<model>/    logged run output for that model
```

condition
```
both_variable   batch composition and run vary together
intrafrozen     inner run held frozen, file suffix _inner_run_FROZEN
```

gpu
```
A100
H100
```

dataset
```
commonsense
hellaswag
mmlu
winogrande
```

## dataset files

each `dataset/<model>/` folder holds the prompt pools for that model.

```
target_XXX.csv         fixed length target prompt sample
larger_N.csv           filler prompt pool, group N, longer than target
partition_summary.csv  stats on how the pools were built and sampled
```

## results files

```
results_<model>__Qbf16_DTYPEbf16_target_<idx>.csv
results_<model>__Qbf16_DTYPEbf16_target_<idx>_inner_run_FROZEN.csv
```

one row per run. columns include model name, seed, quantization, dtype,
GPU name, batch size, which filler group was used, run index, the target
answer, the raw and processed model response, and the logit value for each
answer option.

model folder names differ between dataset and results. dataset side uses
short handles, results side uses the full model name, example
`llama3.1_8b_instruct` in dataset maps to `Llama-3.1-8B-Instruct` in
results. same model, different label.

## notes

no code here yet. notebooks will be available soon.
