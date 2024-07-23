---
title: Anthropic Classifier
description: How to configure the Anthropic classifier
---

The Anthropic Classifier is an alternative classifier for the Multi-Agent Orchestrator that leverages Anthropic's AI models for intent classification. It provides powerful classification capabilities using Anthropic's state-of-the-art language models.

The Anthropic Classifier extends the abstract `Classifier` class and uses the Anthropic API client to process requests and classify user intents.

## Features

- Utilizes Anthropic's AI models (e.g., Claude) for intent classification
- Configurable model selection and inference parameters
- Supports custom system prompts and variables
- Handles conversation history for context-aware classification

### Basic Usage

To use the AnthropicClassifier, you need to create an instance with your Anthropic API key and pass it to the Multi-Agent Orchestrator:

```typescript
import { AnthropicClassifier } from "@aws/multi-agent-orchestrator";
import { MultiAgentOrchestrator } from "@aws/multi-agent-orchestrator";

const anthropicClassifier = new AnthropicClassifier({
  apiKey: 'your-anthropic-api-key'
});

const orchestrator = new MultiAgentOrchestrator({ classifier: anthropicClassifier });
```

### Custom Configuration

You can customize the AnthropicClassifier by providing additional options:

```typescript
const customAnthropicClassifier = new AnthropicClassifier({
  apiKey: 'your-anthropic-api-key',
  modelId: 'claude-3-sonnet-20240229',
  inferenceConfig: {
    maxTokens: 500,
    temperature: 0.7,
    topP: 0.9,
    stopSequences: ['']
  }
});

const orchestrator = new MultiAgentOrchestrator({ classifier: customAnthropicClassifier });
```

The AnthropicClassifier accepts the following configuration options:

- `apiKey` (required): Your Anthropic API key.
- `modelId` (optional): The ID of the Anthropic model to use. Defaults to Claude 3.5 Sonnet.
- `inferenceConfig` (optional): An object containing inference configuration parameters:
  - `maxTokens` (optional): The maximum number of tokens to generate. Defaults to 1000 if not specified.
  - `temperature` (optional): Controls randomness in output generation.
  - `topP` (optional): Controls diversity of output generation.
  - `stopSequences` (optional): An array of sequences that, when generated, will stop the generation process.

## Customizing the System Prompt

You can customize the system prompt used by the AnthropicClassifier:

```typescript
orchestrator.classifier.setSystemPrompt(
  `
  Custom prompt template with placeholders:
  {{AGENT_DESCRIPTIONS}}
  {{HISTORY}}
  {{CUSTOM_PLACEHOLDER}}
  `,
  {
    CUSTOM_PLACEHOLDER: "Value for custom placeholder"
  }
);
```

## Processing Requests

The AnthropicClassifier processes requests using the `processRequest` method, which is called internally by the orchestrator. This method:

1. Prepares the user's message.
2. Constructs a request for the Anthropic API, including the system prompt and tool configurations.
3. Sends the request to the Anthropic API and processes the response.
4. Returns a `ClassifierResult` containing the selected agent and confidence score.

## Error Handling

The AnthropicClassifier includes error handling to manage potential issues during the classification process. If an error occurs, it will log the error and return a default `ClassifierResult` with a null selected agent and zero confidence.

## Best Practices

1. **API Key Security**: Ensure your Anthropic API key is kept secure and not exposed in your codebase.
2. **Model Selection**: Choose an appropriate model based on your use case and performance requirements.
3. **Inference Configuration**: Experiment with different inference parameters to find the best balance between response quality and speed.
4. **System Prompt**: Craft a clear and comprehensive system prompt to guide the model's classification process effectively.

## Limitations

- Requires an active Anthropic API key.
- Classification quality depends on the chosen model and the quality of your system prompt and agent descriptions.
- API usage is subject to Anthropic's pricing and rate limits.

For more information on using and customizing the Multi-Agent Orchestrator, refer to the [Classifier Overview](/classifier/overview) and [Agents](/agents/overview) documentation.