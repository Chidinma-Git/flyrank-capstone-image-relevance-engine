# Database Design

## 1. Overview

The AI Image Understanding & Content Matching Engine will use PostgreSQL as its relational database.

The database will store images, image metadata, blog posts, embeddings, match suggestions, reviews, background processing jobs and AI usage records.

## 2. Database Tables

### 2.1 Images

Stores uploaded or imported images and their classification results.

Fields:

- id: Primary key
- filename: Image filename
- image_path: Local storage path
- subject: Identified subject
- category: Image category
- caption: AI-generated caption
- confidence: Classification confidence
- processing_status: Processing status
- created_at: Creation timestamp
- updated_at: Last update timestamp

### 2.2 Blog Posts

Stores blog content used for matching.

Fields:

- id: Primary key
- title: Blog post title
- content: Blog post content
- category: Blog category
- status: Publication or availability status
- created_at: Creation timestamp
- updated_at: Last update timestamp

### 2.3 Image Tags

Stores individual tags associated with images.

Fields:

- id: Primary key
- image_id: Foreign key referencing images
- tag: Tag text

### 2.4 Embeddings

Stores vector representations of image descriptions and blog post content.

Fields:

- id: Primary key
- entity_type: Image or blog post
- entity_id: Related entity ID
- embedding: Vector representation
- model_name: Embedding model used
- created_at: Creation timestamp

### 2.5 Match Suggestions

Stores matching candidates and matching decisions.

Fields:

- id: Primary key
- image_id: Foreign key referencing images
- blog_post_id: Foreign key referencing blog_posts
- similarity_score: Semantic similarity score
- confidence_score: Classification confidence used in evaluation
- decision: Accepted or rejected
- explanation: Reason for the decision
- created_at: Creation timestamp

### 2.6 Reviews

Stores human review decisions.

Fields:

- id: Primary key
- suggestion_id: Foreign key referencing match_suggestions
- review_status: Approved or rejected
- reviewer_notes: Review notes
- reviewed_at: Review timestamp

### 2.7 Processing Jobs

Tracks background image processing.

Fields:

- id: Primary key
- job_type: Job type
- status: Pending, processing, completed or failed
- total_items: Total number of items
- processed_items: Number of completed items
- retry_count: Number of retries
- error_message: Error details
- created_at: Creation timestamp
- updated_at: Last update timestamp

### 2.8 AI Usage

Tracks AI calls and estimated costs.

Fields:

- id: Primary key
- provider: AI provider
- model_name: Model used
- operation: Vision or embedding operation
- input_units: Usage units or tokens
- output_units: Usage units or tokens
- estimated_cost: Estimated cost
- image_id: Optional related image
- blog_post_id: Optional related post
- created_at: Creation timestamp

## 3. Relationships

- One image can have multiple tags.
- One image can have multiple matching suggestions.
- One blog post can receive multiple matching suggestions.
- One matching suggestion can have a review.
- Processing jobs track batches of image processing tasks.
- AI usage records track individual AI operations.
- Images and blog posts can have embeddings.

## 4. Indexes

The database will use indexes to improve query performance.

Planned indexes:

- Images processing_status
- Images category
- Blog posts category
- Image tags image_id
- Match suggestions image_id
- Match suggestions blog_post_id
- Match suggestions similarity_score
- Processing jobs status
- AI usage created_at

## 5. Data Integrity

- Primary keys uniquely identify records.
- Foreign keys maintain relationships between tables.
- Required fields will be constrained.
- Confidence and similarity scores will be validated.
- Review records will reference existing suggestions.
- Duplicate processing should be minimized through idempotent processing logic.

## 6. Embedding Storage Strategy

The initial version will store embeddings in PostgreSQL.

For the first implementation, embedding values may be stored as arrays or JSON-based data, depending on the database implementation.

The system will use cosine similarity to rank image and blog post embeddings.

The initial dataset is small enough to perform similarity calculations without requiring a dedicated vector database.

## 7. Database Design Goals

The database is designed to support:

- Persistent image metadata.
- Blog content management.
- Semantic image matching.
- Match decision tracking.
- Human review.
- Background job management.
- AI cost monitoring.
- Performance through appropriate indexes.
- Data integrity through relational constraints.

