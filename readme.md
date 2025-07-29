# [Notebook](https://github.com/ARoyyanF/deepface-py/blob/main/experimental_pipeline.ipynb) explanation

> [!NOTE]
> the notebook is designed to be adaptable to variations, below are some links to results of other simulated variations
>
> augment brightness & sphere:
>
> - [facenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere/model/facenet/recognition_results.json)
> - [facenet+ghostfacenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere/model/facenet%2Bghostfacenet/recognition_results.json)
> - [ghostfacenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere/model/ghostfacenet/recognition_results.json)
>
> augment brightness & sphere with resized input
>
> - [facenet+arcface+ghostfacenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere-resize/model/Facenet%2BArcFace%2BGhostFaceNet/recognition_results.json)
> - [facenet+ghostfacenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere-resize/model/Facenet%2Bghostfacenet/recognition_results.json)
> - [sface+vgg-face+ghostfacenet](https://github.com/ARoyyanF/deepface-py/blob/main/variations/augment-brightness-sphere-resize/model/SFace%2BVGG-Face%2Bghostfacenet/recognition_results.json)

## Main Concepts

- Multi-Model Approach for Robustness: The system is not reliant on a single face recognition model. It employs a primary model (Facenet) and has designated fallback models (ArcFace, GhostFaceNet). If the primary model fails to produce a confident result, the system automatically switches to the fallback models in a prioritized order. This significantly improves the system's reliability and accuracy in challenging conditions.

- Image Augmentation: To better handle real-world scenarios where lighting and angles are not perfect, the system uses image augmentation. It creates multiple variations of each person's images with different brightness levels and simulated 3D lighting effects (sphere lighting). This creates a more diverse and robust database of faces.

- JSON Database for Embeddings: Instead of a complex database, the system uses a simple and human-readable JSON file to store the "face embeddings". An embedding is a numerical representation of a face, and this method provides a lightweight way to manage the facial data.

- Enhanced Fallback Mechanism: The system has an intelligent fallback mechanism that goes beyond simple failure. It can detect ambiguity in the results. For instance, if a face is very similar to two different people in the database, or if there are multiple good matches, the system will use a fallback model to get a more definitive answer.

- Comprehensive Evaluation: A significant portion of the notebook is dedicated to a thorough evaluation of the system's performance. It calculates various metrics and generates visualizations to provide a deep understanding of the system's accuracy, efficiency, and the effectiveness of its different components.

## Methods Applied

- Face Detection and Alignment: The system first locates the face in an image. Then, it aligns the face so that features like the eyes and nose are in a consistent position. This standardization is crucial for accurate comparison.

- Face Embedding Generation: This is the core of the recognition process. A deep learning model is used to convert the aligned face into a vector of numbers, known as an embedding. The key principle is that faces of the same person will have mathematically similar embeddings.

- Building the Database: The system processes a folder of images for each known person. For each image, it generates the original and augmented versions and then creates embeddings for all of them using the primary and fallback models. These embeddings, along with metadata, are stored in the JSON database.

- Similarity Matching: When a new image (a query image) is provided, the system generates an embedding for it. This query embedding is then compared against all the embeddings in the database using a distance metric (like cosine distance). A smaller distance signifies a higher similarity.

- Top-K Matching and Recognition: The system identifies the top 5 most similar people from the database and presents them as potential matches, along with their distance scores. A match is considered a "recognition" if the distance is below a predefined threshold for the specific model used.

## Results Analysis and Visualization:

After processing all the query images, the notebook analyzes the results to calculate performance metrics such as:

- Recognition Rate: The percentage of correctly identified faces.

- Confidence Distribution: A breakdown of how confident the system was in its matches.

- Model Usage: Which face recognition models were used most frequently.

- Per-Person Performance: How well the system performed for each individual in the database.

These analyses are then visualized using various charts and plots for an easy-to-understand evaluation of the system's effectiveness.

# Other notebooks

[Facenet with no augmentation](https://github.com/ARoyyanF/deepface-py/blob/main/archive/2025-07-24/readme.md)

[Facenet with brightness augmentation](https://github.com/ARoyyanF/deepface-py/blob/main/archive/2025-07-25/readme.md)
