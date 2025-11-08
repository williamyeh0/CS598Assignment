<p align="center">
  <img src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/0b2f5b42-761e-4d5b-8d6f-77c8b965f017" width="450">
</p>

# CS 598 JH Assignment: Enhancing KG-RAG for Biomedical Question Answering

This assignment requires you to set up, replicate, and enhance the **KG-RAG (Knowledge Graph-based Retrieval Augmented Generation)** framework. You will start by reproducing the baseline results using the `gemini-2.0-flash` model, then design and evaluate three distinct improvement strategies.

## Assignment Instructions

### **Step 1: Set Up the Environment**

First, prepare your local environment by cloning the repository, creating a virtual environment, installing dependencies, and running the setup script.

```bash
# Clone the repository
git clone https://github.com/maszhongming/CS-598-JH-Assignment
cd CS-598-JH-Assignment

# Create and activate a conda virtual environment
conda create -n kg_rag python=3.10.9
conda activate kg_rag

# Install the required packages
pip install -r requirements.txt

# Run the setup script to create the disease vector database
python -m kg_rag.run_setup
```

### **Step 2: Update Your Google API Key**

Configure your Google API key to use the Gemini model. The recommended LLM for this assignment is **Gemini-2.0-flash**, which is used for Disease Entity Extraction and Answer Generation.

  * **Get Your API Key**: You can set up your API key for free by visiting [this link](https://makersuite.google.com/app/apikey).
  * **Free Credits & Rate Limits**: While there are free credits available, please be aware of the daily rate limits. Plan your project schedule accordingly to avoid interruptions.
  * **Update Config File**: Add your API key to the `gpt_config.env` file.

### **Step 3: Replicate the Baseline Model**

Run the following script to generate results using the baseline KG-RAG implementation with `gemini-2.0-flash`.

```bash
sh run_gemini.sh
```

### **Step 4: Evaluate the Baseline**

Execute the evaluation script to measure the performance of the baseline model.

```bash
python data/my_results/evaluate_gemini.py
```

### **Step 5: Implement Enhancement Strategies**

This is the core of the assignment. You are required to implement **3 distinct improvement strategies** in the [`kg_rag/rag_based_generation/GPT/run_mcq_qa.py`](kg_rag/rag_based_generation/GPT/run_mcq_qa.py) file.

We have left TODO sections for `MODE 1`, `MODE 2`, and `MODE 3` in the code as placeholders for your implementations.

### **Step 6: Evaluate Your Enhancements**

Evaluate the performance of each of your proposed strategies.

1.  Ensure your enhanced model variant saves its output to a new file path.
2.  Open the evaluation script at [`data/my_results/evaluate_gemini.py`](data/my_results/evaluate_gemini.py) and modify the file path to point to your new results file.
3.  Run the script again and record the results for each of your three strategies.

-----

## What is KG-RAG?

KG-RAG stands for Knowledge Graph-based Retrieval Augmented Generation.

### Start by watching the video of KG-RAG

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/86e5b8a3-eb58-4648-95a4-271e9c69b4ed" controls="controls" style="max-width: 730px;">
</video>

It is a task agnostic framework that combines the explicit knowledge of a Knowledge Graph (KG) with the implicit knowledge of a Large Language Model (LLM). Here is the [arXiv preprint](https://arxiv.org/abs/2311.17330) of the work.

Here, we utilize a massive biomedical KG called [SPOKE](https://spoke.ucsf.edu/) as the provider for the biomedical context. SPOKE has incorporated over 40 biomedical knowledge repositories from diverse domains, each focusing on biomedical concept like genes, proteins, drugs, compounds, diseases, and their established connections. SPOKE consists of more than 27 million nodes of 21 different types and 53 million edges of 55 types [[Ref](https://doi.org/10.1093/bioinformatics/btad080)]


The main feature of KG-RAG is that it extracts "prompt-aware context" from SPOKE KG, which is defined as: 

**the minimal context sufficient enough to respond to the user prompt.** 

Hence, this framework empowers a general-purpose LLM by incorporating an optimized domain-specific 'prompt-aware context' from a biomedical KG.

## Example use case of KG-RAG
Following snippet shows the news from FDA [website](https://www.fda.gov/drugs/news-events-human-drugs/fda-approves-treatment-weight-management-patients-bardet-biedl-syndrome-aged-6-or-older) about the drug **"setmelanotide"** approved by FDA for weight management in patients with *Bardet-Biedl Syndrome*

<img src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/fc4d0b8d-0edb-461d-86c5-9d0d191bd97d" width="600" height="350">

### Ask GPT-4 about the above drug:

### WITHOUT KG-RAG

*Note: This example was run using KG-RAG v0.3.0. We are prompting GPT from the terminal, NOT from the chatGPT browser. Temperature parameter is set to 0 for all the analysis. Refer [this](https://github.com/yzjiao/KG_RAG/blob/main/config.yaml) yaml file for parameter setting*

<video src="https://github.com/yzjiao/KG_RAG/assets/42702311/dbabb812-2a8a-48b6-9785-55b983cb61a4" controls="controls" style="max-width: 730px;">
</video>

### WITH KG-RAG

*Note: This example was run using KG-RAG v0.3.0. Temperature parameter is set to 0 for all the analysis. Refer [this](https://github.com/BaranziniLab/KG_RAG/blob/main/config.yaml) yaml file for parameter setting*

<video src="https://github.com/yzjiao/KG_RAG/assets/42702311/acd08954-a496-4a61-a3b1-8fc4e647b2aa" controls="controls" style="max-width: 730px;">
</video>

You can see that, KG-RAG was able to give the correct information about the FDA approved [drug](https://www.fda.gov/drugs/news-events-human-drugs/fda-approves-treatment-weight-management-patients-bardet-biedl-syndrome-aged-6-or-older).

-----

## Citation

If you find this work useful, please cite the original paper:

```
@article{soman2023biomedical,
  title={Biomedical knowledge graph-enhanced prompt generation for large language models},
  author={Soman, Karthik and Rose, Peter W and Morris, John H and Akbas, Rabia E and Smith, Brett and Peetoom, Braian and Villouta-Reyes, Catalina and Cerono, Gabriel and Shi, Yongmei and Rizk-Jackson, Angela and others},
  journal={arXiv preprint arXiv:2311.17330},
  year={2023}
}
```
