Quickstart: Run Qwen 27B Free on Dual Tesla T4 GPUs
This setup lets you host Qwen 27B with a 36k context window on Kaggle for free (using their 30h/week GPU quota) and connect it directly to VS Code Copilot / Agent mode.


Step 1: 
  Upload the Notebook to Kaggle
  Go to kaggle.com and log in.
  Click + Create → New Notebook.
  In the menu bar: Click File → 
  Import Notebook → Upload the .ipynb file.
Step 2: 
Configure the Session Settings (Important!)
In the right-hand panel under Session options:
ACCELERATOR: Select GPU T4 x2 (Dual Tesla T4 is required!)
PERSISTENCE: Change from No persistence to Variables and Files
Why? This saves the 16.8 GB model and compiled binaries so you never have to re-download or re-compile on future runs.
ENVIRONMENT: Pin to original environment (or Latest).
INTERNET: Toggle ON.

Step 3: Run the First-Time Setup
Click Run All (or run cells in order):
Cell 1: Confirms 2 Tesla T4 GPUs are active.
Cell 2: Launches a free Cloudflare tunnel.
Cell 3 & 4: Compiles llama.cpp with CUDA support (takes ~2 min once, then saved forever).
Cell 5: Downloads the Qwen 27B GGUF model and starts the inference server.
When Cell 5 finishes, you will see:
FINAL_QWEN_STAGE_PASS
Health check: 200 OK
Look at the output of Cell 2 to find your Cloudflare Tunnel URL:
TUNNEL_READY https://xxxx-xxxx-xxxx.trycloudflare.com

Step 4: Connect to VS Code
In VS Code, press Ctrl + Shift + P and search for:
Preferences: Open User Settings (JSON)
In the same config folder (or create it), open/create:

chatLanguageModels.json
(Path: ~/.config/Code/User/chatLanguageModels.json on Linux, or %APPDATA%\Code\User\chatLanguageModels.json on Windows)
Paste the following configuration:
[
  {
    "name": "Kaggle Qwen 27B",
    "vendor": "customendpoint",
    "apiKey": "test-api-key-change-after-test",
    "apiType": "chat-completions",
    "models": [
      {
        "id": "qwen-27b",
        "name": "Qwen 27B (Dual Tesla T4)",
        "url": "https://YOUR-TUNNEL-URL.trycloudflare.com/v1",
        "toolCalling": true,
        "vision": false,
        "maxInputTokens": 32768,
        "maxOutputTokens": 4096
      }
    ]
  }
]
(Replace YOUR-TUNNEL-URL with the URL from Cell 2).

Reload VS Code (Ctrl + Shift + P → Developer: Reload Window).
Open GitHub Copilot Chat, click the model dropdown, and select Qwen 27B (Dual Tesla T4).
