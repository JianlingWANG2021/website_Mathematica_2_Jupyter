<div align="center">

# *Mathematica and Jupyter notebook conversion*
<br>

<font size="4">
Université d'Orléans, Université de Tours, CNRS, IDP, UMR 7013, Orléans, France
<br>
<br>
</font>

</div>


Here is a concise explanation on the conversion from Mathematica notebook script to Jupyter Notebook, and verse vice. 


> This README summarizes a practical workflow to install **Wolfram Engine**, enable the **WolframLanguageForJupyter** kernel in **Jupyter**, and convert between **Mathematica Notebooks (`.nb`)** and **Jupyter Notebooks (`.ipynb`)**.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Install Wolfram Engine in Jupyter](#install-wolfram-engine-in-jupyter)
- [Conversion A: Mathematica `.nb` → Jupyter `.ipynb` (Mathematica2Jupyter)](#conversion-a-mathematica-nb--jupyter-ipynb-mathematica2jupyter)
- [Conversion B: Jupyter `.ipynb` → Mathematica `.nb` (Export Script / Markdown Import)](#conversion-b-jupyter-ipynb--mathematica-nb-export-script--markdown-import)
- [Conversion C: Jupytext Workflow (`.ipynb` → `.wolfram` → `.nb`)](#conversion-c-jupytext-workflow-ipynb--wolfram--nb)
- [Notes / Known Issues](#notes--known-issues)
- [References](#references)

---

## Prerequisites

- **Git** installed
- **Python + Jupyter** installed (e.g., via Anaconda or `pip install jupyter`)

---

## Install Wolfram Engine in Jupyter

### 1) Install Wolfram Engine for  macOS (Homebrew)

```bash
brew install --cask wolfram-engine
```

### Other platforms

You can also download and install Wolfram Engine from Wolfram’s official <a href="https://www.wolfram.com/engine">website</a>.

---


### 2) Make Wolfram Language engine  available:

Set the `WolframKernel` path (macOS example)

```bash
export WolframKernel="/Applications/Wolfram Engine.app/Contents/MacOS/WolframKernel"
```

To make it persistent, put it into `~/.zshrc` or `~/.bashrc`, then reload:

```bash
source ~/.zshrc
```


### 3) Get Wolfram Language engine free license on the website Wolfram with your account.
From <a href="https://www.wolfram.com/engine/">here</a> to get free license for Wolfram engine.

### 4) Make  Wolfram Language engine  activate 

```bash
wolframscript  -activate
```
Follow the prompts to sign in and activate by inputing your account name and password for activation 


### 5) Make the Wolfram Language available in Jupyter

Clone the <a href="https://github.com/WolframResearch/WolframLanguageForJupyter?tab=readme-ov-file">WolframLanguageForJupyter</a> repository as belowing:

```bash
git clone https://github.com/WolframResearch/WolframLanguageForJupyter.git
cd WolframLanguageForJupyter
```

 Install/register the kernel

```bash
./configure-jupyter.wls add
```


Now you could you Wolfram engine in Jupyter notebook.






---

## Conversion A: Mathematica `.nb` → Jupyter `.ipynb` (Mathematica2Jupyter)

The <a href="https://github.com/divenex/mathematica2jupyter">Mathematica2Jupyter</a>  Wolfram Language package provides a function to convert
Mathematica notebooks (.nb files) into either Jupyter Notebook files (.ipynb)
or VS Code Notebook files (.wlnb/.vsnb). 

### 1) Get the converter script

- Script: `Mathematica2Jupyter.wl`
- Repo: `https://github.com/divenex/mathematica2jupyter`
```bash
git clone https://github.com/divenex/mathematica2jupyter
```


### 2) Convert inside Mathematica / Wolfram Language

```wl
Get["/Your path to /mathematica2jupyter/Mathematica2Jupyter.wl"];
Needs["Mathematica2Jupyter`"];
Mathematica2Jupyter["/Your path to your script/Example.nb", "ipynb"]
```

Expected output: `Example.nb` → `Example.ipynb`


### Alternative: `.nb` → `.md` → `.ipynb`

- In Mathematica: **Save As** Markdown → `mathematica.md`
- In Jupyter:
  1. Import `mathematica.md`
  2. Save As `jupyter_notebook.ipynb`

---

## Conversion B: Jupyter `.ipynb` → Mathematica `.nb` (Export Script / Markdown Import)

### Method 1: Export an executable script (`.m`) from Jupyter, then open in Mathematica

In Jupyter (with Wolfram kernel):

- open `jupyter_example.ipynb`
- `File → Save and Export Notebook As → Executable Script`
- Output: `jupyter_example.m`


In Mathematica notebook:

```wl
txt = Import["/Users/wjl/Downloads/jupyter_example.m", "Text"];
cells = Cell[#, "Input"] & /@ StringSplit[txt, "\n\n"];
nb = CreateDocument[cells];
```

Then Evaluation → Evaluate Notebook


### Method 2: `.ipynb` → Markdown → Import into Wolfram as a notebook

#### Steps

1. Convert a Jupyter notebook to Markdown:

```bash
jupyter your_notebook.ipynb --to markdown --output your_notebook.md
```

2. Import the Markdown and create a notebook in Wolfram Language:

```wl
nb = Import["/path/to/your_notebook.md", "Markdown"];
NotebookPut[nb]
```

#### Alternative: use `ResourceFunction["MarkdownImport"]`

```wl
nb = ResourceFunction["MarkdownImport"]["/path/to/your_notebook.md"];
NotebookPut[nb]
```

---

## Conversion C: Jupytext Workflow (`.ipynb` → `.wolfram` → `.nb`)

### 1) Install jupytext

```bash
pip install jupytext
```

### 2) Pair/convert `.ipynb` with Wolfram text format

```bash
jupytext --set-formats ipynb,wolfram         jupyter_example.ipynb 
```

This produces (or pairs) a file like:

- `jupyter_example.wolfram`

### 3) Import into Mathematica and save as `.nb`

```wl
code = Import["/Your path to/jupyter_example.wolfram", "Text"];
nb = CreateDocument[Cell[#, "Input"] & /@ StringSplit[code, "\n\n"]];
```

Then Evaluation → Evaluate Notebook

Then in Mathematica: `File → Save As...` and save as `.nb`.

---

## Notes / Known Issues

- Conversion tooling may behave differently across OSes/environments; formatting may not always be identical.
- Replace all example paths with your real local paths.
- `export WolframKernel=...` only applies to the current terminal session unless you add it to your shell profile.

---

## References

- Wolfram Engine: `https://www.wolfram.com/engine/`
- WolframLanguageForJupyter: `https://github.com/WolframResearch/WolframLanguageForJupyter`
- Mathematica2Jupyter: `https://github.com/divenex/mathematica2jupyter`


