# Lokalny System RAG: Dokumentacja STM32 NUCLEO

Ten projekt to w pełni lokalny system **RAG (Retrieval-Augmented Generation)** zaprojektowany do analizy i odpowiadania na techniczne pytania dotyczące mikrokontrolerów i płytek STM32 NUCLEO. System wykorzystuje lokalne modele językowe (LLM) za pośrednictwem środowiska Ollama oraz wektorową bazę danych FAISS, co gwarantuje 100% prywatności danych i możliwość działania offline.

## ✨ Główne cechy

* **W pełni lokalne działanie:** Brak wysyłania danych do zewnętrznych API (jak OpenAI). Wszystko przetwarza się na Twoim komputerze.
* **Akceleracja GPU:** Wykorzystanie biblioteki PyTorch z obsługą CUDA do błyskawicznego tworzenia wektorów (embeddingów) na kartach NVIDIA.
* **Inteligentne buforowanie (Caching):** System automatycznie zapisuje przetworzoną bazę wiedzy (`FAISS index`) na dysku. Kolejne uruchomienia wczytują bazę w ułamku sekundy, pomijając proces czasochłonnego szatkowania PDF-ów.
* **Precyzyjny model embeddingowy:** Zastosowanie modelu `BAAI/bge-m3`, który doskonale radzi sobie z terminologią techniczną (numery pinów, nazwy rejestrów) oraz językiem polskim.
* **Odporność na halucynacje:** System posiada rygorystyczny prompt (zero-shot), który zmusza model do odpowiadania *wyłącznie* na podstawie dostarczonej dokumentacji.

## 🛠 Technologie

* **Python 3.10**
* **Ollama** (Lokalny serwer LLM)
* **PyTorch + CUDA 12.x** (Obliczenia tensorowe)
* **FAISS** (Wektorowa baza danych od Meta)
* **SentenceTransformers** (Tworzenie embeddingów)
* **PyMuPDF** (Odczyt i parsowanie plików PDF)
* **Jupyter Notebook** (Środowisko wykonawcze)

---

## 🚀 Wymagania wstępne (Prerequisites)

1.  **Karta graficzna NVIDIA:** Zalecana architektura Turing lub nowsza (np. GTX 1650, RTX 2060+) z minimum 4GB VRAM.
2.  **Ollama:** Zainstalowana i uruchomiona w systemie.
3.  **Anaconda / Miniconda:** Do zarządzania środowiskiem wirtualnym.

## 📦 Instalacja

1.  **Sklonuj repozytorium:**
    ```bash
    git clone [https://github.com/Darius-core/AI_zajecia/tree/main/Projekt_RAG](https://github.com/Darius-core/AI_zajecia/tree/main/Projekt_RAG)
    cd TwojeRepozytorium
    ```

2.  **Pobierz modele językowe przez Ollama:**
    Otwórz terminal i pobierz preferowane modele:
    ```bash
    ollama pull llama3.2:latest
    ollama pull nemotron-3-nano:4b
    # Opcjonalnie dla większej ilości VRAM (8GB+):
    # ollama pull deepseek-r1:7b
    ```

3.  **Stwórz i aktywuj środowisko Conda:**
    Wykorzystaj dołączony plik konfiguracyjny, aby uniknąć konfliktów zależności:
    ```bash
    conda env create -f env_gpu.yml
    conda activate rag_gpu
    ```

---

## ⚙️ Struktura projektu

```text
📦 projekt-rag-stm32
 ┣ 📂 knowledge/             # Tutaj wrzuć swoje pliki PDF (dokumentację STM32)
 ┣ 📂 vec_db/                # Folder generowany automatycznie (zawiera bazę FAISS)
 ┃ ┣ 📜 vector_database.index
 ┃ ┗ 📜 metadata.json
 ┣ 📜 projekt_rag.ipynb      # Główny notatnik z kodem systemu RAG
 ┣ 📜 env_gpu.yml            # Konfiguracja środowiska Anaconda
 ┗ 📜 README.md              # Ten plik