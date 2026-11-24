# AI Image Understanding & Content Matching Engine

## 1. Project Overview

The AI Image Understanding & Content Matching Engine is a backend system designed to automatically understand images, generate structured metadata, and match images to relevant blog posts using semantic similarity.

The system will also include a mismatch guard to prevent irrelevant or unsuitable images from being recommended.

## 2. Project Objectives

- Automatically classify images using Vision AI.
- Generate structured image metadata, tags, captions and confidence scores.
- Generate embeddings for images and blog posts.
- Match images to blog posts using semantic similarity.
- Reject irrelevant image and blog post combinations.
- Explain why a match is accepted or rejected.
- Process images in background batch jobs.
- Track AI processing costs.
- Provide an API for reviewing, approving and rejecting image suggestions.
- Evaluate matching performance using a labeled dataset.

## 3. Technology Stack

| Technology | Purpose |
|---|---|
| Node.js | Backend runtime |
| Express.js | API framework |
| PostgreSQL | Persistent database |
| Docker | Database environment |
| Gemini Vision AI | Image understanding and structured metadata |
| Gemini Embeddings | Semantic vector representations |
| Zod | Input and AI output validation |
| Git and GitHub | Version control and project hosting |
| Node.js Test Runner | Automated testing |

## 4. System Architecture

The system follows a layered backend architecture.

1. Image ingestion layer receives and validates image inputs.
2. Background job layer processes image batches.
3. AI vision layer generates structured image metadata.
4. Validation layer validates AI output using Zod.
5. Persistence layer stores image metadata, blog posts, embeddings and matching records.
6. Embedding layer generates vector representations.
7. Matching engine calculates semantic similarity.
8. Mismatch guard determines whether a candidate is safe and relevant.
9. Review API allows users to inspect, approve or reject suggestions.
10. Evaluation layer measures matching accuracy.

## 5. Image Metadata Schema

Each processed image should contain:

- subject
- category
- attributes
- caption
- confidence

Example:

```json
{
  "subject": "Red fox",
  "category": "wildlife",
  "attributes": ["red fur", "forest", "animal"],
  "caption": "A red fox standing in a forest clearing",
  "confidence": 0.94
}
```

AI-generated metadata must be validated before being stored.

Low-confidence classifications will be flagged for review rather than automatically accepted.

## 6. Semantic Matching Strategy

The system will compare image descriptions and blog post content using embeddings.

The matching process will:

1. Generate an embedding from the image caption and metadata.
2. Generate an embedding from a blog post's title and content.
3. Calculate similarity between the embeddings.
4. Rank candidate blog posts by similarity.
5. Pass the candidates through the mismatch guard.
6. Return accepted suggestions or rejection explanations.

The system should recognize conceptually related terms and descriptions rather than relying only on exact keyword matches.

For example, an image of a red fox may match a post about foxes even when the post uses a scientific name or different wording.

## 7. Mismatch Guard

The mismatch guard is the reliability layer of the matching system.

It will consider:

- Semantic similarity score.
- Image subject.
- Blog post category.
- Classification confidence.
- Configurable matching thresholds.

The guard will:

- Accept sufficiently relevant matches.
- Reject incompatible subjects or categories.
- Reject low-confidence classifications.
- Reject candidates below the similarity threshold.
- Explain why a match was rejected.
- Return no confident match when no candidate meets the requirements.

## 8. Background Processing

Images will be processed using background jobs.

The processing workflow will support:

- Batch image processing.
- Job status tracking.
- Retry mechanisms.
- Failed job reporting.
- Processing progress.
- AI cost tracking.
- Validation failures.

## 9. Database Design

The database will contain the following core entities:

- Images
- Image tags or metadata
- Blog posts
- Image embeddings
- Blog post embeddings
- Matching suggestions
- Reviews
- AI processing jobs
- AI usage and cost records

Database indexes will be added to support efficient lookups and matching workflows.

## 10. Review API

The review API will allow users to:

- View image metadata.
- View suggested blog post matches.
- View similarity scores.
- View explanations for accepted and rejected matches.
- Approve a suggested match.
- Reject a suggested match.
- Review images flagged for low confidence.

## 11. Evaluation

The project will use a small labeled evaluation dataset containing images and their correct blog post matches.

Evaluation will measure Top-1 Precision.

Top-1 Precision measures how often the system's highest-ranked suggestion is correct when it makes a prediction.

The evaluation results will be documented in the project README.

## 12. Reliability and Security

- Validate API inputs.
- Validate AI outputs.
- Protect API credentials using environment variables.
- Never commit secrets.
- Retry failed background jobs.
- Track AI costs.
- Prevent incorrect matches from being automatically accepted.
- Document system limitations.

## 13. Project Scope

The initial dataset will contain approximately 40–50 images across at least four categories.

The project will include at least 10 blog posts and a small labeled evaluation dataset.

The initial implementation will focus on demonstrating a reliable backend matching engine rather than building a full frontend application.

## 14. Expected Outcome

The completed system will demonstrate an AI-powered backend capable of understanding images, generating structured metadata, matching images to blog content, rejecting incorrect matches, tracking AI usage and providing a review workflow.

