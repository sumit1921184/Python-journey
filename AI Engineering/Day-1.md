
# Ollama Setup Guide & Model Management

## 1. Installation of Ollama

### Linux Installation

**Recommended One-Command Method:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**Manual Installation:**
1. Download the latest version from [ollama.com/download](https://ollama.com/download)
2. Make executable and move to system path:
   ```bash
   chmod +x ollama-linux-amd64
   sudo mv ollama-linux-amd64 /usr/local/bin/ollama
   ```

**Verify Installation:**
```bash
ollama --version
```

**Start Ollama Service:**
```bash
ollama serve
```

---

### Windows Installation

1. Download the installer from [ollama.com/download](https://ollama.com/download)
2. Run the `.exe` file
3. Ollama will start automatically after installation

---

### macOS Installation

1. Download from [ollama.com/download](https://ollama.com/download)
2. Open the `.dmg` file
3. Drag Ollama to the Applications folder
4. Launch Ollama

---

## 2. Where Ollama Stores Models

Ollama stores all models in the following default locations:

| Operating System | Default Models Path                          |
|------------------|----------------------------------------------|
| **Linux**        | `~/.ollama/models/`                          |
| **macOS**        | `~/.ollama/models/`                          |
| **Windows**      | `%AppData%\Ollama\models\` or `C:\Users\YourName\AppData\Roaming\Ollama\models\` |

### Folder Structure
- `blobs/` → Contains the actual model weights
- `manifests/` → Model configuration and metadata
- `registry.ollama.ai/` → Registry information

---

## 3. Changing Model Storage Location

### Linux / macOS
```bash
# Set custom location
export OLLAMA_MODELS=/home/yourname/ollama_models

# Make it permanent (add to ~/.bashrc or ~/.zshrc)
echo 'export OLLAMA_MODELS=/path/to/your/models' >> ~/.bashrc
```

### Windows (Examples)

**Method 1: Using Environment Variables (Recommended)**

1. Right-click on **This PC** → **Properties** → **Advanced system settings**
2. Click **Environment Variables**
3. Under **User variables**, click **New**
4. Set:
   - Variable name: `OLLAMA_MODELS`
   - Variable value: `D:\Ollama\Models` (example path)
5. Click OK and restart Ollama

**Method 2: Using Command Prompt (Temporary)**
```cmd
set OLLAMA_MODELS=D:\Ollama\Models
ollama serve
```

**Method 3: Using PowerShell (Temporary)**
```powershell
$env:OLLAMA_MODELS = "D:\Ollama\Models"
ollama serve
```

---

## 4. Basic Model Management Commands

```bash
# List all installed models
ollama list

# Download a model
ollama pull llama3.2:3b

# Remove a model
ollama rm llama3.2:3b

# Show model details
ollama show llama3.2:3b

# Check storage usage
# Linux/macOS:
du -sh ~/.ollama/models/

# Windows (PowerShell):
Get-ChildItem -Path $env:OLLAMA_MODELS -Recurse | Measure-Object -Property Length -Sum
```

---

## 5. How to Download and Remove Models

**Download a Model:**
```bash
ollama pull model-name
```

**Examples:**
```bash
# Download small Llama model
ollama pull llama3.2:3b

# Download quantized version
ollama pull llama3.2:3b-q5_K_M
```

**Remove a Model (to free space):**
```bash
ollama rm llama3.2:3b
```

**List all your models:**
```bash
ollama list
```

---

## 6. Understanding Model Versions & Tags

Models come in different **sizes** and **versions**. You select them using a **tag** after the colon (`:`).

**Simple Examples:**

| Command                        | Meaning                                      | Brain Size |
|--------------------------------|----------------------------------------------|------------|
| `llama3.2`                     | Latest default version                       | Medium     |
| `llama3.2:1b`                  | Very small 1 Billion version                 | Tiny       |
| `llama3.2:3b`                  | Popular 3 Billion version                    | Small      |
| `qwen2.5:7b`                   | 7 Billion parameters                         | Medium     |
| `llama3.1:8b`                  | Llama 3.1 with 8 Billion                     | Medium     |

**Tip for Beginners**: Start with 1B or 3B models. They are easier to run.

---

## 7. Quantized Models Explained (Q2, Q4, Q5, Q6, Q8)

**Quantization = Making the model lighter and smaller.**

Think of it like compressing a photo:
- Original photo = high quality, big file
- Compressed photo = good enough quality, much smaller file

**Common Levels:**

| Level     | Size     | Quality     | Speed     | Best For                     |
|-----------|----------|-------------|-----------|------------------------------|
| Q2        | Smallest | Lower       | Fastest   | Very old computers           |
| Q4        | Small    | Good        | Fast      | Most normal laptops          |
| **Q5**    | Medium   | **Very Good** | Good    | **Best for beginners**       |
| Q6        | Larger   | Excellent   | Good      | Good computers               |
| Q8        | Largest  | Almost Original | Slower | Powerful PCs                 |

**Recommended**: Use **Q5_K_M** for most models when starting.

**Example:**
```bash
ollama pull llama3.2:3b-q5_K_M
```

---

## 8. CPU vs GPU Execution

**CPU** = Normal processor in your computer (like the main brain)  
**GPU** = Graphics card (very good at doing many calculations at once)

| Feature       | CPU                          | GPU (NVIDIA preferred)         |
|---------------|------------------------------|--------------------------------|
| Speed         | Slower                       | Much faster                    |
| RAM Usage     | Uses normal computer RAM     | Uses GPU memory (VRAM)         |
| Compatibility | Works on almost all computers| Needs good graphics card       |
| Best For      | Beginners, simple use        | Heavy usage, large models      |

**How to use GPU?**
- Ollama automatically uses GPU if you have NVIDIA graphics card with CUDA support.
- No extra command needed in most cases.

---

## 9. Context Window and Token Limits

**Context Window** = How much information the model can "remember" at one time.

- Think of it as short-term memory.
- Bigger context = Can read longer conversations or documents.

**Common Examples:**
- 4K context → Can remember about 3000-4000 words
- 8K context → About 6000 words
- 128K context → Very long (good for big documents)

**Token** = Small piece of text (roughly 3/4 of a word).

**Tip**: If the model forgets old messages, it has reached its context limit.

---

## 10. How Model Parameters Affect RAM Usage

**Rough Guidelines (for Q5 models):**

| Model Size | Approximate RAM Needed |
|------------|------------------------|
| 1B         | 1 - 2 GB               |
| 3B         | 2 - 4 GB               |
| 7B         | 5 - 8 GB               |
| 8B         | 6 - 10 GB              |
| 9B         | 7 - 12 GB              |

**Important Rule**:
- You need **more RAM than the model size**.
- Always leave some RAM free for your computer to run smoothly.

---

**Simple Explanation:**

**Parameters** are like the **brain connections** of the AI model.  
The more parameters a model has, the smarter it can be — but it also needs **more RAM** (memory) to run.

Think of it like this:
- Small brain (few parameters) → Needs less space
- Big brain (many parameters) → Needs much more space

---

#### RAM Usage Guidelines (For Beginners)

Here is a simple table for **quantized models (Q4 or Q5)**:

| Model Size | Number of Parameters | Approximate RAM Needed | Difficulty to Run |
|------------|----------------------|------------------------|-------------------|
| 1B         | 1 Billion            | 1 - 2 GB               | Very Easy         |
| 3B         | 3 Billion            | 2 - 4 GB               | Easy              |
| 7B         | 7 Billion            | 5 - 8 GB               | Medium            |
| 8B         | 8 Billion            | 6 - 10 GB              | Medium            |
| 9B         | 9 Billion            | 7 - 12 GB              | Medium-Hard       |

---

#### Important Rules to Remember

1. **You need more RAM than the model size**  
   Example: A 3B model doesn’t need only 3GB — it usually needs **2 to 4 GB**.

2. **Always leave free RAM**  
   Your computer also needs memory to run Windows/Mac/Linux + other programs.  
   → Keep at least **2-4 GB free**.

3. **Quantization helps a lot**  
   - Q5 version → Uses less RAM than the original model.
   - Q4 version → Uses even less RAM.

4. **Higher quantization = More RAM**  
   Example for 3B model:
   - Q4 → ~2 GB
   - Q5 → ~2.5 GB
   - Q8 → ~3.5 GB

---

#### Practical Tips for Beginners

- If you have **8 GB RAM** laptop → Safely use up to **3B models**
- If you have **16 GB RAM** → You can comfortably use **7B or 8B models**
- Start small (1B or 3B) → Then slowly try bigger models
- Always check your available RAM before downloading a big model

---

**Quick Command to Check RAM Usage (after running a model):**
- On Linux/macOS: `htop`
- On Windows: Open Task Manager (Ctrl + Shift + Esc)

---



**✅ Here is the dedicated, clean, and detailed documentation for Sections 2 and 3 only.**

---

# Section 2 & 3: Exploring Different Models & Comparison

## 2. Explore Different Models

In this section, we will install and test various models using Ollama. We have divided them into three categories:

### General Purpose Models
- Llama 3.2 (1B)
- Llama 3.2 (3B)
- Llama 3.1 (8B)
- Qwen 2.5 (3B)
- Qwen 2.5 (7B)
- Gemma 2 (2B)
- Gemma 2 (9B)
- Phi-3 Mini (3.8B)
- Mistral 7B

### Coding Models
- DeepSeek Coder
- Qwen Coder
- CodeGemma
- StarCoder2

### Vision Models (Image + Text)
- LLaVA
- Gemma Vision

**Installation Command Example:**
```bash
ollama pull llama3.2:3b
```

**Testing Tip**: After installing, run the model and test it with:
- General questions
- Reasoning puzzles
- Coding tasks
- Creative writing

---

## 3. Model Comparison

Below is a **detailed comparison table** for all the models listed. This table is created based on typical real-world performance (Q5 quantized versions) on mid-range hardware.

### Comprehensive Model Comparison Table

| Model                    | Size     | RAM Usage (Q5) | Response Speed | Coding Capability | Reasoning Capability | Hallucination Frequency | Best Use Case                        | Main Weaknesses                     |
|--------------------------|----------|----------------|----------------|-------------------|----------------------|-------------------------|--------------------------------------|-------------------------------------|
| **Llama 3.2 1B**        | 1B      | 1-2 GB        | Very Fast      | Basic             | Basic                | Medium                  | Quick chat, mobile, learning         | Limited knowledge, simple answers   |
| **Llama 3.2 3B**        | 3B      | 2-4 GB        | Fast           | Good              | Good                 | Low-Medium              | Daily tasks, students                | Struggles with complex topics       |
| **Llama 3.1 8B**        | 8B      | 6-10 GB       | Medium         | Very Good         | Very Good            | Low                     | General use, writing, research       | Needs good hardware                 |
| **Qwen 2.5 3B**         | 3B      | 2-4 GB        | Fast           | Good              | Good                 | Low                     | Balanced daily assistant             | Slightly weaker in creative tasks   |
| **Qwen 2.5 7B**         | 7B      | 5-8 GB        | Medium-Fast    | Excellent         | Excellent            | Very Low                | Strong all-rounder                   | Slightly slower than 3B             |
| **Gemma 2 2B**          | 2B      | 2-3 GB        | Very Fast      | Good              | Good                 | Medium                  | Fast responses, light tasks          | Less knowledge than Llama           |
| **Gemma 2 9B**          | 9B      | 7-12 GB       | Medium         | Very Good         | Very Good            | Low                     | Advanced reasoning                   | Higher RAM usage                    |
| **Phi-3 Mini**          | 3.8B    | 3-5 GB        | Fast           | Very Good         | Excellent            | Low                     | Reasoning, logical tasks             | Smaller knowledge base              |
| **Mistral 7B**          | 7B      | 5-8 GB        | Medium         | Very Good         | Very Good            | Low                     | Creative writing, chat               | Older model                         |
| **DeepSeek Coder**      | 6.7B    | 5-8 GB        | Medium         | **Outstanding**   | Good                 | Low                     | Programming, code generation         | Weaker in general chat              |
| **Qwen Coder**          | 7B      | 5-8 GB        | Medium         | **Excellent**     | Good                 | Low                     | Coding tasks                         | Less strong in non-coding           |
| **CodeGemma**           | 7B      | 5-8 GB        | Medium         | Excellent         | Good                 | Low                     | Code writing & explanation           | Limited general knowledge           |
| **StarCoder2**          | 7B-15B  | 6-12 GB       | Medium         | Very Good         | Medium               | Medium                  | Code completion                      | Weaker reasoning                    |
| **LLaVA**               | 7B-13B  | 6-12 GB       | Slow           | Basic             | Good                 | Medium                  | Image understanding + chat           | Slow, needs vision support          |
| **Gemma Vision**        | 9B+     | 8-14 GB       | Slow           | Good              | Good                 | Medium                  | Multimodal (text + image)            | High resource usage                 |

---

### Key Observations (Summary)

- **Best Overall General Models**: Qwen 2.5 7B and Llama 3.1 8B
- **Best for Beginners**: Llama 3.2 3B or Gemma 2 2B
- **Best Coding Models**: DeepSeek Coder and Qwen Coder
- **Best Small & Fast**: Llama 3.2 1B / 3B
- **Vision Models**: Significantly slower and need more resources

---



