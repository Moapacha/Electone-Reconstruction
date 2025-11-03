# Reconstructing Vintage Electone Textures with RAVE

## Materials

### Hardware

* Google Colab GPU A100

### Baseline Dataset

* MusicNet

### Custom Dataset

* 1970s Japanese Electone Music Album from Shigeo Sekito (~90 minutes)

### Model

* RAVE V2
* Sampling Rate: 44.1kHz
* Check `Config.gin` in attached for hyperparameters

## Training Process

The training process followed the official IRCAM RAVE tutorial:
[Training RAVE models on custom data (IRCAM Forum)](https://forum.ircam.fr/article/detail/training-rave-models-on-custom-data/)


Below are TensorBoard screenshots showing various training metrics:

![training metric](screenshots/pred_real.png)

![training metric](screenshots/pred_fake.png)

![training metric](screenshots/multiband_spectral_distance.png)

![training metric](screenshots/fullband_spectral_distance.png)

![training metric](screenshots/feature_matching.png)

![training metric](screenshots/adversarial.png)


## Training Process and Observations

At the beginning, I experimented with several pre-trained RAVE models available from IRCAM and Hugging Face, generating various beats and textures for reference and comparison.

Before training with my own dataset, I tested the workflow using the **MusicNet** dataset and successfully replicated the results presented in IRCAMʼs official documentation. 

After confirming the baseline performance, I proceeded to train **RAVE V2** with my custom 1970s Japanese Electone Music Album dataset (~90 minutes in total).

### Training Behavior

Initially, the model **did not converge**, and there was **no meaningful output** observed in the `audio_val` tab. 

After enabling the **“noise”** configuration in the hyperparameters, the model **gradually began to converge**. 

  - I assume this improvement is due to 1970s Japanese Electone Music Album recordings containing a variety of **textural and noisy components**.

  - For datasets with cleaner or more tonal material, disabling the “noise” option might lead the model to behave more like a **timbre transfer model** rather than a texture generator.

During training, the **“prediction_fake”** and **“prediction_real”** metrics did not behave exactly as shown in the tutorial (they did not diverge early in Phase 2 before converging later). 

  - This could be related to the **small dataset size** or **limited timbral diversity**.

## Results

Despite the irregular metric curves, the trained model successfully produced **interesting rhythmic and glitch-like audio textures** when integrated with **Max/MSP** and **Ableton Live**.

[VintageModel Example](https://drive.google.com/file/d/1-gSzRUgIpglbJRjlSN2Le5sgzmpWDjfS/view?usp=drive_link)

[Prior-VintageModel Example](https://drive.google.com/file/d/1P6-__j0DiLc-q7UqnRcYLkviJqdCvwJJ/view?usp=drive_link)

Both the **trained checkpoint model** and `.ts` are attached for reference and further evaluation.

## Observations

* The smaller dataset likely affected model stability and convergence behavior.
* Nevertheless, qualitative results (audio output) remained musically valuable, especially for experimental electronic textures.

Future work could involve:

* Training a Prior Model
* Expanding the dataset with additional Electone recordings
* Comparing results using RAVE V3 or a different discriminator
