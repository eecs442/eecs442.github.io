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

- We suggest you follow these steps to setup the codebase for this assignment depending on whether you are using Google Colab or your local system.

    **Google Colab**: Steps to Setup the Codebase

    1. Download and extract the repository. 
    2. Upload the folder containing the repository (with the notebook) to your Google Drive. 
    3. Ensure that you are using the GPU session by using the `Runtime -> Change Runtime Type` and selecting `Python3` and `T4 GPU`. Start the session by clicking `Connect` at the top right. The default T4 GPU should suffice for you to complete this assignment.
    4. Mount your Google Drive to the Colab by using the below code in the first cell of the notebook.
    5. The first few cells make sure that you can run the notebook along with the code in your drive folder. Fill the variable `GOOGLE_DRIVE_PATH_AFTER_MYDRIVE` with the path to your folder after `drive/MyDrive` to include the repository to your system path.
    6. You are good to start Section 2 of the assignment. Please use your GPU resources prudently. If you wish, you may create multiple google accounts and share the your drive folder to those accounts to use the GPUs from these accounts.

    **Local System**: Steps to Setup the Codebase (This is applicable for Section 1 as well)

    1. Download and extract the repository.
    2. You are good to start Section 2 of the assignment.

## Submission
The submission includes two parts:
1. **To Canvas**: submit a `zip` file containing a **single** directory with your **uniqname** as the name that contains all your code and anything else asked for on the [Canvas Submission Checklist](#canvas-submission-checklist). Don't add unnecessary files or directories. Starter code is given to you on Canvas under the “Homework 5” assignment. You can also download it [here](https://drive.google.com/file/d/1cZEn87Qsc1qlKTLWtWY02rrq6oJ0G3Nn/view?usp=drive_link). Clean up your submission to include only the necessary files. Pay close attention to filenames for autograding purposes.



    {{ code }} - 
    <span class="code">We have indicated questions where you have to do something in code in red. **If Gradescope asks for it, also submit your code in the report with the formatting below.**</span>  
    <!-- {{ autograde }} - 
    <span class="autograde">We have indicated questions where we will definitely use an autograder in purple</span> -->
<!-- 
    Please be especially careful on the autograded assignments to follow the instructions. Don't swap the order of arguments and do not return extra values. If we're talking about autograding a filename, we will be pulling out these files with a script. Please be careful about the name. -->
<!-- 
    Your zip file should contain a single directory which has the same name as your uniqname. If I (David, uniqname `fouhey`) were submitting my code, the zip file should contain a single folder `fouhey/` containing all required files.   -->
        
    <div class="primer-spec-callout info" markdown="1">
      **Submission Tip:** Use the [Tasks Checklist](#tasks-checklist) and [Canvas Submission Checklist](#canvas-submission-checklist) at the end of this homework. We also provide a script that validates the submission format [here](https://raw.githubusercontent.com/eecs442/utils/master/check_submission.py){:target="_blank"}.

      <!-- If we don't ask you for it, you don't need to submit it; while you should clean up the directory, don't panic about having an extra file or two. -->
    </div>

2. **To Gradescope**: submit a `pdf` file as your write-up, including your answers to all the questions and key choices you made.

    {{ report }} - 
    <span class="report">We have indicated questions where you have to do something in the report in green. **Coding questions also need to be included in the report.**</span>


    You might like to combine several files to make a submission. Here is an example online [link](https://combinepdf.com/){:target="_blank"} for combining multiple PDF files. The write-up must be an electronic version. **No handwriting, including plotting questions.** $$\LaTeX$$ is recommended but not mandatory.

    For including code, **do not use screenshots**. Generate a PDF using a [tool like this](https://www.i2pdf.com/source-code-to-pdf){:target="_blank"} or using this [Overleaf LaTeX template](https://www.overleaf.com/read/wbpyympmgfkf#bac472){:target="_blank"}. If this PDF contains only code, be sure to append it to the end of your report and match the questions carefully on Gradescope.

    For `.ipynb` notebooks in your local system you may refer to this [post](https://saturncloud.io/blog/how-to-convert-ipynb-to-pdf-in-jupyter-notebook/) to convert your notebook to a pdf using `nbconvert`. For Google-Colab users, you may use this code snippet as the final cell to download your notebooks as pdf's.
    
    ```python
    # generate pdf
    # 1. Find the path to your notebook in your google drive.
    # 2. Please provide the full path of the notebook file below.

    # Important: make sure that your file name does not contain spaces!

    # Syntax: 
    # notebookpath = '/content/drive/MyDrive/HW4/notebook.ipynb' 
    # 
    import os
    from google.colab import drive
    from google.colab import files

    drive_mount_point = '/content/drive/'
    drive.mount(drive_mount_point)

    notebookpath = '<notebook_path>' 
    file_name = notebookpath.split('/')[-1]
    get_ipython().system("apt update && apt install texlive-xetex texlive-fonts-recommended texlive-generic-recommended")
    get_ipython().system("jupyter nbconvert --to PDF {}".format(notebookpath.replace(' ', '\\ ')))
    files.download(notebookpath.split('.')[0]+'.pdf')
    # PDF will be downloaded in the same directory as the notebook, with the same name.
    ```

# Section 1: Pix2Pix

In this section, you will implement an image-to-image translation program based on [pix2pix](https://phillipi.github.io/pix2pix/). You will train the pix2pix model on the *edges2shoes* dataset to translate images containing only the edges of a shoe, to a full image of a shoe. The edges are automatically extracted from the real shoe images.

Some example edge/image pairs are shown in Figure 1

<figure class="figure-container">
  <img src="{{site.url}}/assets/hw5/figures/edges2shoes.png" alt="Edges2Shoes" width="50%">
  <figcaption>Figure 1: Edges2Shoes Dataset </figcaption>
</figure>


The pix2pix model is based on a conditional GAN (Figure 2). The generator G maps the
source image x to a synthesized target image. The discriminator takes both the source image
and predicted target image as its inputs, and predicts whether the input is real or fake.

## Task 1: Dataloading
You will first build data loaders for training and testing. For the training, you can use a batch size of 4. During testing, you will process 5 images in a single batch, so that we can visualize several results at once. 

{{ code }} <span class="code"> Implement the Edges2Image class and fill in the TODOs in that cell. </span>

**Hint**: please use the DataLoader from torch.utils.data


Next, we move to the model architecture for Pix2Pix. We have provided the implementation for the generator and the discriminator models in the notebook. Refer and familiarize yourself with the architecture from the model summary, especially the input and the output shapes.

## Task 2: Training Pix2Pix

### Optimization

1. For optimization, we’ll use the Adam optimizer. Adam is similar to SGD with momentum, but it also contains an adaptive learning rate for each model parameter. If you want to learn more about Adam, please refer to the deep learning book by [Ian Goodfellow et al](https://www.deeplearningbook.org/). For our model training, we will use a learning rate of 0.0002, and momentum parameters β1 = 0.5 and β2 = 0.999. 

{{ code }} <span class="code"> Please set up `G_optimizer` and `D_optimizer` in the train function. </span>


### Pix2Pix Objective Function

Given a generator $$G$$ and a discriminiator $$D$$, the loss function / objective functions to be minimized are given by

$$
\mathcal{L}_{cGAN}(G, D) = \frac{1}{N} \left(\: \sum_{i=1}^{N} log D(x_i, y_i)
+ \sum_{i=1}^{N} log (1 - D\:(\:G\:(x_i),\: y_i) \:)
\right)
$$

where $$(x_i, y_i)$$ refers to the pair to the ground-truth input-output pair and $$G(x_i)$$ refers to the image translated by the Generator.

$$
\mathcal{L}_{L1}(G, D) = \frac{1}{N} \sum_{i=1}^{N} \|\:y - G(x_i) \:\|_1
$$

The final objective is just a combination of these objectives.

$$
\mathcal{L}_{final}(G, D) = \mathcal{L}_{cGAN}(G, D) + λ \:\mathcal{L}_{L1}(G, D)
$$

$$
G^* = \underset{G}{\mathrm{argmin}} \:\underset{D}{\mathrm{max}}\; \mathcal{L}_{final}(G, D)
$$

You would be implementing these objectives using the `nn.BCELoss` and `nn.L1Loss` as provided in the code.

{{ code }} <span class="code"> Implement the code for the function `train` as instructed by the notebook.</span>

2. You will train the model using the objective $$\mathcal{L}_{final}$$ using λ = 100. Train the network for at least 20 epochs. You are welcome to train longer, though, to potentially obtain better results. Please complete the following tasks for the report.
    - Attach the plot for the history of the Discriminiator.
    - Attach the plot for the history of the BCE Loss of the Generator.
    - Attach the plot for the history of the L1 Loss of the Generator.

    {{ report }} <span class="report">In your report, include these plots.</span>

