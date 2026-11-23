# Meeting Intelligence and Action Item Extraction

A Colab based NLP project that turns a meeting transcript or sample recording into a transcript, speaker labels, action items, sentiment labels, named entities, and a meeting report. The repository also contains a standalone dashboard for exploring the notebook’s sample meeting.

## Contents

- `Copy_of_Meeting_Intelligence_NLP_Project.ipynb` — the original Google Colab notebook and its NLP/audio pipeline.
- `meeting-intelligence-dashboard.html` — a self-contained dashboard with the sample audio and a synchronized sample playback demo.

## What the notebook does

The notebook demonstrates several stages of meeting analysis:

1. **Text preparation:** cleans and normalizes transcripts, tokenizes and lemmatizes text, and demonstrates Bag of Words, TF-IDF, and LDA topic modeling.
2. **Audio transcription:** creates a synthetic, three-voice sample meeting, then transcribes audio with Whisper. The notebook also includes an AWS Transcribe path.
3. **Speaker diarization:** uses pyannote.audio to identify speaker turns and aligns those turns to transcript segments.
4. **Meeting insights:** uses rule-based extraction and a Groq-hosted language model to produce a summary, key points, decisions, and action items with owners and deadlines.
5. **NLP analysis and evaluation:** runs VADER sentiment analysis and spaCy named entity recognition, then compares example predictions with manually supplied labels.
6. **Report output:** assembles a text report with the summary, discussion points, decisions, action items, and sentiment counts.

The notebook’s metrics are demonstrations on small, manually labeled examples. They are not a guarantee of accuracy on other meetings.

## Run the notebook in Google Colab

1. Upload the notebook to Google Drive, open it with Google Colab, and choose **Runtime → Run all**. Run cells in order; the notebook installs packages and creates intermediate audio and analysis outputs.
2. Select a GPU runtime for the Whisper and pyannote audio sections. CPU execution may be slower.
3. For the Groq section, add a Colab Secret named `GROQ_API_KEY` and allow the notebook to access it.
4. For pyannote diarization, add a Colab Secret named `HF_TOKEN`. You may need to accept the model’s access conditions on Hugging Face before loading `pyannote/speaker-diarization-3.1`.
5. The AWS examples read `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` (if applicable), and `AWS_DEFAULT_REGION` from Colab Secrets. These are only needed for AWS cells. Configure an S3 bucket and Transcribe permissions that you control, and update the example bucket name in the notebook before running those cells.
6. The notebook’s audio-generation cell creates `sample_meeting.mp3` using text-to-speech. If using your own audio instead, update the later transcription and diarization cells to point to that file.

Keep credentials in Colab Secrets or another secret manager. Do not paste keys into notebook cells or commit them to GitHub. Some notebook dependencies and hosted model names can change; if a setup cell fails, check the package or provider’s current instructions.

## Open the dashboard

Open `meeting-intelligence-dashboard.html` in a modern browser. Click **Start live demo** to play the bundled sample audio and reveal transcript lines after each utterance. The action list and speaker activity update as the sample progresses; transcript search and action checkboxes are also available.

The dashboard is a static front end. Its live demo replays the included sample; it does not connect to a microphone, conferencing app, Whisper, pyannote, AWS, or Groq. Its in-browser action extraction uses simple phrase rules. To analyze new meetings or stream real audio, connect a transcription and diarization backend and replace the sample data.

## Deployment

The dashboard is deployed on Vercel as a static site. The current Vercel project is not linked to this GitHub repository for automatic redeploys; publish changes by redeploying or connect the project to GitHub in Vercel settings. The Vercel team currently protects deployments with SSO, so visitors may need team access.
