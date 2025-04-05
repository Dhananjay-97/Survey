# AI Survey Generator

**Generate intelligent surveys in minutes using AI.**

AI Survey Generator is a tool that automates the creation of survey questionnaires.  Simply provide your project details, business context, and research goals, and the application will use AI to design a relevant and comprehensive survey for you.

**Key Features:**

*   **AI-Powered Survey Creation:** Leverages OpenAI's GPT-3 and ChatGPT to generate survey questions and structures.
*   **Web Wizard Interface:** User-friendly web interface guides you through the survey creation process step-by-step.
*   **REST API Access:**  Programmatic API for integrating survey generation into other applications.
*   **Customizable and Configurable:**  Adjust settings like AI model choice, question types, and survey length.
*   **Output in DOCX Format:**  Download ready-to-use survey documents in DOCX format, ready for Google Docs or Microsoft Word.

**How to Use:**

1.  **Web UI:**  Run the `app.py` application and access the web wizard in your browser to create surveys interactively.
2.  **REST API:** Run the `flask_api.py` application and send API requests to programmatically generate surveys.

**Key Technologies:**

*   **Flask:** Python web framework for both the UI and API.
*   **OpenAI API:**  GPT-3 and ChatGPT models for AI-powered content generation.
*   **Python-docx:** Library for creating DOCX files.

**Setup (Setup ):**

1.  Install Python and required libraries (see detailed documentation for steps).
2.  Configure your OpenAI API key in `config.ini`.
3.  Run either `app.py` (for web UI) or `flask_api.py` (for API).

**Directory Structure ( 간단 Structure ):**

*   `app.py`, `flask_api.py`, `survey_generator.py`:  Core application code.
*   `config.ini`: Configuration settings.
*   `prompts/`: AI prompts for survey generation.
*   `templates/`, `static/`: Web UI files.

**Benefits:**

*   **Save Time:** Quickly create surveys without manual question writing.
*   **Improve Survey Quality:** AI assistance can help generate relevant and comprehensive questions aligned with your research objectives.
*   **Easy to Use:** User-friendly web interface and straightforward API access.
