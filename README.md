# End-to-End Test Automation Platform

Test automation platform with agentic architecture for generating XPath locators from HTML content.

## Features

- **XPath Generation**: Intelligent XPath locator generation using LangChain
- **HTML Analysis**: Comprehensive HTML structure analysis
- **Test Scenario Generation**: Automated test scenario creation
- **Test Execution**: Built-in test execution capabilities
- **Agentic Architecture**: Modular agent-based design
- **FastAPI Integration**: RESTful API with automatic documentation

## Technology Stack

- **Backend**: FastAPI + Python 3.8+
- **Framework**: LangChain + OpenAI GPT
- **HTML Processing**: BeautifulSoup4
- **Data Validation**: Pydantic
- **API Documentation**: Swagger UI / ReDoc

## Installation

### Prerequisites

- Python 3.8 or higher
- OpenAI API Key (optional - fallback mode available)

### Setup

1. **Clone the repository**
```bash
git clone <repository-url>
cd xpath-agent
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# or
venv\Scripts\activate  # Windows
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Environment configuration**
```bash
cp env_example.txt .env
# Edit .env and add your OpenAI API key
OPENAI_API_KEY=your_openai_api_key_here
```

## Usage

### Start the Server

```bash
python main.py
```

Server will be available at `http://localhost:8000`

### API Documentation

- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

### API Endpoints

#### Generate XPath Locators
```bash
POST /generate-xpath
Content-Type: application/json

{
    "html_content": "<html><body><button id='submit-btn'>Submit</button></body></html>",
    "target_description": "Submit button",
    "element_type": "button",
    "additional_context": "Form submission button"
}
```

#### Analyze HTML Structure
```bash
POST /analyze-html
Content-Type: application/json

{
    "html_content": "<html>...</html>"
}
```

#### Generate Test Scenarios
```bash
POST /generate-test-scenarios
Content-Type: application/json

{
    "html_content": "<html>...</html>",
    "target_description": "login form testing"
}
```

#### Execute Tests
```bash
POST /execute-tests
Content-Type: application/json

{
    "test_cases": [
        {
            "id": "tc_001",
            "name": "Login Test",
            "steps": ["Navigate to page", "Click login", "Verify result"]
        }
    ]
}
```

#### Health Check
```bash
GET /health
```

## Architecture

### Agentic Design

The platform uses a modular agent-based architecture:

- **XPath Generation Agent**: Core XPath locator generation
- **HTML Analysis Agent**: HTML structure analysis
- **Test Scenario Agent**: Test scenario generation
- **Test Execution Agent**: Test execution and reporting

### XPath Generation Strategy

The system prioritizes XPath generation in the following order:

1. **ID Attribute** (Highest priority)
   - `//button[@id='submit-btn']`

2. **Name Attribute**
   - `//input[@name='email']`

3. **Class Attribute**
   - `//button[@class='btn-primary']`

4. **Text Content** (Last resort)
   - `//button[text()='Login']`

5. **Combined Attributes**
   - `//input[@type='email' and @name='user-email']`

### Confidence Scoring

- **0.9-1.0**: ID-based XPaths (Most reliable)
- **0.7-0.9**: Name-based XPaths
- **0.5-0.7**: Class-based XPaths
- **0.3-0.5**: Text-based XPaths
- **0.0-0.3**: Position-based XPaths (Least reliable)

## Project Structure

```
xpath-agent/
├── agents/                 # Agent classes
│   ├── xpath_agent.py     # Main XPath generation agent
│   ├── tools.py           # Agent tools
│   └── prompts.py         # Prompt templates
├── models/                # Pydantic models
│   └── request_models.py  # Request/Response models
├── utils/                 # Utility functions
│   └── html_parser.py     # HTML parsing utilities
├── tests/                 # Test files
│   └── test_xpath_agent.py
├── examples/              # Usage examples
│   ├── example_usage.py   # Python SDK examples
│   └── api_examples.py    # API test examples
├── main.py               # FastAPI application
├── simple_main.py        # Simple version (no API key required)
├── requirements.txt      # Python dependencies
└── README.md            # This file
```

## Development

### Adding New Agents

```python
from langchain.tools import BaseTool

class CustomAgent(BaseTool):
    name = "custom_agent"
    description = "Agent description"
    
    def _run(self, input_data: str) -> str:
        # Agent logic
        return "Result"
```

### Adding New Endpoints

```python
@app.post("/custom-endpoint")
async def custom_endpoint(request: CustomRequest):
    # Endpoint logic
    return CustomResponse()
```

## Testing

### Run Tests

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_xpath_agent.py

# Run with coverage
pytest --cov=agents --cov=utils
```

### Test Examples

```bash
# Run example usage
python examples/example_usage.py

# Run API examples
python examples/api_examples.py
```

## Deployment

### Docker Deployment

```bash
# Build image
docker build -t xpath-agent .

# Run container
docker run -p 8000:8000 xpath-agent
```

### Production Deployment

```bash
# Install production server
pip install gunicorn

# Run with Gunicorn
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
```

## Configuration

### Environment Variables

- `OPENAI_API_KEY`: OpenAI API key for AI features
- `LOG_LEVEL`: Logging level (default: info)
- `HOST`: Server host (default: 0.0.0.0)
- `PORT`: Server port (default: 8000)

### API Key Configuration

The platform supports two modes:

1. **Advanced Mode**: Requires OpenAI API key for advanced features
2. **Simple Mode**: Works without API key using rule-based XPath generation

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License.

## Support

For questions and support:

- Create an issue in the repository
- Check the API documentation at `/docs`
- Review the examples in the `examples/` directory

## Changelog

### v1.0.0
- Initial release
- XPath generation with AI and rule-based fallback
- HTML analysis capabilities
- Test scenario generation
- Test execution framework
- FastAPI integration with automatic documentation
