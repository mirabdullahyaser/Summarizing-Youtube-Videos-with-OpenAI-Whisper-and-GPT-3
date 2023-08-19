# YouTube Video Summarizer with OpenAI Whisper and GPT

This Python script allows you to summarize YouTube videos using OpenAI's Whisper for audio transcription and GPT for generating concise video summaries. The pipeline involves downloading a YouTube video's audio, transcribing it with Whisper, and then generating a summary using GPT.

## Prerequisites

Before you begin, make sure you have the necessary Python libraries and an OpenAI API key:

- Python (>=3.6)
- `whisper` library (for audio transcription)
- `openai` library (for GPT)
- `pytube` library (for downloading YouTube videos)

You should also set up your OpenAI API key as an environment variable.

## Script Overview

### 1. Downloading YouTube Video

The script starts by downloading the audio from a YouTube video. It uses the `pytube` library to fetch the video, select the audio-only stream, and save it as an audio file.

```python
youtube_video = YouTube(YOUTUBE_VIDEO_URL)
streams = youtube_video.streams.filter(only_audio=True)
# taking first object of lowest quality
stream = streams.first()
stream.download(filename=OUTPUT_AUDIO)
```

### 2. Audio Transcription with Whisper

Next, the audio is transcribed using the Whisper model. The whisper library is used to load the Whisper model, transcribe the audio, and obtain the transcript.

```python
model = whisper.load_model(WHISPER_MODEL)
transcript = model.transcribe(OUTPUT_AUDIO.as_posix())
transcript = transcript['text']
print(f'Transcript generated: \n{transcript}')
```

### 3. Summarization with GPT

The script then uses GPT to generate a concise summary of the transcribed text. It provides a system prompt and a user prompt to instruct GPT on generating a meaningful summary.

```python
model = whisper.load_model(WHISPER_MODEL)
transcript = model.transcribe(OUTPUT_AUDIO.as_posix())
transcript = transcript['text']
print(f'Transcript generated: \n{transcript}')    system_prompt = "I would like for you to assume the role of a Life Coach"
    user_prompt = f"""Generate a concise summary of the text below.
    Text: {transcript}
    
    Add a title to the summary.

    Make sure your summary has useful and true information about the main points of the topic.
    Begin with a short introduction explaining the topic. If you can, use bullet points to list important details,
    and finish your summary with a concluding sentence"""
    #
    print('summarizing ... ')
    response = openai.ChatCompletion.create(
        model='gpt-3.5-turbo-16k',
        messages=[
            {'role': 'system', 'content': system_prompt},
            {'role': 'user', 'content': user_prompt}
        ],
        max_tokens=4096,
        temperature=1
    )
    #
    summary = response['choices'][0]['message']['content']
```

### Usage

- Set up your OpenAI API key as an environment variable.
- Install the required Python libraries (whisper, openai, pytube) using pip.
- Modify the script by replacing OPENAI_API_KEY and YOUTUBE_VIDEO_URL with your OpenAI API key and the URL of the YouTube video you want to summarize.
- Run the script using python summarize_youtube_videos.py.

### Sample Usage

- For the youtube [podcast](https://www.youtube.com/watch?v=GNd12j-CGeQclear) clip of Joe Rogan with Dave Chappelle dicussing 'Hollywood is Not Real Life, Celebrities, Fame is weird'. It generated below mentioned summary
```python
'''
Title: The Realities of Fame and the Importance of Being True to Yourself

Summary:
This conversation revolves around the concept of fame and the misconceptions associated with it. The speaker emphasizes that despite the glitz and glamour of Hollywood, celebrities are just like normal people, and it is unrealistic to expect them to maintain a constant facade of perfection. The conversation also explores the speaker's personal journey of self-discovery and his desire to use his success for something more meaningful. The importance of forgiveness, both towards others and oneself, is highlighted. The speaker suggests that being of service to others can bring fulfillment and challenges the notion that fame should be pursued endlessly. The conversation concludes by discussing the distinction between fame and celebrity, and the importance of staying true to oneself amidst external scrutiny. The speaker admires the courage of the person he is speaking to for being relentlessly authentic in the public eye.

Introduction: This conversation delves into the realities of fame and the speaker's desire to go beyond the superficialities of success.

- Fame in Hollywood and the normalcy of celebrities

4. The treadmill of seeking more fame:
- Rejecting the pursuit of increasing celebrity status in the twilight of one's life
- Encouraging a focus on personal growth and meaningful experiences

5. Authenticity and self-discovery:
- Differentiating between fame as external circumstances and one's true self
- Acknowledging the courage required to maintain authenticity in the face of scrutiny

Conclusion: The conversation highlights the speaker's insights on fame, the importance of self-discovery, and the need to stay true to oneself in the public eye.
'''
```

### Contact

If you have any questions, or suggestions, or would like to discuss this project, feel free to get in touch with me:

- [Email](mailto:mirabdullahyaser@gmail.com)
- [LinkedIn](https://www.linkedin.com/in/mir-abdullah-yaser/)

I'm open to collaboration and would be happy to connect!

Mir Abdullah Yaser
