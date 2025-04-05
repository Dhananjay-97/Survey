
## Files Content

*   **`README.md`**: This file, providing an overview and instructions for the application.
*   **`app.py`**:  The main Flask application file that sets up the web UI for the survey generator wizard. It handles routing for different steps of the wizard (project details, business overview, research objectives, and questionnaire generation), renders HTML templates, and interacts with the `SurveyGenerator` class.
*   **`config.ini`**:  Configuration file to manage settings like OpenAI API keys, model choices (GPT-3 or ChatGPT), logging options, prompt file paths, and inference parameters (e.g., minimum question counts).
*   **`flask_api.py`**: Defines a Flask REST API using Flask-RESTful. It exposes endpoints for programmatically generating business overviews, research objectives, and complete survey questionnaires. It includes asynchronous processing and status tracking for survey generation requests using an SQLite database.
*   **`survey_generator.py`**: Contains the core logic for survey generation. The `SurveyGenerator` class handles:
    *   Loading configuration and prompts.
    *   Interacting with OpenAI's GPT-3 and ChatGPT models via the `openai` library.
    *   Generating business overviews, research objectives, survey questions (multiple choice, open-ended, matrix, video), and question choices using prompts.
    *   Structuring the generated questionnaire in SurveyJS JSON format.
    *   Exporting the questionnaire to a DOCX file using a template (`template_new.docx`).
    *   Uploading the DOCX to `uguu.se` and providing a Google Docs viewer link.
    *   Updating metrics and logging.
*   **`prompts/`**: This directory contains JSON files holding prompts for different survey generation tasks. There are separate folders for prompts tailored to `prompts_gpt3/` and `prompts_chatgpt/`, allowing for optimization for each model type.
*   **`static/css/style.css`**:  A basic CSS file to style the web application's user interface, providing a cleaner look and feel to the wizard pages.
*   **`templates/`**:  Contains HTML templates for each page of the web-based survey generator wizard, defining the user interface elements and form structures for collecting project information, business overview, and research objectives.

## Setup and Installation

To run this application, you need to have Python installed, along with several Python libraries. Follow these steps to set up your environment:

**Prerequisites:**

*   **Python 3.7+**
*   **pip** (Python package installer - usually comes with Python installations)

**Installation Steps:**

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd dhananjay-97-survey
    ```
    *(Replace `<repository_url>` with the actual URL of your repository)*

2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Linux/macOS
    venv\Scripts\activate  # On Windows
    ```

3.  **Install required Python packages:**
    ```bash
    pip install -r requirements.txt
    ```
    *(It's good practice to create a `requirements.txt` file listing dependencies. If you don't have one, you'll need to install them manually):*
    ```bash
    pip install flask waitress flask-restful flask-cors openai python-docx requests pandas configparser
    ```

4.  **Configuration:**
    *   **Open `config.ini`**:  You need to configure your OpenAI API key and other settings in the `config.ini` file.
    *   **OpenAI API Key:**
        *   Go to the [OpenAI API website](https://platform.openai.com/) and create an account or log in.
        *   Navigate to your API keys and generate a new secret key.
        *   In `config.ini`, find the `[GPT3 MODEL]` section and replace `sk-proj-zsuuCe8pnWfGT3NSRs6Sikc2R2j0mtOB3Mzgd4BrCpkIJvrDtM2Nh3oPplQtYsqbpjv8kfFk98T3BlbkF` with your actual OpenAI API key in the `Key` field.
    *   **Review other settings:** You can review and adjust other settings in `config.ini` as needed, such as model choices, temperature, logging levels, and prompt usage.

5.  **Run the application:**
    You can run the application in two ways:

    *   **For Web UI (development server):**
        ```bash
        python app.py
        ```
        This will start the Flask development server. Open your browser and go to `http://127.0.0.1:5000/` to access the survey wizard.

    *   **For REST API (development server):**
        ```bash
        python flask_api.py
        ```
        This will start the Flask development server for the API. The API will be available at `http://0.0.0.0:8080/`.

    *   **For Production (using Waitress - for API or Web UI):**
        *(Install Waitress if you haven't already: `pip install waitress`)*
        To use Waitress (a production-ready WSGI server), modify the `if __name__ == '__main__':` block in `app.py` or `flask_api.py` to uncomment the `serve(app)` line and comment out `app.run()`. Then run:
        ```bash
        python app.py  # Or python flask_api.py, depending on which app you modified
        ```
        Waitress will serve the application (by default on port 8080 for `flask_api.py`).

## Usage

**Web UI (using `app.py`):**

1.  **Start the Flask web application:** Run `python app.py`.
2.  **Open the survey wizard:** Access `http://127.0.0.1:5000/` in your web browser.
3.  **Step-by-step wizard:**
    *   **Page 1: Project Details:** Enter the project name, company name, industry, and use case for your survey. Click "Next".
    *   **Page 2: Business Overview:** Review the AI-generated business overview (or modify it). Click "Next".
    *   **Page 3: Research Objectives:**  Review the provided guidance and template for writing research objectives. Enter your research objectives in the text area. Click "Next".
    *   **Page 4: Survey Ready!:**  Your survey questionnaire is generated! Click the "Open with Google Docs" button to view and download the survey in DOCX format, opened in Google Docs Viewer.

**REST API (using `flask_api.py`):**

1.  **Start the Flask REST API application:** Run `python flask_api.py`.
2.  **Send POST requests to API endpoints:** You can use tools like `curl`, `Postman`, or Python's `requests` library to interact with the API.

    *   **`/BusinessOverview` (POST):**
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"company_name": "Your Company Name", "request_id": "unique_request_id", "project_name": "Your Project Name", "industry": "Your Industry", "use_case": "Your Use Case"}' http://0.0.0.0:8080/BusinessOverview
        ```
        *(Replace placeholders with your values)*
        Response will be JSON containing the generated business overview.

    *   **`/ResearchObjectives` (POST):**
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"company_name": "Your Company Name", "business_overview": "Your Business Overview Text", "industry": "Your Industry", "use_case": "Your Use Case", "request_id": "unique_request_id", "project_name": "Your Project Name"}' http://0.0.0.0:8080/ResearchObjectives
        ```
        *(Replace placeholders with your values)*
        Response will be JSON containing the generated research objectives.

    *   **`/Business_ResearchObjAPI` (POST):** (Combines both Business Overview and Research Objectives in one call)
         ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"company_name": "Your Company Name", "industry": "Your Industry", "use_case": "Your Use Case", "request_id": "unique_request_id", "project_name": "Your Project Name"}' http://0.0.0.0:8080/Business_ResearchObjAPI
        ```
        *(Replace placeholders with your values)*
        Response will be JSON containing both business overview and research objectives.

    *   **`/Questionnaire` (POST):** (Generates the complete survey questionnaire)
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{"company_name": "Your Company Name", "business_overview": "Your Business Overview Text", "research_objectives": "Your Research Objectives Text", "industry": "Your Industry", "use_case": "Your Use Case", "request_id": "unique_request_id", "project_name": "Your Project Name"}' http://0.0.0.0:8080/Questionnaire
        ```
        *(Replace placeholders with your values)*
        Response will be JSON containing the SurveyJS questionnaire JSON, DOCX download link, and status information.

        **Asynchronous Processing for `/Questionnaire`:**
        The `/Questionnaire` endpoint uses asynchronous processing.
        *   **Initial Request:** The first request with a new `request_id` will return a JSON response with `"status": "STARTED"` or `"status": "RUNNING"` and `"success": 2`, indicating the survey generation is in progress. `pages` and `doc_link` will be empty.
        *   **Subsequent Requests (same `request_id`):** If you send the same request again (with the same `request_id`) while the generation is still running, you will continue to get a `"status": "RUNNING"` response.
        *   **Completion:** Once survey generation is complete, subsequent requests with the same `request_id` will return `"status": "COMPLETED"`, `"success": 1`, and the JSON response will include the generated `"pages"` (SurveyJS questionnaire JSON) and `"doc_link"` (Google Docs viewer link).

## Configuration (`config.ini`)

The `config.ini` file allows you to customize the application's behavior. Key sections include:

*   **`[LOGGING]`**:
    *   `Logging`: Enable or disable logging ( `1` for enable, `0` for disable).
    *   `LoggingLevel`: Set the logging level (e.g., `Info`, `Debug`).
    *   `Overwrite`:  Overwrite the log file on each run (`1`) or append (`0`).
    *   `UseChatGPT_...`: Flags to enable/disable using ChatGPT prompts for specific tasks (e.g., `UseChatGPT_business_overview = 1` to use ChatGPT prompts for business overview generation).

*   **`[GPT3 MODEL]`**:
    *   `Key`: Your OpenAI API key.
    *   `GPT3Model`:  The OpenAI model to use for GPT-3 completion tasks (e.g., `gpt-4o-mini`).
    *   `ChatGPTModel`: The OpenAI model to use for ChatGPT chat completion tasks (e.g., `gpt-4o-mini`).
    *   `GPT3TemperatureDefault`, `GPT3TopP`, `GPT3FrequencyPenalty`, `GPT3PresencePenalty`: Default parameters for GPT-3 API calls.
    *   `...MaxToken`, `...Temperature`: Max token limits and temperature settings for specific survey generation tasks (e.g., `BusinessOverviewMaxToken`, `BusinessOverviewTemperature`).

*   **`[GPT3 PROMPTS]`**:
    *   External URLs (using `textdoc.co`) for default GPT-3 prompts. These are fallback prompts if you don't want to use the JSON prompt files in the `prompts_gpt3/` directory.

*   **`[OPENAI PROMPTS]`**:
    *   File paths to JSON prompt files located in the `prompts/` directory. These prompts are used for both GPT-3 and ChatGPT, depending on the `UseChatGPT_...` flags in the `[LOGGING]` section.

*   **`[INFERENCE]`**:
    *   `MetricsFilename`: Filename for the CSV file to store survey generation metrics (`survey_generator_metrics.csv`).
    *   `MinMatrixQuestions`, `MinMatrixOEQuestions`: Minimum number of matrix and open-ended matrix questions to include in the generated survey.

## Logs

Application logs are saved in the `logs/` directory. Log filenames are timestamped with the current date and time when the application starts (e.g., `logs/04_05_2025_13_32_55_survey_generator.log`).  Examine these log files for debugging information, errors, and details about the survey generation process.

## Contributing

[Optional: Add information about how others can contribute to the project, e.g., reporting issues, submitting pull requests, etc.]

## License

[Optional: Specify the license under which the project is distributed, e.g., MIT License, Apache 2.0, etc.]

---

This README provides a comprehensive overview of the Knit AI Survey Generator. For any questions or issues, please refer to the documentation or contact the project maintainers.
