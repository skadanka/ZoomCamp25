
# 🚀 Zoomcamp Week 1: Introduction to GitHub Codespaces & Docker

## 🐳 Introduction to Docker

Docker is a platform for **developing, shipping, and running applications** inside containers — lightweight, isolated environments that ensure consistency across different systems.

---

## ✅ Verify Docker Installation

Run the following command to confirm Docker is installed and working:

```bash
docker run hello-world
```

This pulls a test image and runs it in a container. If successful, it confirms that Docker is set up correctly.

---

## 🧪 Running Interactive Docker Sessions

You can run containers in **interactive mode** using the `-it` flags:

```bash
docker run -it ubuntu bash
```

- `-i`: Keeps STDIN open (for input)
- `-t`: Allocates a pseudo-TTY (for terminal output)
- `ubuntu`: Image name
- `bash`: Command to run (interactive shell)

> Containers are isolated. Any destructive command, like the one below, **won’t affect your host system**:

```bash
rm -rf / --no-preserve-root
```

---

## 🐍 Running Python in Docker

To start a container with a specific image, e.g., Python 3.9:

```bash
docker run -it python:3.9
```

To override the default entrypoint (to gain shell access):

```bash
docker run -it --entrypoint bash python:3.9
```

From there, you can manually install packages:

```bash
pip install pandas
python
>>> import pandas as pd
>>> pd.__version__
```

> 📝 Remember: changes made inside the container are **not persistent** after exiting unless committed or baked into the image.

---

## 📦 Dockerfile: Custom Image with Pandas

Create a `Dockerfile` to automate your environment setup:

```Dockerfile
FROM python:3.9

RUN pip install pandas

COPY pipeline.py .

ENTRYPOINT ["python", "pipeline.py"]
```

Then build the image:

```bash
docker build -t test:pandas .
```

Run the container:

```bash
docker run -it test:pandas
```

To pass arguments to your script:

```bash
docker run -it test:pandas 2021-01-15
```

---

## 📂 pipeline.py — Sample Entry Script

A simple `pipeline.py` script:

```python
import sys
import pandas as pd

def main(date):
    print(f"Running pipeline for date: {date}")
    print(f"Pandas version: {pd.__version__}")

if __name__ == "__main__":
    date_arg = sys.argv[1] if len(sys.argv) > 1 else "No date provided"
    main(date_arg)
```

---

## 📘 Summary

| Concept | Description |
|--------|-------------|
| `docker run` | Creates and starts a container |
| `-it` | Interactive terminal |
| `--entrypoint` | Override the default command |
| Dockerfile | Script to build container images |
| `ENTRYPOINT` | Specifies command to run when container starts |
| `docker build` | Builds image from Dockerfile |
| `docker run image args` | Runs container with optional arguments |

---

## 🧠 Pro Tips

- **Immutable Containers**: Think of containers as disposable. If something breaks, rebuild.
- **Layer Caching**: Docker caches image layers. Changing a line in Dockerfile may invalidate the cache.
- **Use `.dockerignore`**: Prevent unnecessary files from being copied into the image (like `.git`, `__pycache__`, etc.)
