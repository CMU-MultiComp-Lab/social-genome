# Social Genome: Grounded Social Reasoning Abilities of Multimodal Model

Benchmark of performance of large language models on social reasoning video-answering questions. Evaluation server hosted on TBD. 

## Social-IQ 2.0 Dataset 
```
social-genome
├── audio
│   ├── mp3 # will contain mp3 files
│   └── wav # will contain wav files
├── download_all.py
├── frames
├── videos.json # contains video IDs to download 
├── qa_test.json # contains question / answer unlabelled data 
├── requirements.txt
├── transcript # will contain .vtt transcript files
├── trims.json # contains start times for the videos; videos are 60 seconds long from that start second
└── video # will contain .mp4 video files
└── youtube_utils.py
```
Each video ID is from the URL of a YouTube video (for example, https://www.youtube.com/watch?v=-04kGoWIAXk, where the video ID is `-04kGoWIAXk`). The following instructions and `download_all.py` script are modified from the Social-IQ 2.0 Github repository on how to download the videos from the dataset (see [Social-IQ 2.0 repo](https://github.com/abwilf/Social-IQ-2.0-Challenge/tree/main?tab=readme-ov-file#the-social-iq-20-dataset)). 

To download the data, first install all required dependencies:
```
conda create -n social-genome python=3.8 -y && conda activate social-genome
pip install -r requirements.txt
sudo apt update && sudo apt install ffmpeg # or however you need to install ffmpeg; varies per machine
```

Then, run the following to download the dataset
```
python social-genome/download_all.py # this takes about 4 hours to download and 60GB to store the whole dataset. This goes down to 30GB if you run with flag --no_frames
```

## Submission Structure 

`qa_test.json` contains questions and answers for each video. Below is an example of the format of each question: 
```
    {
        "qid": "-04kGoWIAXk_q5_0", # question ID (unique to each question)
        "q": "How does the woman in black feel about the woman in the dress?", # question
        "vid_name": "-04kGoWIAXk", # video ID (unique to each video)
        "ts": "0.00-60.019000",
        "a0": "The woman in black is a fan of the Crazy Trilogy movies.", # answer A
        "a1": "The woman in black is annoyed by the woman in the dress.", # answer B
        "a2": "She acts indifferent, but is actually attracted to her.", # answer C
        "a3": "She is interested as she looks her way." # answer D
    }
```
There is a total of 1486 questions. 

The evaluation server will only accept a pickle file in the following format: 

key = question ID, value = dictionary with the following keys, steps (with the reasoning chain split up into individual steps), modalities (a list of which modalities are present in the step, verbal, visual, vocal, and/or external knowledge), steps_modalities (tuple with the first element being a string for the response and a list of modalities present), and answer (a single letter for which answer choice the model chose as the correct answer)


```
{
    "<qid>" : {
        "steps": [
            "step 1 of reasoning chain", 
            "step 2 of reasoning chain"
        ],
        "modalities": [
            ["modality 1 for step 1", "modality 2 for step 1"], 
            ["modality 1 for step 2", "modality 2 for step 2"], 
            
        ],
        "steps_modalities":[
            ("step 1 of reasoning chain", ["modality 1 for step 1", "modality 2 for step 1"]), 
            ("step 2 of reasoning chain", ["modality 1 for step 2", "modality 2 for step 2"])
        ],
        "answer":"<answer>" # either A, B, C, or D 
    }
}
```
Note that for the "answer" value, you must give a multiple choice answer A, B, C, or D (these match with answer index 0, 1, 2, and 3, respectively). 


## Evaluation Criteria 
Specifics on what metrics we compute on the evaluation server can be found on TBD and our [paper](https://arxiv.org/abs/2502.15109).