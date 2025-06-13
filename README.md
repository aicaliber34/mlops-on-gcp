# ML Engineering on Google Cloud Platform

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)


This repository maintains hands-on labs and code samples that demonstrate best practices and patterns for implementing and operationalizing production grade machine learning workflows on Google Cloud Platform. 

## Navigating this repository
This repository is organized into two sections:
- [Mini workshops](./workshops/)
- [Code samples](./examples/)


### Mini workshops
This section contains hands-on labs for instructor led ML Engineering mini workshops. 

### Code Samples
This section compiles  samples demonstrating design and code patterns for a variety of ML Engineering topics. 



#### Sequence Diagram

```mermaid
sequenceDiagram
    participant User
    participant run_full_pipeline.sh
    participant main_pipeline (main_topic_pipeline.py / main_sentiment_pipeline.py / main_chunking_pipeline.py)
    participant utils
    participant azure_api_service
    participant schema
    participant retrieval (retrieval_thefuzz.py / retrieval_semantic_search.py)
    participant metrics
    participant evaluate_pipeline

    Note over User: User initiates the pipeline process

    User->>run_full_pipeline.sh: Start pipeline (bash Scripts/run_full_pipeline.sh)
    Note right of run_full_pipeline.sh: Shell script orchestrates the pipeline

    run_full_pipeline.sh->>main_pipeline: python main_*.py --client ... --configs ...
    Note right of main_pipeline: Entry point for topic, sentiment, or chunking

    main_pipeline->>utils: read_yaml_file(), read_excel_sheet(), etc.
    Note right of utils: Utilities for reading configs and data

    main_pipeline->>schema: get_schema(), load response schema
    Note right of schema: Data validation and schema loading

    main_pipeline->>azure_api_service: Initialize AzureOpenAIService
    Note right of azure_api_service: Handles LLM and Azure API calls

    main_pipeline->>retrieval: (optional) FuzzyMatcher/SemanticRetriever for examples
    Note right of retrieval: Retrieves example data for LLM prompts

    main_pipeline->>azure_api_service: call_*_in_parallel()
    Note right of azure_api_service: Makes parallel LLM calls

    azure_api_service->>schema: Validate LLM responses
    Note right of schema: Ensures LLM output matches schema

    azure_api_service->>utils: Logging, helpers
    Note right of utils: Logs results and errors

    main_pipeline->>metrics: calculate_metrics_pipeline()
    Note right of metrics: Calculates evaluation metrics

    main_pipeline->>evaluate_pipeline: (optional) Detailed evaluation/reporting
    Note right of evaluate_pipeline: Generates evaluation reports

    main_pipeline->>utils: Export results to Excel/CSV
    Note right of utils: Exports final results

    main_pipeline->>User: Output results and logs
    Note over User: User receives output files and logs
```
<br>



