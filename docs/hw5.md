---
layout: spec
permalink: /hw5
latex: true

title: Homework 5 – Image Generative Models
due: 11:59 p.m. on Wednesday April 3th, 2024
---

<link href="style.css" rel="stylesheet">
<div style="display:none">
    <!-- Define LaTeX commands here -->
    \(
        \newcommand{\RR}{\mathbb{R}}
        \newcommand{\pd}[2]{\frac{\partial #1}{\partial #2}}
    \)
</div>

{% capture code %}<i class="fa fa-code icon-large"></i>{% endcapture %}
{% capture autograde %}<i class="fa fa-robot icon-large"></i>{% endcapture %}
{% capture report %}<i class="fa fa-file icon-large"></i>{% endcapture %}

# Homework 5 – Image Generative Models

## Instructions

This homework is **due at {{ page.due }}**.

This homework is divided into two major sections based on how you're expected to write code:

**Section 1**:
    
- You'll be writing the code in the same way you've been writing for Homework 4 Part 2, i.e., [Google Colab](https://colab.research.google.com/notebooks/intro.ipynb#recent=true){:target="_blank"}. You may use local [Jupyter Notebooks](https://jupyter.org/){:target="_blank"}, however suggest that you use Google Colab, as running these models on local system may consume too much of GPU RAM (This assignment is not **CPU Friendly** like Homework 4.).


**Section 2**:

- In this section, you will be downloading a github repository from this link. We suggest you follow these steps to setup the codebase for this assignment depending on whether you are using Google Colab or your local system.

    **Google Colab**: Steps to Setup the Codebase

    1. Download and extract the repository and transfer the hw5_diffusion.ipynb to the root of the repository. 
    2. Upload the folder containing the repository (with the notebook) to your Google Drive. 
    3. Ensure that you are using the GPU session by using the `Runtime -> Change Runtime Type` and selecting `Python3` and `T4 GPU`. Start the session by clicking `Connect` at the top right. The default T4 GPU should suffice for you to complete this assignment.
    4. Mount your Google Drive to the Colab by using the below code in the first cell of the notebook.
    5. The first few cells make sure that you can run the notebook along with the code in your drive folder. Fill the variable `GOOGLE_DRIVE_PATH_AFTER_MYDRIVE` with the path to your folder after `drive/MyDrive` to include the repository to your system path.
    6. You are good to start Section 2 of the assignment. Please use your GPU resources prudently. If you wish, you may create multiple google accounts and share the your drive folder to those accounts to use the GPUs from these accounts.

    **Local System**: Steps to Setup the Codebase

    1. Download and extract the repository and transfer the hw5_diffusion_local.ipynb to the root of the repository.
    2. You are good to start Section 2 of the assignment.