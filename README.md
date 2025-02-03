Credits to Fireship https://www.youtube.com/watch?v=clJCDHml2cA

# 🔥 BhAi: DeepSeek R1 VS Code Extension  

A VS Code extension powered by **DeepSeek R1** via **Ollama**, enabling seamless AI-assisted coding inside the editor.  

## 🚀 Features  
- Chat with **DeepSeek R1** directly in VS Code  
- Streaming responses for real-time interaction  
- Simple UI with prompt input and response display  

## 🛠 Installation  
1. Install **Node.js** and **Ollama**  
2. Pull the DeepSeek model:  
   ```bash
   ollama pull deepseek-r1
   ```  
3. Clone this repo and install dependencies:  
   ```bash
   npm install
   ```  
4. Compile and run:  
   ```bash
   npm run compile && npm run watch
   ```  

## ⚙️ Usage  
- Run the **"Chat with DeepSeek"** command from the VS Code command palette.  
- Type your query and get responses instantly.  

## 🔧 Running DeepSeek 7B in VS Code  
### 1️⃣ Verify Model Installation  
```bash
ollama list
```
If `deepseek-7b` is missing, install it:  
```bash
ollama pull deepseek-7b
```  

### 2️⃣ Modify Your Extension Code  
Update `extension.ts` to use the 7B model:  
```typescript
const streamResponse = await ollama.chat({
  model: 'deepseek-7b', // Ensure correct model name
  messages: [{ role: 'user', content: userPrompt }],
  stream: true
});
```  

### 3️⃣ System Requirements  
- **16GB+ RAM** (32GB recommended)  
- **8GB+ VRAM** for GPU acceleration  
- **SSD storage** for better performance  

### 4️⃣ Optimizations  
For better speed and efficiency:  
```typescript
const streamResponse = await ollama.chat({
  model: 'deepseek-7b',
  messages: [{ role: 'user', content: userPrompt }],
  stream: true,
  options: {
    num_gpu: 1, // Use GPU if available
    num_thread: 8, // Optimize for CPU cores
    temperature: 0.7 // Control randomness
  }
});
```  

## 🐞 Troubleshooting  
- **Model not found?** Run `ollama list` to verify installation.  
- **Slow responses?** Reduce `num_ctx` or use a quantized model (`q4_K_M`).  

## 📌 Roadmap  
- Add context window customization  
- Implement multi-turn conversation history  

---

🔗 **Follow [VS Code Extension Guidelines](https://code.visualstudio.com/api/references/extension-guidelines) for best practices.**  
