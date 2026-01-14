# Email Summarizer AI

An intelligent email management system that automatically fetches unread emails, generates AI-powered summaries using **Ollama** (cloud LLM), and sends them to Slack with human approval.

## Features

- **Automated Email Fetching**: Connects to your email account and retrieves all unread messages
- **AI-Powered Summarization**: Uses Ollama with local LLM models (llama2, mistral, codellama) for privacy-focused, intelligent summaries
- **Human-in-the-Loop**: Review and approve/reject summaries before sending
- **Real-time Updates**: WebSocket-based live interface with instant status updates
- **Slack Integration**: Automatically sends approved summaries to your Slack workspace
- **Beautiful UI**: Modern, gradient-rich interface with smooth animations
- **Edit Capability**: Modify AI-generated summaries before approval
- **Privacy-First**: All AI processing happens locally with Ollama - no data sent to external APIs

## Project Structure

```
Email_Summarizer/
├── frontend/
│   ├── index.html          # Main UI interface
│   └── static/             # Static assets
├── graph/
│   ├── __init__.py
│   └── workflow.py         # LangGraph workflow definition
├── nodes/
│   ├── __init__.py
│   ├── fetch_emails.py     # Email fetching logic
│   ├── human_approval.py   # Approval workflow
│   ├── slack.py            # Slack integration
│   └── summarize.py        # AI summarization
├── state/
│   └── agent_state.py      # State management
├── app.py                  # FastAPI backend server
├── main.py                 # CLI workflow runner
├── credentials.json        # Email credentials (not in repo)
├── token.json              # OAuth token (auto-generated)
├── requirements.txt        # Python dependencies
└── .env                    # Environment variables
```

## Prerequisites

- Python 3.8+
- Gmail account with API access enabled
- Ollama installed locally with a compatible model (e.g., llama2, mistral, codellama)
- Slack workspace with incoming webhook

## Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd Email_Summarizer
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Set up Gmail API**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project
   - Enable Gmail API
   - Create OAuth 2.0 credentials
   - Download credentials and save as `credentials.json` in project root

## Usage

### Web Interface (Recommended)

1. **Start the server**

   ```bash
   python app.py
   ```

2. **Open your browser**
   Navigate to `http://localhost:8000`

3. **Workflow**
   - Click "📥 Fetch Emails" to retrieve unread emails
   - Review the fetched emails in the interface
   - Click "🤖 Generate Summary" to create AI summary
   - Review the summary and optionally edit it
   - Click "✅ Approve & Send" to send to Slack
   - Or click "❌ Reject" to discard the summary

### Command Line Interface

Run the workflow from the terminal:

```bash
python main.py
```

This will execute the workflow step-by-step with console output.

## Configuration

### Email Settings

The email fetching is configured in `nodes/fetch_emails.py`. By default, it:

- Fetches all unread emails
- Marks them as read after fetching
- Supports Gmail API authentication

### AI Summarization

The summarization uses **Ollama** with local LLM models (llama2, mistral, etc.) and is configured in `nodes/summarize.py`. You can customize:

- Model selection (set via `OLLAMA_MODEL` environment variable)
- Temperature and other parameters
- Prompt engineering for better summaries
- Model endpoint (default: `http://localhost:11434`)

### Slack Integration

Configured in `nodes/slack.py`. The webhook URL is set via environment variables.

## Workflow Architecture

The project uses **LangGraph** for workflow orchestration:

1. **Fetch Emails**: Retrieves unread emails from Gmail
2. **Generate Summary**: AI creates a concise summary
3. **Human Approval**: User reviews and approves/rejects
4. **Send to Slack**: Approved summaries are posted to Slack

## API Endpoints

- `GET /`: Serves the web interface
- `GET /api/status`: Returns current workflow state
- `WebSocket /ws`: Real-time bidirectional communication

## WebSocket Events

**Client → Server:**

- `fetch_emails`: Trigger email fetching
- `generate_summary`: Generate AI summary
- `approve`: Approve and send summary
- `reject`: Reject current summary
- `reset`: Reset workflow state

**Server → Client:**

- `connected`: Initial connection established
- `emails_fetched`: Emails successfully retrieved
- `summary_generated`: Summary created
- `slack_sent`: Message sent to Slack
- `error`: Error occurred

## Technologies Used

- **Backend**: FastAPI, LangGraph, LangChain
- **AI**: Ollama (Local LLM - llama2, mistral, codellama, etc.)
- **Email**: Gmail API (OAuth 2.0)
- **Messaging**: Slack Webhooks
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **Real-time**: WebSockets
- **Styling**: Custom CSS with gradients and animations

## Security Notes

- Never commit `credentials.json`, `token.json`, or `.env` files
- Keep your API keys and webhook URLs secure
- Use environment variables for sensitive data
- The `token.json` is auto-generated on first OAuth flow
- **Privacy advantage**: With Ollama, all AI processing happens locally - your email content never leaves your machine

## Troubleshooting

**Gmail Authentication Issues:**

- Ensure `credentials.json` is properly configured
- Delete `token.json` and re-authenticate
- Check Gmail API is enabled in Google Cloud Console

**WebSocket Connection Failed:**

- Verify the server is running on port 8000
- Check for firewall or proxy issues
- Ensure browser supports WebSockets

**AI Summary Not Generating:**

- Verify Ollama is running (`ollama serve`)
- Check that your model is pulled (`ollama list`)
- Ensure `OLLAMA_MODEL` matches an available model
- Test Ollama directly: `ollama run llama2 "test prompt"`
- Check if Ollama endpoint is accessible (default: `http://localhost:11434`)
- Review console logs for detailed errors

**Slack Message Not Sending:**

- Confirm `SLACK_WEBHOOK_URL` is correct
- Test webhook manually with curl
- Check Slack workspace permissions

## Future Enhancements

- [ ] Multi-email provider support (Outlook, Yahoo)
- [ ] Email filtering and categorization
- [ ] Scheduled automatic summaries
- [ ] Summary templates
- [ ] Multi-language support
- [ ] Email priority detection
- [ ] Attachment handling

## License

MIT License - feel free to use and modify as needed.

## Author

**Khizar Ishtiaq**