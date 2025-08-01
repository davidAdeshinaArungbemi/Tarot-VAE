# Patch-based Variational Autoencoder with Contextual Transformer Encoder

## Motivation

This project began after encountering visually striking tarot card images on Pinterest. I aimed to build a generative model capable of capturing their complex visual patterns.

An initial attempt using a conventional VAE resulted in a model size of approximately 600 MB, which was impractical. To address this, I adopted a patch-based approach that breaks images into smaller patches processed with contextual embeddings, significantly reducing model size.

## Data

- Tarot card images collected from [dataset link].

## Approach and Architecture

- **Patch-based processing:** Images are split into fixed-size patches (e.g., 32×32 pixels).
- **Contextual embedding:** Each patch is embedded along with its neighboring patches within a sliding window context to provide spatial awareness.
- **Variational Transformer Encoder:** Processes the sequence of patch embeddings with attention mechanisms to learn latent distributions.
- **Patch Decoder:** Reconstructs patches from latent codes using transpose convolution layers.
- **Model size:** Approximately 16 MB in `.keras` format, a substantial reduction from the initial model.

### Core Components

```python
# Patch extraction and embedding
patches = tf.image.extract_patches(inputs, sizes=[1,H,W,1], strides=[1,H,W,1], padding='VALID')
flat = tf.reshape(patches, [batch, num_patches, patch_features])
embedded = patch_embed_layer(flat)

# Context gathering: For each patch, gather embeddings of neighboring patches within context window
context_embeddings = gather_context(embedded)

# Variational Transformer Encoder forward pass
z_mean, z_log_var = encoder(contextual_patch_embeddings)
z = reparameterize(z_mean, z_log_var)

# Patch Decoder reconstructs patches from latent z
reconstructed_patches = decoder(z)
