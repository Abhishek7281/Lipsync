## Wav2Lip Setup and Audio Recording
This repository contains code to set up Wav2Lip and record audio in a Colab environment. Wav2Lip is a lip-syncing algorithm that can be used to generate videos with synchronized speech and lip movements.

# Step 1: Setup Wav2Lip
To set up Wav2Lip and download the required dependencies, follow the steps below:

1. Install dependencies and download the pretrained model for Wav2Lip.
2. Clone the Wav2Lip repository.
3. Download the pretrained model for face detection.
# Install dependencies and download the pretrained model for Wav2Lip
!pip install https://raw.githubusercontent.com/AwaleSajil/ghc/master/ghc-1.0-py3-none-any.whl
!cd Wav2Lip && pip install -r requirements.txt
!pip install -q youtube-dl
!pip install ffmpeg-python

# Clone the Wav2Lip repository
!git clone https://github.com/Rudrabha/Wav2Lip

# Download the pretrained model for Wav2Lip
!wget 'https://iiitaphyd-my.sharepoint.com/personal/radrabha_m_research_iiit_ac_in/_layouts/15/download.aspx?share=EdjI7bZlgApMqsVoEUUXpLsBxqXbn5z8VTmoxp55YNDcIA' -O '/content/Wav2Lip/checkpoints/wav2lip_gan.pth'

# Download pretrained model for face detection
!wget "https://www.adrianbulat.com/downloads/python-fan/s3fd-619a316812.pth" -O "/content/Wav2Lip/face_detection/detection/sfd/s3fd.pth"



# Step 2: Audio Recording
The following code sets up audio recording in the Colab environment. You can use this feature to record audio for lip-syncing with Wav2Lip.

# Function to display the audio recording button and record audio
from IPython.display import HTML, Audio
from google.colab.output import eval_js
from base64 import b64decode
import numpy as np
from scipy.io.wavfile import read as wav_read
import io
import ffmpeg

AUDIO_HTML = """
<script>
// ... (omitted for brevity) ...
</script>
"""

def get_audio():
  display(HTML(AUDIO_HTML))
  data = eval_js("data")
  binary = b64decode(data.split(',')[1])

  process = (ffmpeg
    .input('pipe:0')
    .output('pipe:1', format='wav')
    .run_async(pipe_stdin=True, pipe_stdout=True, pipe_stderr=True, quiet=True, overwrite_output=True)
  )
  output, err = process.communicate(input=binary)

  riff_chunk_size = len(output) - 8
  q = riff_chunk_size
  b = []
  for i in range(4):
      q, r = divmod(q, 256)
      b.append(r)

  riff = output[:4] + bytes(b) + output[8:]

  sr, audio = wav_read(io.BytesIO(riff))

  return audio, sr


Step 3: Display Video
The following code is used to display the generated video after processing the lip-syncing.

# Function to display video
from IPython.display import HTML
from base64 import b64encode

def showVideo(path):
  mp4 = open(str(path),'rb').read()
  data_url = "data:video/mp4;base64," + b64encode(mp4).decode()
  return HTML("""
  <video width=700 controls>
        <source src="%s" type="video/mp4">
  </video>
  """ % data_url)


Note: The code provided is designed to work in a Google Colab environment. Make sure to run this code in a Colab notebook to utilize all the features properly.

Now, you can use the get_audio() function to record audio and proceed with Wav2Lip processing to generate lip-synced videos.

## Video Trimming: 
This code allows you to trim a video in the Google Colab environment. Follow the steps below to upload your video, trim it, and preview the trimmed video.

# Step 1: Upload Your Video File
Upload your video file and then copy its path.

# Step 2: Trim the Video (Start, End) Seconds
If you want to trim the video, provide the start and end seconds to specify the duration you want to keep. If you do not want to trim the video, set both 'start' and 'end' to -1.

<i>Note: The trimmed video must have a face on all frames.</i>

# Step 3: Trimming and Previewing the Video
The code will handle trimming and previewing the video based on your input. The resulting trimmed video will be displayed below.

Important: Ensure you have all the required dependencies installed before running the code.

Note: The code provided is designed to work in a Google Colab environment. Make sure to run this code in a Colab notebook to utilize all the features properly.

Now, you can upload your video, trim it as required, and preview the trimmed video.






# Audio Selection - 
This code allows you to select audio for your video in the Google Colab environment. Follow the steps below to either record audio or upload an audio file.

# Step 1: Select Audio Source (Record or Upload)
You have two options to select audio:

# Record: Choose this option to record audio directly using the provided feature. Click the "Record" button to start recording and click again to stop recording.

# Upload: Choose this option to upload an existing audio file. Click the "Upload" button and select the audio file from your local device.

After selecting the audio source, the audio will be displayed for your preview.

Important: Ensure you have all the required dependencies installed before running the code.

# Note: The code provided is designed to work in a Google Colab environment. Make sure to run this code in a Colab notebook to utilize all the features properly.

# Now, you can select the audio for your video by either recording a new one or uploading an existing audio file.






# Video Crunching and Output Preview
This code performs the video crunching process and allows you to preview the output video in the Google Colab environment. Follow the steps below to start the crunching process and preview the output.

# Step 1: Configure Padding and Scaling (Optional)
The following parameters can be configured before starting the crunching process:

pad_top: Padding from the top of the video (in pixels).
pad_bottom: Padding from the bottom of the video (in pixels).
pad_left: Padding from the left of the video (in pixels).
pad_right: Padding from the right of the video (in pixels).
rescaleFactor: Rescale factor for the video (default is 1). You can adjust it to change the video's size.

# Step 2: Smoothing Option (Optional)
You can enable or disable video smoothing by setting the nosmooth parameter. Set it to True to disable smoothing or False to enable smoothing.

# Step 3: Start Crunching and Preview Output
After configuring the parameters, run the code to start the crunching process. The code will generate the output video based on the provided audio and video, along with the specified settings. The generated output video will be displayed below for your preview.

# Note: The code provided is designed to work in a Google Colab environment. Make sure to run this code in a Colab notebook to utilize all the features properly.

Now, you can start the video crunching process and preview the final output video. The download link for the video will also be provided for you to save the generated video.
