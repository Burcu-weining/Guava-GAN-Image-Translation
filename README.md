# Guava Image Translation with a Conditional GAN

An experimental **image-to-image translation** project that learns to map photographs of **wrapped guavas** to paired **unwrapped guava** photographs. The project uses TensorFlow/Keras and a conditional generative adversarial network (cGAN), with an encoder–decoder generator, skip connections, and a discriminator conditioned on the input image.

## Dataset

I photographed and assembled the dataset used here. The repository includes **40 JPEG photographs forming 20 wrapped/unwrapped pairs**:

| Split | Wrapped | Unwrapped | Pairs |
| --- | ---: | ---: | ---: |
| Train | 10 | 10 | 10 |
| Validation | 10 | 10 | 10 |

Images are in `dataset/{train,val}/{wrapped,unwrapped}/`. Files are paired **by identical filenames within each split**, so keep filenames unchanged. The original notebook prepared the dataset by applying EXIF orientation correction and cropping the top 15% of images. This repository contains the prepared images, not the original raw photos.

**Research limitation:** This is a very small dataset. The validation metrics below are from a single saved notebook run and are not evidence of generalization to a larger or independent sample.

## Model and training

- **Generator:** convolutional encoder–decoder with skip connections
- **Discriminator:** evaluates a concatenated input image / target-or-generated image pair
- **Objective:** adversarial binary cross-entropy + weighted L1 reconstruction loss (`LAMBDA = 30`)
- **Framework:** TensorFlow / Keras, with Pillow, Matplotlib and NumPy
- **Configuration:** 256 × 256 RGB images, batch size 8, 200 epochs configured, Adam learning rate `2e-4` (`beta_1 = 0.5`)

The notebook includes training, sample visualization, and evaluation code.

## Results recorded in the original notebook

| Metric | Reported value |
| --- | ---: |
| Structural Similarity Index (SSIM) | 0.468 |
| Peak Signal-to-Noise Ratio (PSNR) | 14.36 dB |

These are outputs preserved from the original experiment **before notebook output cleanup**. They have not been independently reproduced in this packaged version. Re-running the notebook trains a new model and may yield different results.

## Run in Google Colab

1. Create a GitHub repository and upload these files. Open `GAN.ipynb` in Colab (e.g., use **File → Open notebook → GitHub**).
2. In a Colab cell **before running the notebook**, clone your own repository and change into it:

   ```python
   git clone https://github.com/Burcu-weining/Guava-GAN-Image-Translation.git
   cd Guava-GAN-Image-Translation  
    ```

3. Select **Runtime → Change runtime type → GPU** if available.
4. Install dependencies if missing: `!pip install -r requirements.txt`.
5. Run the notebook from top to bottom. Training 200 epochs can take time; reduce `EPOCHS` only if experimenting with a shorter run.

Alternatively, run it locally from the repository root with a compatible Python environment. The dataset must be accessible at `dataset/` relative to the notebook's current working directory.

**Note:** Trained model weights/checkpoints are *not* included. Running training is required to obtain a trained generator. The repository does not currently provide a hosted inference demo.

## Repository files

- `GAN.ipynb`: cleaned and portable training/evaluation notebook
- `dataset/`: the paired train and validation images
- `requirements.txt`: Python package requirements
- `.gitignore`: common generated files to exclude from version control

## Credit and use

Project implementation and original photographs by the repository author. Dataset images are shared here for educational demonstration. No third-party dataset license is implied. If you reuse or redistribute the photographs, please contact the author for permission.
