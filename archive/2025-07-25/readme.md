# Notebooks explanation:

- [Facenet as primary model](https://github.com/ARoyyanF/deepface-py/blob/main/archive/2025-07-25/variations/augment-brightness-only/facenet/experimental_pipeline.ipynb)
- [Ghostfacenet as primary model](https://github.com/ARoyyanF/deepface-py/blob/main/archive/2025-07-25/variations/augment-brightness-only/ghostfacenet/experimental_pipeline.ipynb)

## 1. Foundational Goal: Creating a Robust and Adaptable System

The primary goal is to build a face recognition system that is not only accurate but also resilient to real-world variations. This is achieved through several core concepts:

Model Redundancy (Primary & Fallback): The system doesn't rely on a single face recognition model. It uses a primary model (like Facenet) for its high accuracy and a fallback model (like GhostFaceNet) as a backup. If the primary model fails to detect a face or generate an embedding for any reason (e.g., poor image quality, unusual angle), the system automatically tries again with the fallback. This significantly increases the chances of a successful analysis, a metric tracked as the Processing Success Rate. The rate at which this backup is used is also tracked as the Fallback Usage Rate.

Adaptability through Configuration: All key components—models, face detectors, and distance metrics—are defined in a central configuration. This allows for easy experimentation to find the optimal combination for a specific use case without rewriting the logic.

## 2. Building a Comprehensive Reference Database

The system's accuracy is highly dependent on the quality of its reference data (the "known faces"). Instead of relying on a single photo per person, the pipeline builds a more comprehensive and robust database.

One-to-Many Identity Representation: Each person in the database is represented by multiple images. This provides a more complete picture of an individual's appearance under different conditions.

Image Augmentation: For every reference image provided, the system automatically creates several slightly altered versions with different lighting conditions (brighter and darker variations). This process, known as augmentation, artificially expands the database. The goal is to train the system to recognize a person even if the lighting in a new photo is different from the original reference shots. A system trained on these variations is less likely to fail due to shadows or bright lights, improving the overall Recognition Rate.

## 3. Face Processing: From Image to Vector Embedding

A face image cannot be directly compared to another. It must first be converted into a standardized, numerical format called an embedding or a vector. This is the core of modern face recognition.

Detection and Alignment: First, a face detector (like mtcnn) scans the image to locate the face. It then performs alignment, rotating and cropping the face so that key features (eyes, nose, mouth) are in a standard position. This standardization is crucial for ensuring that the model compares "apples to apples," leading to higher accuracy.

Embedding Generation: The aligned face is then fed into the neural network model (e.g., Facenet). The model processes the facial features and outputs a vector—a list of numbers (e.g., 128 or 512 numbers) that mathematically represents the unique characteristics of that face.

JSON Vector Database: All these embeddings (from both original and augmented images) are stored in a structured JSON file. Each embedding is linked to a person's ID and includes metadata like which model was used and the source image. This file acts as the system's "memory" of known faces.

## 4. Recognition: The Matching Process

When a new image (a "query image") needs to be identified, it goes through the same face processing pipeline to generate its own embedding. This new embedding is then compared against every embedding stored in the JSON database.

Distance Metric (Cosine Similarity): The comparison isn't a simple check for equality. It uses a distance metric, such as cosine similarity, to calculate how "close" the query embedding is to each embedding in the database. A smaller distance implies a higher similarity between the faces.

Finding the Best Match: The system calculates the distance between the query embedding and all reference embeddings. It then identifies the reference embedding with the smallest distance (i.e., the highest similarity). The person associated with that best-match embedding is considered the recognized identity.

Confidence Score and Threshold: The distance score is converted into a more intuitive Confidence Score (typically between 0 and 1). To be considered a valid match, this score must be above a predefined similarity threshold. If the best match's score is below this threshold, the system concludes that the person is "not recognized," even if they are the closest match. This prevents incorrect identifications from weakly similar faces.

## 5. Generating Performance Metrics: The Final Analysis

After processing all query images, the system aggregates the results to evaluate its own performance. This is how the final metrics are derived:

Processing Success Rate: A simple percentage of how many query images could be successfully processed (i.e., an embedding was generated) versus how many failed.

Recognition Rate: Of the successfully processed images, this metric calculates the percentage that were matched to a person in the database with a confidence score above the threshold.

Mean Confidence and Distance: The system calculates the average confidence score and average distance for all successful recognitions. A high mean confidence and low mean distance indicate that the system is performing well and is "sure" about its matches.

Confidence Distribution: The results are categorized into confidence buckets (e.g., high, medium, low). This helps to understand not just the average performance, but also the consistency of the system.

Per-Person Performance: The system also breaks down the recognition statistics for each individual in the database, calculating their average confidence score. This can reveal if the system struggles to recognize certain individuals, perhaps due to poor quality reference images.
