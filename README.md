# LLM-Generated Disambiguation Dataset Evaluation

A comprehensive framework for evaluating the quality of LLM-generated disambiguation datasets using another LLM as a judge. This tool assesses how well a generator LLM creates facets and taxonomies for ambiguous questions.

## Overview

This evaluation system measures multiple quality dimensions of disambiguation datasets:

- **Taxonomy Accuracy**: Correctness of ambiguity type classification
- **Facet Coverage**: Completeness of disambiguation questions
- **Facet-Taxonomy Alignment**: Consistency between facets and assigned taxonomy
- **Ambiguity Detection**: Accuracy of ambiguity flagging
- **Disambiguation Quality**: Effectiveness of disambiguation questions
- **QA Alignment**: Consistency between questions and answers

## Taxonomy Categories

The evaluation uses a comprehensive taxonomy system with the following categories:

1. **Entity Reference**: Ambiguity due to multiple entities sharing the same name
2. **Underspecified Common Nouns**: Generic nouns that could refer to multiple specific instances
3. **Degree of an Action**: Questions about the extent or intensity of an action
4. **Event Specification**: Ambiguity about which specific event is being referenced
5. **Temporal Ambiguity**: Unclear time references
6. **Spatial Ambiguity**: Unclear location references
7. **Quantifier Scope**: Ambiguous scope of quantifiers like "all," "some," "most"
8. **Contextual Dependency**: Questions requiring additional context
9. **Multiple Valid Interpretations**: Questions with several legitimate meanings

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd <project-directory>

# Install required dependencies
pip install openai python-dotenv pandas numpy
```

## Configuration

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_api_key_here
```

## Usage

### Basic Evaluation

```python
from evaluation_script import evaluate_dataset

# Evaluate your dataset
results = evaluate_dataset(
    dataset_path="path/to/your/dataset.json",
    sample_size=100,  # Optional: evaluate subset
    output_path="evaluation_results.json"
)
```

### Full Dataset Evaluation

```python
# Evaluate the complete dataset
results = evaluate_dataset(
    dataset_path="data/full_dataset.json",
    output_path="results/full_evaluation.json"
)
```

### Custom Evaluation Parameters

```python
results = evaluate_dataset(
    dataset_path="data/dataset.json",
    model="gpt-4",  # Specify evaluation model
    temperature=0.1,  # Control randomness
    max_workers=5  # Parallel processing
)
```

## Output Format

The evaluation produces detailed JSON reports with:

```json
{
  "overall_metrics": {
    "taxonomy_agreement": 0.85,
    "facet_coverage_score": 0.78,
    "facet_taxonomy_alignment": 0.92,
    "ambiguity_flag_accuracy": 0.88,
    "avg_disambiguation_quality": 4.2,
    "qa_alignment_score": 0.91
  },
  "detailed_results": [...],
  "error_analysis": {...},
  "recommendations": [...]
}
```

## Key Metrics

### Taxonomy Agreement
Measures whether the LLM judge agrees with the generator's taxonomy classification.

### Facet Coverage Score
Assesses completeness of generated disambiguation questions for identified ambiguities.

### Facet-Taxonomy Alignment
Evaluates consistency between facets and their assigned taxonomy categories.

### Ambiguity Flag Accuracy
Checks if questions are correctly flagged as ambiguous or unambiguous.

### Disambiguation Quality Score
Rates the effectiveness of disambiguation questions (1-5 scale).

### QA Alignment Score
Measures consistency between questions, disambiguation facets, and answers.

## Common Issues

### Over-application of "Entity Reference"
The evaluation framework includes specific guidance to prevent over-classification of questions as "Entity Reference" ambiguity. Ensure your prompts include clear examples distinguishing entity ambiguity from other types.

### Missing Components
If your dataset has incomplete entries (missing facets, taxonomies, or QA pairs), the evaluation will flag these and provide detailed error reports.

## Project Structure

```
.
├── evaluation_script.py      # Main evaluation logic
├── prompts/
│   ├── taxonomy_eval.txt     # Taxonomy evaluation prompt
│   ├── facet_eval.txt        # Facet coverage evaluation prompt
│   └── alignment_eval.txt    # Alignment evaluation prompt
├── data/
│   └── dataset.json          # Input dataset
├── results/
│   └── evaluation_*.json     # Output evaluation results
└── README.md
```

## Best Practices

1. **Start Small**: Test on a sample before evaluating the full dataset
2. **Review Prompts**: Ensure taxonomy definitions are clear and comprehensive
3. **Iterative Refinement**: Use evaluation insights to improve generator prompts
4. **Error Analysis**: Pay attention to common failure patterns in the error reports
5. **Validate Results**: Manually review a subset of evaluations for quality assurance

## Troubleshooting

### Import Errors
Ensure all required packages are installed:
```bash
pip install -r requirements.txt
```

### API Rate Limits
Adjust the `max_workers` parameter to control parallel requests and avoid rate limiting.

### Memory Issues
For large datasets, process in batches using the `sample_size` parameter.

## Contributing

Contributions are welcome! Please submit pull requests with:
- Clear descriptions of changes
- Updated tests if applicable
- Documentation updates

## License

[Your License Here]

## Contact

[Your Contact Information]

## Acknowledgments

Built using OpenAI's API for LLM-as-judge evaluation methodology.
