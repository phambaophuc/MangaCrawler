# Manga Crawler 🧗

This project is a Python-based manga crawler designed to download manga chapters and images from various online sources. It utilizes a robust architecture involving database management, asynchronous operations, and cloud storage integration.

![Python Version](https://img.shields.io/badge/python-3.12-blue.svg) ![License: None](https://img.shields.io/badge/License-No%20license-red.svg)

## Table of Contents 📜

- [Project Title & Badges](#manga-crawler-f0f0f0)
- [Description](#description)
- [Features](#features-star)
- [Tech Stack](#tech-stack-computer)
- [Installation](#installation-wrench)
- [Usage](#usage-play-button)
- [License](#license-key)
- [Footer](#footer-writing-hand)

## Description 📝

The `manga-crawler` is a sophisticated Python application engineered to automate the process of downloading manga. It connects to a PostgreSQL database to manage manga series, chapters, and image metadata. The crawler supports different online manga sources through a pluggable parser system and can optionally upload downloaded images to Cloudflare R2 storage.

## Features ⭐

- **Automated Manga Downloading** 📥: Downloads manga chapters and images efficiently.
- **Multiple Source Support** 🌐: Extensible parser system to support various manga websites (currently `truyenqq` is implemented).
- **Database Management** 🗄️: Uses Prisma with PostgreSQL to store and manage manga data.
- **Cloudflare R2 Integration** ☁️: Optional upload of downloaded images to R2 for persistent storage and accessibility.
- **Image Optimization** 🖼️: Converts downloaded images to WebP format with configurable quality.
- **Scheduling** ⏰: Configurable cron jobs to run the manga service at specific times.
- **Robust Error Handling** ⚠️: Includes retries for network requests and mechanisms to reset stuck downloads.
- **Docker Support** 🐳: Provides a Dockerfile for easy deployment and environment management.
- **Testing Suite** 🧪: Includes various test files to verify core functionalities.

## Tech Stack 💻

- **Language:** Python 3.12
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Cloud Storage:** Cloudflare R2 (via boto3)
- **Web Scraping:** requests, beautifulsoup4
- **Image Processing:** Pillow
- **Scheduling:** APScheduler
- **Containerization:** Docker

## Installation 🔧

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/phambaophuc/manga-crawler.git
    cd manga-crawler
    ```

2.  **Install Dependencies:**
    The project uses `requirements.txt` for Python dependencies and a `Dockerfile` for environment setup.

    *   **Using Docker (Recommended):**
        Build and run the Docker image:
        ```bash
        docker build -t manga-crawler .
        docker run -d --name manga-crawler-app -p 8000:80 manga-crawler
        ```

    *   **Using Pip (Manual Setup):**
        Ensure you have Python 3.12 and PostgreSQL installed.
        ```bash
        pip install --no-cache-dir -r requirements.txt
        ```

3.  **Database Setup:**
    - Configure your PostgreSQL connection string in the `.env` file (create it from `.env.example`).
    - Ensure Prisma is installed and configured if you are not using Docker's `prisma generate` command.

4.  **Cloudflare R2 Configuration:**
    - Create a `.env` file based on `.env.example`.
    - Fill in your Cloudflare R2 credentials (`R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET_NAME`, `R2_PUBLIC_URL`).

## Usage ▶️

### Running the Crawler

- **Using `main.py`:**
  The `main.py` script sets up a scheduler to run the manga service at specified times.
  ```bash
  python main.py
  ```
  The service is scheduled to run at 00:00 and 20:15 daily.

- **Using `run.py`:**
  The `run.py` script directly starts the manga leech service.
  ```bash
  python run.py
  ```

### Adding Manga Sources

Sources are registered via `ParserFactory.register_parser`. Currently, `truyenqq` is registered in `leecher/__init__.py` and `leecher/service.py`.

To add a new source, you would create a new parser class inheriting from `leecher.base_parser.BaseMangaParser` and register it.

### Example Workflow (Conceptual):

1.  **Add a Manga Source:** The `test/test_add_source.py` demonstrates adding a new source like `truyenqq` to the database.
    ```python
    # In database/leech_manager.py
    await db.add_manga_source(
        name='truyenqq',
        base_url='https://truyenqqgo.com',
        parser_class='TruyenQQParser'
    )
    ```

2.  **Add a Manga Series:** You can add a manga series that the crawler will track.
    ```python
    # In database/leech_manager.py
    await db.add_manga_series(
        source_name='truyenqq',
        title='Example Manga',
        target_url='https://example.com/manga/example-manga'
    )
    ```

3.  **Scheduled Downloads:** The `main.py` script schedules the `run_manga_service` function to run periodically, which then fetches pending series from the database and starts the download process.

## How to use 🛠️

This project is designed to automatically download manga chapters and images. The primary entry points are `main.py` and `run.py`.

- **Automatic Scheduling:** Configure the desired download times in `main.py` using `APScheduler`. The current configuration runs the service twice daily.
- **Cloud Storage Integration:** If Cloudflare R2 is configured (`.env` file), downloaded images will be uploaded to your specified R2 bucket. Otherwise, they will be stored locally in the `manga_storage` directory.
- **Extensibility:** The `ParserFactory` pattern allows easy addition of support for new manga websites by creating new parser classes.

## License 🔑

This project is not under any specified license.

## Footer ✍️

**manga-crawler** • ![GitHub Repo stars](https://img.shields.io/github/stars/phambaophuc/manga-crawler?style=social) ![GitHub forks](https://img.shields.io/github/forks/phambaophuc/manga-crawler?style=social)

**Author:** phambaophuc
**Contact:** (No contact information provided in the repository)

Feel free to fork, star, and open issues on the [GitHub repository](https://github.com/phambaophuc/manga-crawler)!


---
