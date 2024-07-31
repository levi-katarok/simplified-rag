# RAG Application with PostgreSQL and PGVector

This project demonstrates a basic Retrieval Augmented Generation (RAG) application using PostgreSQL with PGVector for efficient similarity search of text embeddings.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Features

- Extract text from PDF documents
- Generate text embeddings using OpenAI's API
- Store embeddings in PostgreSQL using PGVector
- Query for similar text embeddings based on cosine distance

## Requirements

- Python 3.6+
- PostgreSQL with PGVector extension installed
- OpenAI API key
- Libraries: numpy, pandas, sqlalchemy, pgvector, openai, pypdf

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/rag-postgres-pgvector.git
   cd rag-postgres-pgvector
   ```

2. Create a virtual environment and activate it:
   ```
   python3 -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

4. Set up your PostgreSQL database and ensure the PGVector extension is installed. Note you need to do this within a sql shell.:
   ```sql
   CREATE EXTENSION IF NOT EXISTS vector;
   ```

5. Update the database connection string in `main.py`:
   ```python
   engine = create_engine('postgresql://user:password@localhost/dbname')
   ```

6. Set your OpenAI API key in `main.py`:
   ```python
   openai.api_key = 'your_openai_api_key'
   ```

## Usage

1. Place your PDF file in the project directory.

2. Update the file path in `main.py`:
   ```python
   pdf_text = read_pdf("path_to_your_pdf.pdf")
   ```

3. Run the script:
   ```
   python main.py
   ```

4. The script will perform the following actions:
   - Extract text from the PDF
   - Split the text into manageable chunks
   - Generate embeddings for each chunk
   - Store the embeddings in the PostgreSQL database

5. To query for similar embeddings, uncomment and modify the example query section in `main.py`:
   ```python
   query_embedding = generate_embeddings(["Your query text"])[0]
   similar_embeddings = find_similar_embeddings(query_embedding)
   # Process and use similar_embeddings as needed
   ```

## Project Structure

- `main.py`: Contains the main application code
- `requirements.txt`: Lists all Python dependencies
- `README.md`: Project documentation (you are here)
- `LICENSE`: MIT License file

## Configuration

### Database Configuration

Update the database connection string in `main.py`:

```python
engine = create_engine('postgresql://user:password@localhost/dbname')
```

Replace `user`, `password`, `localhost`, and `dbname` with your PostgreSQL credentials and database information.

### OpenAI API Configuration

Set your OpenAI API key in `main.py`:

```python
openai.api_key = 'your_openai_api_key'
```

Replace `'your_openai_api_key'` with your actual OpenAI API key.

### Embedding Model Configuration

The project uses OpenAI's "text-embedding-ada-002" model by default. If you want to use a different model, update the `generate_embeddings` function in `main.py`:

```python
response = openai.Embedding.create(
    input=chunk,
    model="your-chosen-model"
)
```

Also, ensure that the `N_DIM` constant matches the output dimension of your chosen model:

```python
N_DIM = 1536  # Adjust this value based on your model's output dimension
```

## Troubleshooting

- **PG Vector Installation Issues**: Ensure you have the necessary permissions on your PostgreSQL instance. For cloud-hosted databases, check the provider's documentation for enabling extensions.

- **Python Dependency Conflicts**: Use a virtual environment as recommended. If issues persist, try updating packages to their latest versions.

- **API Key Security**: Always use environment variables or secure vaults to store sensitive information like API keys.

- **Database Connection Errors**: Verify your database connection string, credentials, and ensure there are no firewall restrictions.

- **Model Dimensionality Mismatch**: Double-check the `N_DIM` parameter against your embedding model's output dimensionality to avoid data truncation or insertion failures.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License