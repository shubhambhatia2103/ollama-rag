# Medical Device RAG Assistant

A sophisticated AI-powered assistant for analyzing medical device logs using Retrieval-Augmented Generation (RAG). This system combines MongoDB vector search with Ollama LLMs to provide intelligent insights from device logs.

## 🌟 Features

- **Semantic Search**: Vector-based similarity search through device logs
- **Natural Language Queries**: Ask questions in plain English
- **Structured Data Retrieval**: SQL-like field queries with filters
- **Real-time Analysis**: Instant insights from large log datasets
- **Clean Architecture**: Modular, maintainable codebase
- **Error Handling**: Robust error handling and validation

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   CLI Interface │    │  Query Service  │    │ Database Service│
│                 │────│                 │────│                 │
│  • User Input   │    │  • Parse Query  │    │  • MongoDB Ops  │
│  • Command Proc │    │  • Extract Fields│    │  • Vector Search│
│  • Output Format│    │  • Build Filters│    │  • Field Queries│
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        │                        │
         │              ┌─────────────────┐                │
         │              │ Embedding Service│                │
         └──────────────│                 │────────────────┘
                        │  • Text Extract │
                        │  • Generate Emb │
                        │  • Ollama API   │
                        └─────────────────┘
                                 │
                        ┌─────────────────┐
                        │   LLM Service   │
                        │                 │
                        │  • Generate Resp│
                        │  • Context Build│
                        │  • Intent Analys│
                        └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- Node.js 16+
- MongoDB (local or Atlas)
- Ollama with models: `nomic-embed-text` and `mistral`

### Installation

1. Clone the repository
```bash
git clone <repository-url>
cd medical-device-rag-assistant
```

2. Install dependencies
```bash
npm install
```

3. Configure environment
```bash
cp .env.example .env
# Edit .env with your settings
```

4. Ingest logs (generate embeddings)
```bash
npm run ingest
```

5. Start the assistant
```bash
npm start
```

## 📋 Usage Examples

### Field Queries
```bash
# List specific fields
list devicename and ward where loglevel=error

# Show device IDs for specific model
show deviceid where model=plus

# Find devices in ICU
find devicename and state where ward=ICU
```

### Log Display
```bash
# Show all logs with filters
show logs where deviceid=DEV001

# Show error logs
show logs where loglevel=error and ward=ICU
```

### Natural Language Queries
```bash
# Ask questions
What devices are showing critical errors?
Which models have the most alerts?
Analyze patterns in ICU device failures
Show me devices with connectivity issues
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `MONGO_URI` | MongoDB connection string | `mongodb://localhost:27017` |
| `DB_NAME` | Database name | `logs` |
| `COLLECTION` | Collection name | `device_logs` |
| `OLLAMA_URL` | Ollama API URL | `http://localhost:11434` |
| `EMBEDDING_MODEL` | Embedding model | `nomic-embed-text` |
| `CHAT_MODEL` | Chat model | `mistral` |

### MongoDB Schema

The system expects log documents with this structure:

```json
{
  "DeviceId": "string",
  "OrganizationId": "string",
  "UserId": "string",
  "LogLevel": "string",
  "LogSummary": "string",
  "Timestamp": "string",
  "LogData": {
    "DeviceName": "string",
    "Model": "string",
    "State": "string",
    "Ward": "string",
    "Description": "string",
    "TagDetail": {
      "AlertCode": "string",
      "Message": "string",
      "AlertLevel": "number"
    }
  }
}
```

## 📚 API Reference

### Services

#### DatabaseService
- `connect()` - Establish database connection
- `queryFields(fields, filters)` - Query specific fields
- `vectorSearch(embedding, filters)` - Semantic search
- `findDocuments(filters, limit)` - Find documents

#### EmbeddingService
- `generateEmbedding(text)` - Generate text embedding
- `extractTextFromLog(doc)` - Extract searchable text
- `validateEmbedding(embedding)` - Validate embedding vector

#### LLMService
- `generateResponse(context, question)` - Generate AI response
- `analyzeQueryIntent(query)` - Analyze query intent
- `buildPrompt(context, question)` - Build LLM prompt

#### QueryService
- `processQuery(query)` - Process user query
- `extractFieldsAndFilters(query)` - Parse field queries
- `isShowLogsQuery(query)` - Check query type

## 🧪 Testing

```bash
# Run tests
npm test

# Run with coverage
npm run test:coverage
```

## 🔍 Troubleshooting

### Common Issues

1. **MongoDB Connection Failed**
   - Check MongoDB is running
   - Verify connection string in `.env`
   - Ensure database exists

2. **Ollama API Errors**
   - Verify Ollama is running: `ollama serve`
   - Check models are installed: `ollama list`
   - Test API: `curl http://localhost:11434/api/tags`

3. **No Embeddings Found**
   - Run ingestion script: `npm run ingest`
   - Check MongoDB vector index exists
   - Verify embedding model is working

### Debug Mode

Set `DEBUG=true` in `.env` for detailed logging.

## 🛠️ Development

### Code Structure

```
src/
├── config/          # Configuration management
├── services/        # Core business logic
├── utils/          # Utility functions
├── cli/            # Command line interface
└── app.js          # Application entry point
```

### Adding New Features

1. Create service in `src/services/`
2. Add utilities in `src/utils/`
3. Update CLI interface
4. Add tests
5. Update documentation

## 📈 Performance Optimization

- **Database Indexing**: Ensure proper indexes on query fields
- **Embedding Cache**: Consider caching embeddings
- **Batch Processing**: Process multiple queries in batches
- **Connection Pooling**: Use MongoDB connection pooling

## 🔐 Security

- Input validation and sanitization
- MongoDB injection prevention
- Rate limiting (future enhancement)
- Authentication (future enhancement)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## 📄 License

MIT License - see LICENSE file for details.

## 📞 Support

For issues and questions:
- Create an issue on GitHub
- Check troubleshooting guide
- Review documentation

---

Built with ❤️ for healthcare professional
