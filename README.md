# Video-MME: The First-Ever Comprehensive Evaluation Benchmark of Multi-modal LLMs in Video Analysis

![VideoQA](https://img.shields.io/badge/Task-VideoQA-red) 
![Multi-Modal](https://img.shields.io/badge/Task-Multi--Modal-red) 
![Video-MME](https://img.shields.io/badge/Dataset-Video--MME-blue)  
![Gemini](https://img.shields.io/badge/Model-Gemini-green) 
![GPT-4V](https://img.shields.io/badge/Model-GPT--4V-green) 
![GPT-4o](https://img.shields.io/badge/Model-GPT--4o-green)

<p align="center">
    <img src="./asset/name_logo.jpg" width="100%" height="100%">
</p>

<font size=7><div align='center' > [[🍎 Project Page](https://video-mme.github.io/)] [[📖 arXiv Paper](https://arxiv.org/pdf/2405.21075)] [[📊 Dataset](https://github.com/BradyFU/Video-MME?tab=readme-ov-file#-dataset)][[🏆 Leaderboard](https://video-mme.github.io/home_page.html#leaderboard)]  </div></font>

Video-MME applies to both **image MLLMs**, i.e., generalizing to multiple images, and **video MLLMs**. 🌟


---

## 🔥 News
* **`2024.06.15`** 🌟 We have refreshed our evaluation: 1) replace broken and potentially broken video links, and re-annotated them; 2) GPT-4o now samples 384 frames (previously 10 from the website) at 512x512 resolution, boosting overall accuracy to 71.9%.
* **`2024.06.03`** 🌟 We are very proud to launch Video-MME, the first-ever comprehensive evaluation benchmark of MLLMs in Video Analysis!



## 👀 Video-MME Overview

In the quest for artificial general intelligence, Multi-modal Large Language Models (MLLMs) have emerged as a focal point in recent advancements, but their potential in processing sequential visual data is still insufficiently explored. We introduce <strong>Video-MME</strong>, the first-ever full-spectrum, <strong>M</strong>ulti-<strong>M</strong>odal <strong>E</strong>valuation benchmark of MLLMs in <strong>Video</strong> analysis. It is designed to comprehensively assess the capabilities of MLLMs in processing video data, covering a wide range of visual domains, temporal durations, and data modalities. Video-MME comprises **900 videos** with a total of 254 hours, and **2,700 human-annotated question-answer pairs**. Our work distinguishes from existing benchmarks through four key features: 
* *Duration in temporal dimension*. Encompassing both **short- (< 2min)**, **medium- (4min\~15min)**, and **long-term (30min\~60min)** videos, ranging from **11 seconds to 1 hour**, for robust contextual dynamics;
* *Diversity in video types*. Spanning **6 primary visual domains**, i.e., Knowledge, Film & Television, Sports Competition, Life Record, and Multilingual, with **30 subfields** to ensure broad scenario generalizability;
* *Breadth in data modalities*. Integrating multi-modal inputs besides video frames, including **subtitles and audios**, to assess the all-round capabilities of MLLMs;
* *Quality in annotations*. **All data are newly collected and annotated by humans, not from any existing video dataset**, ensuring diversity and quality. 


<p align="center">
    <img src="./asset/sta.jpg" width="100%" height="100%">
</p>

## 📐 Dataset Examples

<p align="center">
    <img src="./asset/Highlights-2.png" width="100%" height="100%">
</p>

<div align='center' >
<details>
<summary> Click to expand more examples</summary>
<p align="center">
    <img src="./asset/Highlights-1.png" width="100%" height="100%">
    <img src="./asset/Highlights-3.png" width="100%" height="100%">
    <img src="./asset/Highlights-4.png" width="100%" height="100%">
</details>
</div>


## 🔍 Dataset

**License**:
```
Video-MME is only used for academic research. Commercial use in any form is prohibited.
The copyright of all videos belongs to the video owners.
If there is any infringement in Video-MME, please email videomme2024@gmail.com and we will remove it immediately.
Without prior approval, you cannot distribute, publish, copy, disseminate, or modify Video-MME in whole or in part. 
You must strictly comply with the above restrictions.
```

Please send an email to **videomme2024@gmail.com**. 🌟


## 🔮 Evaluation Pipeline
📍 **Extract Frames and Subtitles**:

There are a total of **900 videos** and **744 subtitles**, where all long videos have subtitles.

With respect to the setting of adding subtitles, you should only use the subtitles corresponding to the sampled video frames.
For example, if you extract 10 frames per video for evaluation, take the 10 subtitles that corresponding to the time of those 10 frames.

If you have already prepared the video and subtitle file, you could refer to [this script](https://github.com/look4u-ok/video-slicer) to extract the frames and corresponding subtitles.


📍 **Prompt**:

The common prompt used in our evaluation follows this format:

```
This video's subtitles are listed below:
[Subtitles] 
Select the best answer to the following multiple-choice question based on the video. Respond with only the letter (A, B, C, or D) of the correct option. 
[Question]
The best answer is:
```

For the subtitles-free setting, you should remove the subtitle content.


<details>
<summary> Click to expand the prompt examples.</summary>

* With subtitles:

```
This video's subtitles are listed below:
Hi guys, I'm going to show you how to perfectly prepare a ...
Select the best answer to the following multiple-choice question based on the video. Respond with only the letter (A, B, C, or D) of the correct option.
What is the color of the clothing worn by the persons in the video?
A. Black.
B. Gray.
C. Green.
D. Brown.
The best answer is:
```

* Without subtitles:
```
Select the best answer to the following multiple-choice question based on the video. Respond with only the letter (A, B, C, or D) of the correct option.
What is the color of the clothing worn by the persons in the video?
A. Black.
B. Gray.
C. Green.
D. Brown.
The best answer is:
```
</details>


📍 **Evaluation**: 

To extract the answer and calculate the scores, we add the model response to a JSON file. Here we provide an example template [output_test_template.json](./evaluation/output_test_template.json). Once you have prepared the model responses in this format, please refer to the evaluation script [eval_your_results.py](https://github.com/thanku-all/parse_answer/blob/main/eval_your_results.py), and you will get the accuracy scores across video_durations, video domains, video subcategories, and task types. 
The evaluation does not introduce any third-party models, such as ChatGPT.

```bash
python eval_your_results.py \
    --results_file $YOUR_RESULTS_FILE \
    --video_duration_type $VIDEO_DURATION_TYPE \
    --return_categories_accuracy \
    --return_sub_categories_accuracy \
    --return_task_types_accuracy
```
Please ensure that the `results_file` follows the specified JSON format stated above, and `video_duration_type` is specified as either `short`, `medium`, or `long`. If you wish to assess results across various duration types, you can specify multiple types separated by commas or organize them in a list, for example: `short,medium,long` or `["short","medium","long"]`.

📍 **Leaderboard**: 

If you want to add your model to our [leaderboard](https://video-mme.github.io/home_page.html#leaderboard), please send model responses to **bradyfu24@gmail.com**, as the format of [output_test_template.json](./evaluation/output_test_template.json).


## 📈 Experimental Results
- **Evaluation results of different MLLMs.**

<p align="center">
    <img src="./asset/results_of_various_models.png" width="96%" height="50%">
</p>


- **Evaluation results of different MLLMs across different task types.**

<p align="center">
    <img src="./asset/results_of_question_types_0616.png" width="50%" height="50%">
</p>

- **Evaluation results of Gemini 1.5 Pro across different video duration types.**

<p align="center">
    <img src="./asset/results_of_video_type.jpg" width="96%" height="96%">
</p>

- **Evaluation results of Gemini 1.5 Pro across different video sub-types.**

<p align="center">
    <img src="./asset/results_of_video_sub_type.png" width="80%" height="80%">
</p>


## :black_nib: Citation

If you find our work helpful for your research, please consider citing our work.   

```bibtex
@article{fu2024video,
  title={Video-MME: The First-Ever Comprehensive Evaluation Benchmark of Multi-modal LLMs in Video Analysis},
  author={Fu, Chaoyou and Dai, Yuhan and Luo, Yondong and Li, Lei and Ren, Shuhuai and Zhang, Renrui and Wang, Zihan and Zhou, Chenyu and Shen, Yunhang and Zhang, Mengdan and others},
  journal={arXiv preprint arXiv:2405.21075},
  year={2024}
}
```

## 📜 Related Works

Explore our related researches:
-  **[MME]** [MME: A Comprehensive Evaluation Benchmark for Multimodal Large Language Models](https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models/tree/Evaluation)
-  **[Awesome-MLLM]** [A Survey on Multimodal Large Language Models](https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models)

