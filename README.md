# CloudSat-near-surface-profile-reconstruct
A research project developing a Transformer-based model to reconstruct global near-surface hydrometeor profiles in CloudSat mission's blind zone. The model follows a pre-training fine-tune approach for transfer learning to overcome the domain shift along the horizontal and vertical dimensions while "flying".


![Alt text](model_training_vertical.jpg "Model training paradigm")


![Alt text](model_architecture.jpg "Model architecture diagram")
Caption: Model architecture. Granules are sampled randomly, but the footprints are always adjacent to ensure the conservation of the horizontal spatial autocorrelation. The profiles are flattened through a 3D-tokenizer along 1) footprint, 2) channel, and 3) vertical bin in sequence. Positional encodings for the three dimensions are added in the same order. Query, key, and value are embedded vectors of each pixel token in the learned vector space, based on which the attention encoder headers capture the relationships between tokens pairwise to yield a contextualized representation of the current “climate scene”. The decoder headers read the existing information (from both given and already-predicted) and predict the next token in the blind zone based on the "scene knowledge" from the encoder headers. The predicted token is then mapped back to the blind zone, and then appended to the tokenized vector. The process inpaints the blind zone in sequence autoregressively, from top to bottom, from left to right. The window size for one “climate scene” is constrained to 5-by-5 based on the quasi-Markov assumption to reduce running time. The "scene knowledge" from the encoder and the existing information read by the decoder are dynamically updated accordingly.
