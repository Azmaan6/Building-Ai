# AI Voice Detector

Final project for the Building AI course

## Summary

A machine learning system that listens to an audio clip and predicts whether the voice is a real human recording or AI-generated (synthetic/deepfake) speech, helping people spot voice cloning and audio fraud before they're fooled by it.


## Background

Voice cloning and text-to-speech tools have become good enough to convincingly imitate real people, and this creates serious problems:

* Scammers use cloned voices of relatives or executives to trick people into sending money ("grandparent scams," fake CEO calls)
* Fake audio clips of public figures spread misinformation and can damage reputations
* Call centers, banks, and voice-authentication systems can be fooled by synthetic voices
* Regular people have no easy way to check "is this voice really them?"

I got interested in this because deepfake audio is much harder for humans to detect than deepfake video — we don't have obvious visual glitches to rely on, just subtle unnaturalness in tone, rhythm, or background noise. As voice-cloning tools get cheaper and more realistic, having an accessible detection tool feels increasingly important for personal safety and trust in media.


## How is it used?

The solution is used as follows:

1. A user uploads or records a short audio clip (a few seconds of speech)
2. The system extracts audio features from the clip (e.g. spectrograms, pitch patterns, and other acoustic features)
3. A trained classifier analyzes these features and outputs a prediction: **Human** or **AI-generated**, along with a confidence score
4. The result is shown to the user, optionally with a short explanation of which signals influenced the decision

**Who needs this and when?**
* Everyday users who receive a suspicious phone call or voice message and want a quick sanity check
* Journalists and fact-checkers verifying whether an audio clip circulating online is authentic
* Companies with voice-based authentication who want an extra fraud-detection layer
* Social media platforms wanting to flag potentially synthetic audio content

The tool would ideally be available as a simple web app or browser extension where a user drags in an audio file and gets a result within seconds — no technical knowledge required.

```
def predict_voice(audio_features, model):
    prediction = model.predict(audio_features)
    confidence = model.predict_proba(audio_features)

    if prediction == 1:
        print("AI-generated voice detected (%.2f%% confidence)" % (confidence * 100))
    else:
        print("Human voice detected (%.2f%% confidence)" % (confidence * 100))

main()
```


## Data sources and AI methods

Training a detector like this requires a labeled dataset of both real and synthetic voice samples:

* [ASVspoof Challenge datasets](https://www.asvspoof.org/) — widely used benchmark datasets of genuine and spoofed/synthetic speech
* [Fake-or-Real (FoR) Dataset](https://bil.eecs.yorku.ca/datasets/) — a public dataset of real vs AI-generated speech samples
* Self-collected samples: real recordings (with consent) paired with clips generated using open text-to-speech and voice-cloning tools, to broaden coverage of newer AI voice generators

**Methods:**

| Component | Description |
| ----------- | ----------- |
| Feature extraction | MFCCs, spectrograms, pitch/prosody features extracted from raw audio |
| Model | A classifier such as a CNN, RNN/LSTM, or a fine-tuned pretrained audio model (e.g. Wav2Vec2) trained to distinguish real vs. synthetic speech |
| Evaluation | Accuracy, precision/recall, and equal error rate (EER), the standard metric used in spoof-detection research |

[Wav2Vec2 documentation](https://huggingface.co/docs/transformers/model_doc/wav2vec2)


## Challenges

This project does not solve everything and has important limitations:

* Detection accuracy will likely drop as voice-generation technology improves — this is an ongoing arms race, not a one-time fix
* A model trained on today's AI voice generators may not generalize well to tomorrow's newer, unseen generators
* False positives (flagging a real human voice as AI) could unfairly damage someone's credibility
* False negatives could give people false confidence that a scam call is legitimate
* Ethical considerations: the same underlying technology used to detect cloned voices could theoretically be studied by bad actors to make their fakes harder to detect, so responsible disclosure of methods matters
* Privacy: any tool that processes people's voice recordings must handle that audio data responsibly and securely


## What next?

To grow this project further, I would need:

* A larger and more diverse dataset covering many languages, accents, and the latest AI voice generation tools
* Access to more computing power (GPUs) to train larger, more accurate models
* Feedback from real-world testing (e.g. journalists or call centers) to understand failure cases
* Collaboration with people who have expertise in audio signal processing and adversarial machine learning
* Eventually, a simple web app or API so anyone can use the tool without coding knowledge


## Acknowledgments

* Building AI course by Reaktor Innovations and University of Helsinki, for the project framework and inspiration
* [ASVspoof Challenge](https://www.asvspoof.org/) for open research and datasets on voice spoofing detection
* Open-source text-to-speech and speech-recognition communities whose research made this project possible
* Do not use code, images, data, etc. from others without permission. When you have permission to use other people's materials, always mention the original creator and the open source / Creative Commons licence they've used
