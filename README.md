# fireship-ext README

This is the README for your extension "fireship-ext". After writing up a brief description, we recommend including the following sections.

## Features

Describe specific features of your extension including screenshots of your extension in action. Image paths are relative to this README file.

For example if there is an image subfolder under your extension project workspace:

\!\[feature X\]\(images/feature-x.png\)

> Tip: Many popular extensions utilize animations. This is an excellent way to show off your extension! We recommend short, focused animations that are easy to follow.

## Requirements

If you have any requirements or dependencies, add a section describing those and how to install and configure them.

## Extension Settings

Include if your extension adds any VS Code settings through the `contributes.configuration` extension point.

For example:

This extension contributes the following settings:

* `myExtension.enable`: Enable/disable this extension.
* `myExtension.thing`: Set to `blah` to do something.

## Known Issues

Calling out known issues can help limit users opening duplicate issues against your extension.

## Release Notes

Users appreciate release notes as you update your extension.

### 1.0.0

Initial release of ...

### 1.0.1

Fixed issue #.

### 1.1.0

Added features X, Y, and Z.

---

## Following extension guidelines

Ensure that you've read through the extensions guidelines and follow the best practices for creating your extension.

* [Extension Guidelines](https://code.visualstudio.com/api/references/extension-guidelines)

## Working with Markdown

You can author your README using Visual Studio Code. Here are some useful editor keyboard shortcuts:

* Split the editor (`Cmd+\` on macOS or `Ctrl+\` on Windows and Linux).
* Toggle preview (`Shift+Cmd+V` on macOS or `Shift+Ctrl+V` on Windows and Linux).
* Press `Ctrl+Space` (Windows, Linux, macOS) to see a list of Markdown snippets.

## For more information

* [Visual Studio Code's Markdown Support](http://code.visualstudio.com/docs/languages/markdown)
* [Markdown Syntax Reference](https://help.github.com/articles/markdown-basics/)

**Enjoy!**

## Here's how to run a larger 7B model in your VS Code extension with Ollama:

First, verify the model exists in Ollama:

bash
Copy
ollama list
If you don't see your 7B model, you'll need to:

Pull the 7B model (example for DeepSeek 7B):

bash
Copy
ollama pull deepseek-7b
Modify your extension code:

typescript
Copy
// Change the model name in your chat request
const streamResponse = await ollama.chat({
  model: 'deepseek-7b', // Use the exact name from Ollama
  messages: [{ role: 'user', content: userPrompt }],
  stream: true
});
Important considerations for larger models:

System Requirements:

Minimum 16GB RAM (recommended 32GB)

At least 8GB VRAM for GPU acceleration

SSD storage recommended

Performance Optimization:

typescript
Copy
// Add these options to your chat request
const streamResponse = await ollama.chat({
  model: 'deepseek-7b',
  messages: [{ role: 'user', content: userPrompt }],
  stream: true,
  options: {
    num_gpu: 1, // Use GPU if available
    num_thread: 8, // Adjust based on your CPU cores
    temperature: 0.7 // Control creativity vs determinism
  }
});
Verify Model Compatibility:

Check the model's required context window

Some 7B models need additional parameters:

typescript
Copy
options: {
  num_ctx: 4096, // Context window size
  stop: ['\n', '###'] // Model-specific stop sequences
}
Handle Longer Response Times:

typescript
Copy
// Add a loading indicator
panel.webview.postMessage({ command: 'loading', state: true });

try {
  // ... existing code ...
} finally {
  panel.webview.postMessage({ command: 'loading', state: false });
}
Update your webview HTML:

html
Copy
<div id="loading" style="display: none;">Generating...</div>

<script>
window.addEventListener('message', event => {
  if (event.data.command === 'loading') {
    document.getElementById('loading').style.display = 
      event.data.state ? 'block' : 'none';
  }
});
</script>
Run HTML
Troubleshooting Tips:

If you get "model not found" errors:

bash
Copy
# List all available models
ollama list

# Search for models
ollama search deepseek
For better performance:

bash
Copy
# Run Ollama with GPU acceleration
OLLAMA_NUM_GPU=1 ollama serve

# Allocate more system resources
OLLAMA_NUM_PARALLEL=4 OLLAMA_MAX_LOADED_MODELS=2 ollama serve
If responses are slow:

Reduce the context window size (num_ctx)

Use quantization (look for models with -q4 or -q5 suffixes)

Try a smaller prompt template

Example of a production-ready configuration:

typescript
Copy
const streamResponse = await ollama.chat({
  model: 'deepseek-7b:q5_K_M',
  messages: [{
    role: 'system',
    content: 'You are a helpful coding assistant. Keep responses under 500 characters.'
  }, {
    role: 'user', 
    content: userPrompt
  }],
  stream: true,
  options: {
    num_ctx: 2048,
    num_gpu: 1,
    temperature: 0.5,
    repeat_penalty: 1.2
  }
});
Remember to:

Update your package.json requirements if needed

Add error handling for model-specific limitations

Test with different prompt formats (some 7B models require specific templates)
