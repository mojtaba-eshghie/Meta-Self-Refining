# Repairing Language Model Pipelines by Meta Self-Refining Competing Constraints at Runtime

This repository contains the code for the paper **"Repairing Language Model Pipelines by Meta Self-Refining Competing Constraints at Runtime"** It provides a proof-of-concept implementation of the **Meta Self-Refining** framework using the [DSPy](https://github.com/stanfordnlp/dspy) library.

## 🧐 The Problem: The "Ping-Pong" Failure Loop

Modern language model (LM) pipelines often use self-correction to adhere to programmatic constraints. However, a critical failure mode emerges when these constraints are in logical tension. For example, asking a model to be both **highly specific** and **extremely concise** can cause it to enter a "ping-pong" loop:

1.  The model generates a specific output that is **too long**.
2.  The model corrects for length but **omits required details**.
3.  The model corrects for the missing details but is once again **too long**.

This oscillation wastes computational resources and fails to produce a satisfactory output.

## 💡 The Solution: Meta-Self-Refining

**Meta-Self-Refining** is a framework that adds a meta-cognitive layer to the pipeline to resolve these conflicts.

1.  **Loop Detection**: It monitors the history of failed attempts to detect a repeating `A -> B -> A` failure pattern.
2.  **Context Aggregation**: It gathers a rich history of the conflicting attempts, including the generated output and the specific error for each.
3.  **Meta-Correction**: It invokes a "meta-corrector" LM, which analyzes this history and synthesizes a new, **strategic instruction** to guide the base model.
4.  **Informed Retry**: The base model is retried with this new, "healing" instruction, allowing it to break the loop and satisfy all constraints.

## 🧪 The Experiment: The "Specific-yet-Concise" Tweet

The included notebook (`Meta-Self-Refining.ipynb`) demonstrates this concept with a challenging "Tweet Summarizer" task:

  * **Goal**: Summarize a dense technical paragraph about Generative Adversarial Networks (GANs).
  * **Conflicting Constraints**:
    1.  The tweet **must include** the keywords: `GAN`, `generator`, and `discriminator`.
    2.  The tweet **must be under** 100 characters.

The experiment shows the pipeline entering a ping-pong loop and then successfully using the **Meta-Self-Refining** framework to generate a tweet that meets both requirements.

-----

## 🚀 How to Rerun the Experiment

### 1\. Prerequisites

First, clone the repository and install the necessary Python packages. It is recommended to use a virtual environment.

```bash
git clone https://github.com/mojtaba-eshghie/Meta-Self-Refining
cd Meta-Self-Refining
```

**This part is still in progress...**


### 2\. Setup Your Environment

The experiment uses the OpenAI API. You need to set your API key as an environment variable.

On macOS/Linux:

```bash
export OPENAI_API_KEY='your-api-key-here'
```

On Windows (Command Prompt):

```cmd
set OPENAI_API_KEY=your-api-key-here
```

Or (PowerShell):

```powershell
$env:OPENAI_API_KEY="your-api-key-here"
```

### 3\. Run the Notebook

Launch JupyterLab and open the notebook to run the experiment.

```bash
jupyter lab
```

Inside JupyterLab, open `Meta-Self-Refining.ipynb` and run the cells sequentially.

-----

## ✅ Expected Outcome

If everything is configured correctly, running the notebook will produce an output log demonstrating the framework in action. You should see the following sequence of events:

1.  A series of failed attempts as the model oscillates between violating the length and keyword constraints.
2.  The detection of the "ping-pong" loop.
3.  The generation of a new, synthesized instruction by the meta-corrector.
4.  A final, successful attempt that satisfies all constraints.

**Example Log:**

```
🚨 Attempt 1 Failed: Tweet must be very concise (under 100 characters). (...)
🚨 Attempt 2 Failed: Tweet must include these keywords: ['GAN', 'generator', 'discriminator'] (...)
🚨 Attempt 3 Failed: Tweet must be very concise (under 100 characters). (...)

--- META-SELF-REFINING: PING-PONG LOOP DETECTED ---
💥 Conflicting History: [...]
✨ Synthesized Instruction: To create a concise tweet under 100 characters that includes 'GAN', 'generator', and 'discriminator', try using abbreviations and focus on the core adversarial relationship.

✅ Success: GANs: generator creates data, discriminator detects fakes—adversaries in AI. #AI #GAN
```

## 📜 Citation

```bibtex
@misc{EshghieMeaSelfRefining,
      title={Repairing Language Model Pipelines by Meta Self-Refining Competing Constraints at Runtime}, 
      author={Mojtaba Eshghie},
      year={2025},
      eprint={2507.10590},
      archivePrefix={arXiv},
      primaryClass={cs.SE},
      url={https://arxiv.org/abs/2507.10590}, 
}
```
