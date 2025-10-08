The ASVspoof 2019 Logical Access (LA) database is organized for the automatic speaker verification spoofing and countermeasures challenge and contains audio data, protocols, and baseline scores structured as follows:

### Directory Structure
- **LA/**: Main folder for Logical Access data.
  - **ASVspoof2019_LA_asv_protocols**: ASV system protocol files for trials and enrollment.
  - **ASVspoof2019_LA_asv_scores**: Baseline ASV system similarity scores.
  - **ASVspoof2019_LA_cm_protocols**: Countermeasure protocol files defining trial lists.
  - **ASVspoof2019_LA_train, ASVspoof2019_LA_dev, ASVspoof2019_LA_eval**: Audio files for training, development, and evaluation, respectively.
  - **README.LA.txt**: Documentation for the LA subset.

### Audio Files
- Audio files are in **FLAC format**, 16 kHz sampling rate, 16-bit.
- Naming conventions:
  - **LA_T_*.flac**: Training set audio
  - **LA_D_*.flac**: Development set audio
  - **LA_E_*.flac**: Evaluation set audio

### Protocols

#### Countermeasure (CM) Protocols
- Located in **ASVspoof2019_LA_cm_protocols**.
- Contain trial lists in ASCII format for training, development, and evaluation.
- Format of each line:

| SPEAKER_ID | AUDIO_FILE_NAME | SYSTEM_ID | - | KEY     |
|------------|-----------------|-----------|---|---------|
| 4-digit ID | File name       | "-"       | - | bonafide/spoof |

- SYSTEM_ID corresponds to spoofing system IDs (A01-A19) or is blank for bonafide speech.
- Spoofing systems include various text-to-speech (TTS) and voice conversion (VC) methods:
  - Examples: A01 = TTS neural waveform model, A05 = VC vocoder, A07 = TTS vocoder+GAN, etc.

#### ASV Protocols
- Located in **ASVspoof2019_LA_asv_protocols**.
- Files named as `ASVspoof2019.LA.asv.<dev/eval>.<m/f/gi>.<trl/trn>.txt`.
- Trial files specify:
  - Claimed speaker ID
  - Test file ID
  - Spoof attack ID or 'bonafide'
  - Label: target, nontarget, or spoof
- Enrollment files list speaker ID and corresponding enrollment audio files.

### Baseline ASV Scores
- Located in **ASVspoof2019_LA_asv_scores**.
- Scores files for dev and eval sets provide similarity scores between test and enrolled speaker data.
- Format per line:

| CM_KEY     | ASV_KEY     | SCORE            |
|------------|-------------|------------------|
| bonafide/A01–A19 | target/nontarget/spoof | similarity score |

This structure supports research on spoofing detection and ASV robustness by providing audio samples, detailed protocols, and baseline performance metrics.
