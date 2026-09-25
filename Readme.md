Quickstart: Run Qwen 27B Free on Dual Tesla T4 GPUs

This setup lets you host Qwen 27B with a 36k context window on Kaggle for free using their 30 hours/week GPU quota, and connect it directly to VS Code Copilot / Agent mode.

Step 1: Upload the Notebook to Kaggle

1. Go to kaggle.com and log in.
2. Click + Create → New Notebook.
3. In the menu bar, click File → Import Notebook.
4. Upload the .ipynb file.

Step 2: Configure the Session Settings

Important: In the right-hand panel under Session options:

- ACCELERATOR: Select GPU T4 x2 (Dual Tesla T4 is required)
- PERSISTENCE: Change from No persistence to Variables and Files
- ENVIRONMENT: Keep the original environment (or Latest)
- INTERNET: Turn ON

Why this matters:
This saves the 16.8 GB model and compiled binaries so you never have to re-download or re-compile on future runs.

Step 3: Run the First-Time Setup

Click Run All (or run the cells in order):

- Cell 1: Confirms that 2 Tesla T4 GPUs are active
- Cell 2: Launches a free Cloudflare tunnel
- Cell 3 & 4: Compile llama.cpp with CUDA support
  - This takes about 2 minutes once and is saved for future use
- Cell 5: Downloads the Qwen 27B GGUF model and starts the inference server

When Cell 5 finishes, you should see:

- FINAL_QWEN_STAGE_PASS
- Health check: 200 OK

Look at the output of Cell 2 to find your Cloudflare tunnel URL:

- TUNNEL_READY https://xxxx-xxxx-xxxx.trycloudflare.com

Step 4: Connect to VS Code

1. In VS Code, press Ctrl + Shift + P and search for:
   Preferences: Open User Settings (JSON)
2. Open or create the configuration file:
   - Linux: ~/.config/Code/User/chatLanguageModels.json
   - Windows: %APPDATA%\Code\User\chatLanguageModels.json
3. Paste the following configuration:

```json
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
```

Replace YOUR-TUNNEL-URL with the actual URL from Cell 2.

Then:

1. Reload VS Code:
   Ctrl + Shift + P → Developer: Reload Window
2. Open GitHub Copilot Chat
3. Click the model dropdown
4. Select: Qwen 27B (Dual Tesla T4)
