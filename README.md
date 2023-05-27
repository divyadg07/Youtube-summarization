# Youtube-summarization
from youtube_transcript_api import YouTubeTranscriptApi
import gradio as gr
from gradio.mix import Series

def generate_transcript(url):
    id = url[url.index("=")+1:]
        
    transcript = YouTubeTranscriptApi.get_transcript(id)
    script = ""

    for text in transcript:
        t = text["text"]
        if t != '[Music]':
            script += t + " "
		
    return script

transcriber = gr.Interface(generate_transcript, 'text', 'text')
summarizer = gr.Interface.load("huggingface/sshleifer/distilbart-cnn-12-6")

gradio_ui = Series(transcriber, summarizer,
                  inputs = gr.inputs.Textbox(label = "Enter the YouTube URL below:"),
                  outputs = gr.outputs.Textbox(label = "Transcript Summary"),
                  colour = gr.Blocks(css=".gradio-container {background-color: grey}"),
                  title = "YouTube Transcript Summarizer",
                  theme = "aqua",
                  description = "This application is use to summarize a short YouTube video that has English subtitles.")
               
gradio_ui.launch(share=True)


#This helps to summarize the youtube video files which has captions.
#Language used is English Only
#Import gradio through pip install gradio
