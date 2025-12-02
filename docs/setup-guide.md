# Setup Guide

This comprehensive guide will walk you through setting up the project locally, configuring MkDocs, and understanding the automated deployment process.

---

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.7+**: Required for MkDocs
- **pip**: Python package manager (usually comes with Python)
- **Git**: For version control
- **Text Editor**: VS Code, Sublime, or your preferred editor
- **GitHub Account**: For deploying to GitHub Pages

### Verify Your Installation

Check if Python is installed:
```bash
python3 --version
```

Check if pip is available:
```bash
pip3 --version
```

Check if Git is installed:
```bash
git --version
```

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/PouyanNorouzi/COMP4964ProjectDemo.git
cd COMP4964ProjectDemo
```

### 2. Install Dependencies

Install MkDocs and the Material theme:

```bash
pip install mkdocs mkdocs-material
```

Or use a virtual environment (recommended):

```bash
# Create virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate  # On Linux/Mac
# OR
venv\Scripts\activate  # On Windows

# Install dependencies
pip install mkdocs mkdocs-material
```

### 3. Serve Locally

Start the development server:

```bash
mkdocs serve
```

Open your browser and navigate to: **http://127.0.0.1:8000**

You should see the documentation site running locally with live reload enabled!

---

## 📝 Project Structure

Understanding the file organization:

```
COMP4964ProjectDemo/
│
├── .github/
│   └── workflows/
│       └── docs.yml          # GitHub Actions workflow
│
├── docs/                      # Documentation source files
│   ├── index.md              # Homepage
│   ├── introduction.md       # Project introduction
│   ├── setup-guide.md        # This file
│   └── faq.md                # FAQ page
│
├── mkdocs.yml                # MkDocs configuration
└── README.md                 # GitHub README
```

### Key Files Explained

#### `mkdocs.yml`
The main configuration file for your documentation site:

```yaml
site_name: DevOps Docs Automation Demo
theme:
  name: material

nav:
  - Home: index.md
  - Introduction: introduction.md
  - Setup Guide: setup-guide.md
  - FAQ: faq.md
```

#### `.github/workflows/docs.yml`
The GitHub Actions workflow that automates deployment:

```yaml
name: Deploy Docs

on:
  push:
    branches: ["main"]

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.x"

      - name: Install MkDocs
        run: pip install mkdocs mkdocs-material

      - name: Build site
        run: mkdocs build --clean

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

---

## 🛠️ Local Development

### Making Changes

1. **Edit Markdown Files**: Modify files in the `/docs` directory
2. **Live Preview**: Changes appear instantly at http://127.0.0.1:8000
3. **Check Navigation**: Ensure new pages are added to `mkdocs.yml`
4. **Test Links**: Verify all internal links work correctly

### Common MkDocs Commands

```bash
# Start development server with live reload
mkdocs serve

# Start server on different port
mkdocs serve -a localhost:8080

# Build static site (output in ./site directory)
mkdocs build

# Build with clean slate (removes old files)
mkdocs build --clean

# Validate configuration
mkdocs build --strict

# Deploy to GitHub Pages manually (not needed with Actions)
mkdocs gh-deploy
```

### Adding New Pages

1. Create a new `.md` file in the `/docs` directory:
   
   ```bash
   touch docs/new-page.md
   ```

2. Add content to the file:
   
   ```markdown
   # New Page Title
   
   Your content here...
   ```

3. Update `mkdocs.yml` navigation:
   
   ```yaml
   nav:
     - Home: index.md
     - Introduction: introduction.md
     - Setup Guide: setup-guide.md
     - New Page: new-page.md
     - FAQ: faq.md
   ```

4. Save and the page appears immediately in local server!

---

## 🎨 Customizing the Theme

### Basic Theme Configuration

Extend `mkdocs.yml` with theme options:

```yaml
theme:
  name: material
  palette:
    primary: indigo
    accent: indigo
  font:
    text: Roboto
    code: Roboto Mono
  features:
    - navigation.tabs
    - navigation.sections
    - search.highlight
```

### Adding Features

Enable additional Material theme features:

```yaml
theme:
  name: material
  features:
    - navigation.instant      # Instant loading
    - navigation.tracking     # URL updates with scroll
    - navigation.tabs         # Top-level tabs
    - navigation.sections     # Section navigation
    - navigation.expand       # Expand sections by default
    - navigation.top          # Back to top button
    - search.suggest          # Search suggestions
    - search.highlight        # Highlight search terms
    - content.code.copy       # Copy button for code blocks
```

### Custom Colors

```yaml
theme:
  name: material
  palette:
    # Light mode
    - scheme: default
      primary: blue
      accent: light blue
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    # Dark mode
    - scheme: slate
      primary: blue
      accent: light blue
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
```

---

## 🌐 Deployment Setup

### Automated Deployment (GitHub Actions)

The project is already configured for automatic deployment! Here's what happens:

1. **Trigger**: You push changes to the `main` branch
2. **GitHub Actions**: Workflow is triggered automatically
3. **Build**: MkDocs builds the static site
4. **Deploy**: Site is published to `gh-pages` branch
5. **Live**: Changes appear on GitHub Pages within minutes

### Enabling GitHub Pages (First Time)

If setting up a new repository:

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under "Source", select **Deploy from a branch**
4. Select branch: **gh-pages**
5. Select folder: **/ (root)**
6. Click **Save**

### Viewing Your Site

After deployment, your site will be available at:

```
https://pouyanorouzi.github.io/COMP4964ProjectDemo/
```

### Manual Deployment (Alternative)

If you prefer to deploy manually:

```bash
# Build and deploy in one command
mkdocs gh-deploy

# With custom commit message
mkdocs gh-deploy -m "Updated documentation"
```

---

## 🔧 Troubleshooting

### Build Fails in GitHub Actions

**Check the Actions tab** in your repository to see error logs.

Common issues:

- **Missing dependencies**: Ensure `mkdocs-material` is installed in workflow
- **Invalid YAML**: Validate `mkdocs.yml` syntax
- **Broken links**: Fix any references to non-existent files
- **Permissions**: Ensure Actions has write permissions (Settings → Actions → General)

### Local Server Won't Start

```bash
# Check if port 8000 is in use
lsof -i :8000

# Kill the process if needed
kill -9 <PID>

# Or use a different port
mkdocs serve -a localhost:8080
```

### Changes Not Appearing

- **Clear browser cache**: Hard refresh with Ctrl+Shift+R (or Cmd+Shift+R on Mac)
- **Rebuild**: Stop server and run `mkdocs build --clean` then `mkdocs serve`
- **Check file save**: Ensure your editor saved the file

### MkDocs Not Found

```bash
# Ensure MkDocs is installed
pip install --upgrade mkdocs mkdocs-material

# Or if using venv, activate it first
source venv/bin/activate
```

---

## 📚 Additional Resources

### Official Documentation

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)

### Markdown Guide

- [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)
- [GitHub Flavored Markdown](https://github.github.com/gfm/)

### Community & Support

- [MkDocs Community](https://github.com/mkdocs/mkdocs/discussions)
- [Material Theme Discussions](https://github.com/squidfunk/mkdocs-material/discussions)

---

## 🎯 Next Steps

Now that you have the project running locally:

1. **Experiment**: Try editing the Markdown files and see live changes
2. **Customize**: Modify the theme to match your preferences
3. **Deploy**: Push changes and watch GitHub Actions deploy automatically
4. **Extend**: Add new pages and features to the documentation

Happy documenting! 🚀
